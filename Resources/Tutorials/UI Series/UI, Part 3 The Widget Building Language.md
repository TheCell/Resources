---
tags:
  - ui
  - blog
created: 21.04.2025
up: "[[UI Series Table of Content]]"
prev: "[[UI, Part 2 Every Single Frame (IMGUI)]]"
---
[https://www.rfleury.com/p/ui-part-3-the-widget-building-language?utm_source=substack&utm_campaign=post_embed&utm_medium=web](https://www.rfleury.com/p/ui-part-3-the-widget-building-language?utm_source=substack&utm_campaign=post_embed&utm_medium=web)

### Details on what I've learned about building an adequate core widget hierarchy construction API.

I outlined in [Part 1](https://ryanfleury.substack.com/p/ui-part-1-the-interaction-medium) the existence of at least two layers in my style of user interface implementation: **core code** and **builder core**. In [Part 2](https://ryanfleury.substack.com/p/ui-part-2-build-it-every-frame-immediate), I introduced an immediate-mode style API that would help reach certain goals in **builder code**. Now, let’s talk about how we might go about writing the **core code**.

I first introduced the idea that builder code needs to be kept small, simple, and flexible—that’s the place that must be plastic to the design behind all of the user interfaces. The amount of time that goes into iterating on that part of the code is important, because it adds up overtime. The larger the number of design iterations you’re able to perform on a project, the better the design will become (assuming you’re not a major corporation or government agency, which seem to have a tendency to produce increasingly disastrous designs over time).

The core, on the other hand, is the “machinery” that allows builder code to be kept small and flexible. The core code’s size does not grow at any dramatic rate with the number of user interface designs we produce over time. In my experience, core code grows rapidly in size very _early_ in a project as it adjusts to its constraints. The acceleration of its size then decreases rapidly, and it hovers around a fixed size (and, if done well, a fixed _small_ size). It’s still, of course, better to make decisions that keep it small and flexible, because it sometimes requires adjustments, but it’s our “engine” for building user interfaces rapidly, so we only write (and maintain) one such layer in a codebase.

But the core _can_ and _will_ grow in size with respect to supporting several _combinations of features_. When I use the word “feature”, really what I mean is some “effect” produced by a set of codepaths—it’s some end-result on the screen, or some interactive feature. Say you implement N features that each node in the widget hierarchy can have “equipped”. These features might be things like “can be clicked/dragged”, “drawn with a border around it”, “center-aligned text”, “show hot and active animations”, and so on (more on that shortly). This would suggest that there are 2^N possible feature combinations.

_**Note:** N features implying 2^N possible feature-combinations just comes from very basic combinatorics; the intuitive explanation for this is that each feature can either be “inside” or “outside” (2 cases) an arbitrary combination that you build—this multiplies the number of possibilities produced by the introduction of a single feature by 2._

Let’s get a feeling for some of the features that often arise in production user interfaces. The following list is pulled from a project I work on that has relatively complex user interface requirements. It is a set of possible “features” or “effects” that I can apply at each node in a widget hierarchy:

- Clickability—taking mouse events and hovering, responding to clicks and drags
    
- Mouse wheel scrolling to shift the “view offset”
    
- Keyboard focusing behavior—another path for producing “clicks” or “presses” in a keyboard-driven fashion
    
- Being, and appearing, disabled and non-interactive to the user
    
- “Floating” on the X axis—skipping layout on this axis, effectively
    
- “Floating” on the Y axis
    
- Allowing overflow on the X axis—skipping size-constraint-solving on this axis
    
- Allowing overflow on the Y axis
    
- Requiring a drop shadow to be rendered
    
- Requiring a background to be rendered
    
- Requiring a border to be rendered
    
- Requiring text to be rendered
    
- Hot animation visualization
    
- Active animation visualization
    
- Arbitrary “render command buffer” attachment
    
- Center-aligned text
    
- Clipping rectangle for children widgets
    
- The X-axis position being smoothly animated across frames
    
- The Y-axis position being smoothly animated across frames
    
- Bypassing text-truncation with ellipses
    

You’ll notice that some of these are “anti-features”—they are simply skipping codepaths that otherwise would run—this is in essence identical to the feature being explicitly enabled, but it is specified in reverse so that the default case is the common case (better compression of core code API usage—the usage code needs to send less information for the very common case). These are, nevertheless, still features that may or may not need to be applied, depending on the widget in question.

You’ll also notice that these features are not entirely orthogonal. Why not just implement _all of the rendering_ with the “arbitrary rendering command buffer?” This comes down to compression again—standard borders, backgrounds, text, and drop shadows all need to be rendered consistently across the user interface, and it’s just far less wasteful to prepare and send _a single bit_ for each of those features than to send arbitrary rendering commands to produce the same effect many times. The rendering commands may also require specific transformation rules (in order to adjust to a fresh layout—keep in mind that these rendering command buffers are produced with an in-flight widget hierarchy construction, so they cannot rely on entirely up-to-date layout information. To have truly arbitrary rendering specified per-widget that can use a fresh layout every frame, you’d just want to use a rendering function pointer attachment, which would be another feature).

That is an incomplete list, but hopefully you get the basic idea. There are 20 features in the list, which means there are 2^20 = 1,048,576 possible feature combinations. Now, to be clear, not _all of those feature combinations_ are meaningful or useful in all user interface designs. But, what we can say, is that the number of possibly-useful feature combinations is—in a handwavy, correct-for-our-purposes way—O(2^N) when the number of features is N. For anything except a very small constant multiplier on whatever the real function is, that’s _a lot of combinations_.

## Confronting Combinatorics

So why is that important?

It all comes down to the fact that doing O(2^N) programming work when N = 20 _by hand_ is actually just a waste of everyone’s time. Furthermore, it’s not obvious that there is a very small set of useful combinations that we can “fix in place” as the only possibly useful ones. If you’ll recall back to [Part 1](https://ryanfleury.substack.com/p/ui-part-1-the-interaction-medium), I specifically mentioned that “we can’t assume a closed set of widget designs” in the core code for precisely this reason. There _definitely are_ certain combinations that we can elect to being “fast paths” provided by the core API—for example, it would be very commonly useful to have a “make a widget that has text, border, background rendering, hot/active animations, is clickable, and has a specific hovering cursor icon” (effectively your standard, everyday button). But, we should also then immediately realize that an identical button _but without the border_ is not an impossibility or even an improbability.

I feel that this is a very important point to drive home, because it’s very common to see core code designs that try to do something like this:

```
enum UI_WidgetKind
{
  UI_WidgetKind_Null,
  UI_WidgetKind_Button,
  UI_WidgetKind_Checkbox,
  UI_WidgetKind_Slider,
  // ...
  UI_WidgetKind_COUNT
};

struct UI_Widget
{
  UI_WidgetKind kind;
  // ...
  union
  {
    struct { ... } button;
    struct { ... } checkbox;
    struct { ... } slider;
    // ...
  };
};
```

_**Note:** In fact, this is a pattern that shows up all over the place in many domains. I will now explain why this can be a gigantic mistake when dealing with combinatoric explosions of features._

If we look at that code, how do we interpret that within the framework I’ve been building up thus far? For me, the glaring aspect about the above code is that each “widget kind”—`Button`, `Checkbox`, `Slider`, whatever else we might need—is _one particular feature combination_, or a small set of them.

So, if you imagine the gigantic set of over a million feature combinations, these “widget kinds” are more-or-less just “poking into” that set. They’re just picking _one specific combination_ and saying “this is something special”. Now, as I’ve already mentioned, there’s nothing wrong with choosing certain feature combinations to be common fast-paths (and indeed we might call those combinations “buttons”, or “sliders”, or “checkboxes”), but the above code goes a step further and says “this is so special that it needs to have unique data requirements and be _baked into the type information for our widget tree_”.

This design—ultimately born from a strange obsession to use runtime data to encode “what something is” and how its memory should be interpreted—seems to come from what can only be described as the mind virus of object-oriented programming. I don’t intend to be disrespectful to people who often use this pattern—I was for a _very long time_ subject to the aforementioned mind virus, and I think it’d be very useful for others to try ditching it also.

What you can hopefully realize is that each “widget kind” cross-cuts into the feature set with other “widget kinds”. All three of the above “widget kinds” require nearly identical codepaths for clicking and dragging.

The main realization I had about this design, at some point, was “why do I even care what ‘is a button’ or what ‘is a slider’, to the degree that I have to store it at each node in the hierarchy?” At the end of the day, these things are entirely irrelevant to the _basic data transformation needs_ of the program. The fact is, we want certain _high-level features—_clickability, animations, certain rendering characteristics. We want the computer to _do something_. All of these higher level features can be described at a lower level with what data we _have_, what data we _need to produce from that data_, and a function (in the mathematical sense) taking our inputs to our outputs.

What I’ve found to be a much more effective strategy than the above is to _confront the combinatoric explosion of feature-combinations head-on_. Instead, we can prepare up-front for 2^N possible feature combinations by implementing codepaths for only N features.

Sounds awesome, right?! Well, turns out it’s actually pretty boring at the end of the day:

```
typedef U32 UI_WidgetFlags;
enum
{
  UI_WidgetFlag_Clickable       = (1<<0),
  UI_WidgetFlag_ViewScroll      = (1<<1),
  UI_WidgetFlag_DrawText        = (1<<2),
  UI_WidgetFlag_DrawBorder      = (1<<3),
  UI_WidgetFlag_DrawBackground  = (1<<4),
  UI_WidgetFlag_DrawDropShadow  = (1<<5),
  UI_WidgetFlag_Clip            = (1<<6),
  UI_WidgetFlag_HotAnimation    = (1<<7),
  UI_WidgetFlag_ActiveAnimation = (1<<8),
  // ...
};

struct UI_Widget
{
  // ...
  UI_WidgetFlags flags;
  // ...
};
```

Yup, that’s pretty much it—it’s just a mask that controls whether a per-widget codepath (or possibly, but less preferably, a set of codepaths) will run taking a given widget as input. Importantly, the type remains uniformly structured irrespective of which flags happened to be set (as opposed to the discriminated union case, which must be interpreted according to the discriminator). In this style, all fields are “valid”. A feature being introduced simply means that a new—often single and very localized—codepath must be written, and that’s all there is to it. Before feature implementation codepaths, the feature bit is simply tested on all widgets that are potentially being fed into those codepaths.

A mental model that might help explain why this happens to be very powerful goes as follows. Take the following codepath:

```
// do step A
{
  ...
}

// do step B
{
  ...
}

// do step C
{
  ...
}

// do step D
{
  ...
}
```

And the corresponding “feature flags”:

```
typedef U32 StepFlags;
{
  StepFlag_A = (1<<0),
  StepFlag_B = (1<<1),
  StepFlag_C = (1<<2),
  StepFlag_D = (1<<3),
}
```

And some sample `StepFlags` values:

```
0b0000
0b1001
0b1010
0b1111
0b1011
```

You can imagine, visually, projecting each one of those values onto the unified codepath, with each bit corresponding to one of the “steps”. What you ultimately get is a “code mask”, and to control which feature combination you want, you simply choose an integral value with the bits you wanted. There are no types to change, no `switch`es to update, no extra code to write.

Within the context of our `UI_Widget`, we get this same effect (even though the code may not be neatly organized in-order with the flags like that). Whenever we introduce new features, we also ensure that all of our codepaths that _generically_ work over a `UI_Widget` structure (for example, layout and rendering) continue to function gracefully with the new feature (and the new feature combinations introduced by the new feature).

## On Memory Usage

I’ll now address the primary concern I hear whenever I introduce this idea.

> That’s a lot of extra data—discriminated unions help me keep my structure tighter for all cases, whereas in this case, fields will be valid and occupying space even if they aren’t used.

As it turns out, it’s not a lot of data. It’s really just not. Even the most complicated user interfaces will have—almost always—less than 1,000 widgets on the screen at once (and even in complex scenarios, it’s dramatically less than that—the actual number I used to estimate this, that I found in a fairly busy user interface of mine, was 419 active widgets). In this interface, the size of my widget node structure is 392 bytes—I just throw stuff I need into that structure without thinking about it. So, just to be extra conservative with our calculations, let’s assume 512 bytes per node, and 1,024 active nodes. The total amount of storage we need to manage all of that data (without any attempts at compressing it) is 512 bytes * 1024 = 512 KiB = 0.5 MiB. On modern machines—and indeed even machines from decades ago—this is just an unremarkable amount of storage. In all modern cases this will fit entirely into the L2 cache (with plenty of room to spare!), and it will occupy around 0.0031% of working physical system memory. It will occupy around 0.00000018% of your available virtual address space.

So, really, don’t worry about it. Doing simple back-of-the-napkin calculations like this can show how little data you need to do many wonderful things, and thus how little work you need to do to stay reasonably fast and small, thanks to amazing hardware advancements in the past several decades. All of the bloat, sluggishness, and unreliability we find in modern software is actually _not_ because of decisions like the one I am advocating for right now, which is simply allowing some extra wiggle room in the type system at the expense of a miniscule amount of wasted space that will have no ultimate impact on the user. Instead, it is often decisions that introduce decades of legacy software cruft into a system and assume that this will somehow be more secure, more reliable, more thought-through, and more performant than writing something from scratch.

## Rubber, Meet Road

Let’s iron out some specifics. If you’ll recall from [Part 2](https://ryanfleury.substack.com/p/ui-part-2-build-it-every-frame-immediate), I outlined some details on what `UI_Widget` would need in order to encode both hierarchy, and links into a cross-frame persistent cache table. I also outlined some basic per-widget data we’d need for simple animations, and a simple offline autolayout algorithm. Now, we also know we can allow our widgets to appear and act differently with feature flags. Here’s that whole picture so far:

```
struct UI_Widget
{
  // tree links
  UI_Widget *first;
  UI_Widget *last;
  UI_Widget *next;
  UI_Widget *prev;
  UI_Widget *parent;

  // hash links
  UI_Widget *hash_next;
  UI_Widget *hash_prev;
  
  // key+generation info
  UI_Key key;
  U64 last_frame_touched_index;

  // per-frame info provided by builders
  UI_WidgetFlags flags;
  String8 string;
  UI_Size semantic_size[Axis2_COUNT];

  // computed every frame
  F32 computed_rel_position[Axis2_COUNT];
  F32 computed_size[Axis2_COUNT];
  Rng2F32 rect;

  // persistent data
  F32 hot_t;
  F32 active_t;
};
```

A very simple API that we can offer as the lowest level widget hierarchy building tools might look something like this:

```
// basic key type helpers
UI_Key UI_KeyNull(void);
UI_Key UI_KeyFromString(String8 string);
B32 UI_KeyMatch(UI_Key a, UI_Key b);

// construct a widget, looking up from the cache if
// possible, and pushing it as a new child of the
// active parent.
UI_Widget *UI_WidgetMake(UI_WidgetFlags flags, String8 string);
UI_Widget *UI_WidgetMakeF(UI_WidgetFlags flags, char *fmt, ...);

// some other possible building parameterizations
void UI_WidgetEquipDisplayString(UI_Widget *widget,
                                 String8 string);
void UI_WidgetEquipChildLayoutAxis(UI_Widget *widget,
                                   Axis2 axis);

// managing the parent stack
UI_Widget *UI_PushParent(UI_Widget *widget);
UI_Widget *UI_PopParent(void);
```

And finally, because this is an immediate-mode API and we allow the builder code to interleave input event consumption and regular procedural code with the widget hierarchy build, all we need is a place to implement actual user interaction with a widget:

```
// still searching for a good name for this concept.
// effectively it's a name for "interaction result",
// which is just a bucket for information about all of
// the ways that a user interacted with a widget on
// the current frame. but I get so tired of typing
// "interaction" or "interact result" or something,
// so I'll try calling it "comm" this time, short for 
// "communication from the user".
//
// let me know if anyone has any ideas...
struct UI_Comm
{
  UI_Widget *widget;
  Vec2F32 mouse;
  Vec2F32 drag_delta;
  B8 clicked;
  B8 double_clicked;
  B8 right_clicked;
  B8 pressed;
  B8 released;
  B8 dragging;
  B8 hovering;
};

// and finally, the call that implements all interaction
// for all widgets - "get the user communication from
// this widget":
UI_Comm UI_CommFromWidget(UI_Widget *widget);
```

## Common Case Helpers

That’s looking not quite like the original pitch of just calling `UI_Button`, though. In fact, we did a lot of work to try and eliminate the concept of “button” altogether, and to only implement codepaths relevant to what we actually wanted the computer to do. So what gives?

Interestingly, when reframing a problem this way, it turns out that “button” stops being a “widget kind” or “subtype”, and instead becomes only a helper function that implements a very common case.

An actual implementation of `UI_Button` may look like this:

```
B32 UI_Button(String8 string)
{
  UI_Widget *widget = UI_WidgetMake(UI_WidgetFlag_Clickable|
                                    UI_WidgetFlag_DrawBorder|
                                    UI_WidgetFlag_DrawText|
                                    UI_WidgetFlag_DrawBackground|
                                    UI_WidgetFlag_HotAnimation|
                                    UI_WidgetFlag_ActiveAnimation,
                                    string);
  UI_Comm comm = UI_CommFromWidget(widget);
  return comm.clicked;
}
```

In reality, I generally ditch returning `B32` here, and just return the whole `UI_Comm`, so my button calls turn into this:

```
if(UI_ButtonF("Foo").clicked)
{
  // the button was clicked!
}
```

This makes helper functions like `UI_Button` compose much more seamlessly with the various requirements of a builder codepath—the entire return channel being occupied by “was the button clicked?” is just too wasteful, and you can throw a lot of other information into that channel that is frequently useful.

The other widgets I introduced previously in the series can be implemented in similar ways. `UI_Button` is the easiest case, of course, but the other basic standardized widgets are not too far of a stretch.

## Conclusion

The important part about all of that is this: while in many cases, builder code _will_ want to use calls like `UI_Button`, in a large number of other cases, it will want to have a tighter grip on what exactly it is building. This is why exposing the lower level core API and keeping that small, simple, and constrained, is still a massive win for even the highest-level codepaths in very application-specific code. Those high-level codepaths can still render exactly what they need, and get exactly the right behavior—“feature combinations”—they want, _or_ they can opt-in to some standard common cases (which they could’ve implemented trivially themselves—and indeed they may want to do so, if they have a very common widget that deviates from whatever the core decided was useful to provide).

That wraps up my overview on the basic structure of a satisfactory core code layer.

Thanks for reading, and hope you enjoyed! More coming soon.