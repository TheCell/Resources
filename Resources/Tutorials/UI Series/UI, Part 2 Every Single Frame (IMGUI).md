---
tags:
  - ui
  - blog
created: 21.04.2025
up: "[[UI Series Table of Content]]"
prev: "[[UI, Part 1 The Interaction Medium]]"
---

[https://www.rfleury.com/p/ui-part-2-build-it-every-frame-immediate?utm_source=substack&utm_campaign=post_embed&utm_medium=web](https://www.rfleury.com/p/ui-part-2-build-it-every-frame-immediate?utm_source=substack&utm_campaign=post_embed&utm_medium=web)

### Introduces the concept and structure of an "immediate mode user interface" and how we can use it to accomplish our goals, including strategies for autolayout, animation, and keying.

[Ryan Fleury](https://substack.com/@ryanfleury)
May 09, 2022

In [Part 1](https://ryanfleury.substack.com/p/ui-part-1-the-interaction-medium?s=w), I outlined my framework for thinking about user interfaces, and established a set of goals, constraints, and ideas that inform my approach to building one.

One of these ideas was that our code for implementing a user interface would be subdivided into at least two parts, or “layers”. One layer is the **core**, which implements common codepaths, helpers, and fastpaths for very common widgets. The other layer is the **builder code**, which implements actual, concrete user interfaces, using the code provided by the core. And, importantly, the **builder** code is where the worst explosion of both code size and complexity can occur, because it is much higher in volume than **core** code, and it must be changed more frequently. Thus, one of the goals I established was ease of writing and maintaining **builder code**.

Ideally, the code that it takes to construct a single widget should be entirely localized and as small as possible, so if we ever _add_ or _remove_ widgets from our user interface design, we only have a very small, local place in the code to modify. In the case where we fail to achieve this, modifying a user interface design would require modification across several locations in the codebase. It'd be very easy to forget about one of those locations, which could cause both bugs and accumulation of dead code. So I posit that it's best to avoid this issue altogether.

In user interface APIs in the wild, this property is often not achieved. For example, how do you specify to the API that you'd like a button with certain text, _and also_ what should happen when that button is clicked? You'll often see APIs handle this through user-provided event callbacks that are called when a button is clicked, or with event messages that are later processed by the user. This API design is segmented; you distinctly have **interface building code**, and **interaction response code**, which are entirely separated.

To make things worse, these APIs are often segmented in more than just those two parts. Not only is there an _interface building_ codepath, there is also one (or several) _interface modification_ codepaths. This is because these APIs expose widget construction through what is known as a “retained-mode API”, where each widget's lifetime is _individually_ managed. If you do not want a button to be visible to the user, you must explicitly _remove it_ from the widget hierarchy (or set a hidden flag, or something akin to that), and when you’d like it to be visible again, you must explicitly _insert it_ back into the widget hierarchy (or unset a hidden flag, etc.).

This is not acceptable, given the goals and constraints I’ve laid out. But there is another option, which is often given the name of _immediate-mode user interfaces_.

## The Immediate Mode User Interface

It's unclear to me when or where the first immediate mode interface was written, but it was _popularized_ in recent years by [Casey Muratori’s video on the subject](https://caseymuratori.com/blog_0001), in which he outlines an API design in which all code relating to one widget instance is entirely localized. The code for a button—its text, appearance, what happens when you click it, and everything else—is entirely local to one spot.

```c++
if(UI_Button("Hello, World!"))
{
  // this code runs when this button is clicked!
}
```

This same pattern can apply to every widget design.

```c++
UI_Slider(&my_float, 0.0f, 100.0f, "My Float");
UI_Checkbox(&my_bool, "My Bool");
UI_Radio(&radio, 0, "Radio 0");
UI_Radio(&radio, 1, "Radio 1");
UI_Radio(&radio, 2, "Radio 2");
UI_ColorPicker(&color, "Color");
```

The widget hierarchy is constructed on every frame of an application's runtime, instead of being a stateful tree that must be carefully managed and mutated. This means it gracefully responds to changes in the hierarchy, which can be easily encoded:

```c++
UI_Checkbox(&show_buttons, "Show Buttons");
if(show_buttons)
{
  UI_Button("Foo");
  UI_Button("Bar");
  UI_Button("Baz");
}
```

In the above example, the buttons with `Foo`, `Bar`, and `Baz` for text will only show up if the `Show Buttons` checkbox is toggled. We did not have to write any code for the case where we have those three buttons visible, and we need to _mutate_ the widget hierarchy to remove or hide them, or the reverse. Instead, this design assumes that there could _always be a mutation_, and reconstructs the widget hierarchy every frame, from scratch, through a unified codepath, so it is always correct and requires no code to carefully mutate a persistent stateful widget hierarchy.

That looks pretty manageable, compared to the aforementioned APIs that require all of this code to be segmented. And, notably, this API design fits our goals fairly well. It shrinks and centralizes the code for each widget instance, meaning our **builder** code gets smaller, and unified. Both _widget building_ code, _widget hierarchy modification_ code, and _widget interaction response_ code exists in the same location, and is succinct.

## Layout

So that’s all well and good, but there seems to be a bunch of missing information here. Where do these widgets show up on screen? In fact, isn't that actually necessary information to _implement_ a call like `Button`? Wouldn’t we need to check the mouse position against the actual on-screen rectangle coordinates of the button widget? So where is that information going to come from?

These are all valid questions, which all are really asking about the problem of _layout_. Unsurprisingly, there are a number of ways to answer them.

One trivial layout solution, which is perhaps the simplest for the _core code_, is to simply manually specify the rectangle coordinates for each widget. So, instead of:

```c++
if(UI_Button("Foo"))
{
  // clicked
}
if(UI_Button("Bar"))
{
  // clicked
}
```

We’d instead have:

```c++
if(UI_Button(0, 0, 200, 30, "Foo"))
{
  // clicked
}
if(UI_Button(0, 30, 200, 60, "Bar"))
{
  // clicked
}
```

That's pretty unfortunate for our **builder code**, though; adding a new button to the middle of a user interface might require adjusting coordinate values for anything coming after it (because they would need to be shifted, to avoid overlapping with the added button).

But maybe that problem can be mitigated by just pulling out a layout offset:

```c++
float h = 30;
float y0 = 0;
float y1 = y0 + h;
if(UI_Button(0, y0, 200, y1, "Foo"))
{
  // clicked
}
y0 += h;
y1 += h;
if(UI_Button(0, y0, 200, y1, "Bar"))
{
  // clicked
}
y0 += h;
y1 += h;
```

That is maybe a bit better for maintenance than specifying each pixel coordinate directly, but it has added some noise to our builder code, and has only given us a solution for very simple, unidirectional layouts. We’re also repeating ourselves a lot for each widget. Keep in mind that these issues compound for each user interface we have builder code for, and smaller inconveniences will therefore add up.

We can keep going, though.

```c++
UI_Layout layout = UI_MakeLayout(...);
if(UI_Button(&layout, "Foo"))
{
  // clicked
}
if(UI_Button(&layout, "Bar"))
{
  // clicked
}
```

That’s a bit better. We still have an extra parameter that we repeat, but that’s not too big of an issue. The decision of how tall buttons are, and how each widget is laid out, has been pulled out into a common codepath, and is called into in the implementation of `UI_Button`.

If we want to get rid of the `layout` parameter, we can simply notice that for any given codepath, it's very likely that one `layout` parameter is the same as that which came before it, or that which comes after it. Thus, we can instead phrase the API as having a “selected layout”, which can just be a global or thread-local contextual state:

```c++
UI_Layout layout = UI_MakeLayout(...);
UI_SelectLayout(&layout);
if(UI_Button("Foo"))
{
  // clicked
}
if(UI_Button("Bar"))
{
  // clicked
}
```

That cleaned up the API for each widget a bit, though possibly at the expense of ease of differentiating something per-widget. But, differentiation is more rare than uniformity in a user interface, so I would argue this is overall more preferable (even though it’s not free of tradeoffs).

It’s still not quite sufficient, though. To understand why, let’s look at examples of some real, complicated user interface designs.

![[8c0e36d9-466b-4b2e-b4e7-8c0a432949a9_244x199.png]]

![[990a32cb-afdd-451e-90e8-52961e826464_890x813.png]]

![[d2df56d5-e071-4233-b83c-d41a8ea50c92_1091x132.png]]


All of the above user interfaces have something in common: they all can be encoded _hierarchically_, which I’ve shown with the red rectangles. I’ve already briefly mentioned the fact that widgets being hierarchical is an important constraint for sufficiently complex user interface designs (and “sufficiently complex” is not particularly complex; almost all programs, including simple ones, that you use, arrange their interfaces in some kind of a hierarchy).

What this means is that any given layout is _nested_ within another (or, alternatively, it’s the root layout).

To start accounting for this, we cannot simply _select a layout_, and forget which layout was already selected. We need to _return_ to our “parent layout”, after we’re done building one subtree of widgets. The natural data structure we can use for walking a hierarchy and returning to a parent is a stack, which allows us to make our layouts hierarchical:

```c++
UI_Layout layout = UI_MakeLayout(...);
UI_PushLayout(&layout);
if(UI_Button("Foo"))
{
  // clicked
}
if(UI_Button("Bar"))
{
  // clicked
}
UI_PopLayout();
```

Now `UI_PushLayout` can use the _current_ selected layout to carve out a space for our _new_ layout, which should be nested within the current selected layout.

I’ll go one step further. The hierarchy we are dealing with is not _simply_ of layouts; it’s also of a number of other features. For example, a window in our user interface may need to have a clipping rectangle for any widgets that are “inside” it, or we may want some subtree in the hierarchy to be scrolled by a scrollbar. Furthermore, it’s possible we want to apply certain widget-like behavior to the entirety of a layout; for example, if a user clicks the background of a window, we might want that to have some special behavior (like selecting the window, or reordering it to be visible over other windows), which sounds awfully similar to the clicking behavior of a button.

For this reason, I’d reframe the hierarchy to being of _purely widgets_, meaning a _parent of widgets_ is no fundamentally different from a button widget. Following from this, any types we use to construct this hierarchy will be entirely uniform, which means also that any codepaths we have that rely on these types—for example, the codepath that implements our standardized clicking behavior—can be applied to any level of the hierarchy, including parents of widgets.

With that in mind, our naming, and the type of `layout`, will change:

```c++
UI_Widget *parent = ...;
UI_PushParent(parent);
if(UI_Button("Foo"))
{
  // clicked
}
if(UI_Button("Bar"))
{
  // clicked
}
UI_PopParent(parent);
```

## Fitting In An Offline Autolayout Algorithm

This all seems great, but this whole “layout” thing seems kind of fishy. How exactly is that all working?

In short, it depends. A very unsophisticated layout algorithm does not need to do anything fancy. It may simply do what the initial examples did: assume a fixed size for each button, and advance on a particular axis by the size for that axis. In such a scenario, you can know on the very first frame where a button is on the screen in rectangular coordinates.

In my experience, however, this type of layout algorithm is often insufficient, and easily building even slightly-more complex user interfaces with this is a pain to manage, which is—again—a hindrance on our **builder code**, which I’ve already said needs to be kept small and simple.

In many cases, it’s far simpler and more robust to specify widget sizes in a more “semantic” way. For example, take the following user interface:

![[af002ebc-a538-46d0-aa32-2c1538fd1bcf_845x34.png]]

How might you describe the arrangement of buttons for this interface to someone else? And, following from that, how do you _think_ about the layout? Probably not by calculating the exact pixel sizes and coordinates of each button. Instead, you might describe the interface in the following way, starting from the left:

- Begin a row that fills the available space (one descent in the hierarchy).
    
- File button, sized just enough to fit the text, with some extra padding.
    
- Window button, sized the same way.
    
- Panel button, sized the same way.
    
- View button, sized the same way.
    
- Control button, sized the same way.
    
- Enough space to fill the screen, leaving enough room for the following buttons.
    
- “Play” button, only being big enough for the “Play” icon.
    
- “Pause” button, the same size as the previous.
    
- “X” button, the same size as the previous.
    
- End the row (one ascent in the hierarchy).
    

The reason I call the above example a more “semantic” version of sizing and layout description is because nowhere above did I give a specific number that determines the exact size of each button, or the spacing. And, it’s more ideal for the associated _builder code_ to be specified on exactly the same terms. I say this because each size I described above depends on a number of factors, none of which we want to care about in every builder codepath: What font is being used? How large of a space are these buttons occupying? How much padding should we add to each textual width?

Obviously, specifying all of these details manually is possible with a very simple layout algorithm. But, we want a way to specify the “semantic sizes” without needing to manually do calculations based on font, allotted space, padding, and so on.

But there is a big problem here. Take the `UI_Button` example. When that call happens, we need to know the rectangular coordinates of the button _immediately_. How, then, can we use an autolayout algorithm that depends on widgets specified _after_ this button? In effect, you only have _a partial_ widget hierarchy at the time that this button construction happens. In fact, in order to use the higher-level specifications I mentioned above, we’d _need_ parts of the widget tree that we don’t “have”—at that point in the frame—yet.

(For what it’s worth, the problem here—specified in other terms—is that we want an _offline_ algorithm for autolayout, but we don’t have all of the data we need at the point in time that we need the algorithm to run.)

There’s a way we can work around this problem, though.

What necessarily happens in an immediate mode user interface is that rendering for the widget hierarchy is _deferred_ until a later stage in the frame. This happens for a very important reason, which is that—in the way that 2D rendering normally works—the order that the widget hierarchy needs to be _rendered_ is actually the _reverse_ of the order in which the widget hierarchy is specified. Widgets that _consume input events first_ must be _rendered last_ (on top).

The structure of a frame, then, is this:

1. Build widget hierarchy (one pass)
    
2. Render (separate pass)
    

The way we can fit an “offline autolayout” algorithm into the picture comes by making a tradeoff decision: accepting a _single frame of delay_ for the rectangles that we use for _consumption of events_, while _preserving_ no-frame-delay for the final rendering of each frame. We can do this by updating our frame’s structure to this:

1. Build widget hierarchy (use last frame’s data)
    
2. Autolayout pass (produce fresh layout data)
    
3. Render (use up-to-date layout data)
    

This is an acceptable decision, in my estimation, for a number of reasons.

Firstly, interfaces are only moving around from frame-to-frame when the positions of widgets are animating. Animations should be concise, explanatory, and simple. They are not intended to be actively happening while the user is _simultaneously_ trying to begin interacting with the animating widgets. An interface that a user is actively interacting with should be static across time, so that it remains predictable and easy to work with.

Secondly, in order for a user to—say—even position their mouse over a button, press down on their mouse, then release (which itself takes many frames), _they must have first been presented a frame accurately depicting the widget’s position anyways_. This makes ensuring that the initial set of rectangles used to _consume input events_ is 100% up-to-date with everything that will happen in the frame a useless exercise that prohibits building the best API.

The rendering of the user interface, altogether, remains fully up-to-date every frame.

## A Sample Offline Autolayout Algorithm

Our frame now has a big box that we can fill with code to do a fully offline autolayout algorithm. It can use the entire widget hierarchy to whatever extent it wants. It can be a very complex autolayout algorithm, and can be as sophisticated as anything that worked on a retained-mode widget hierarchy.

There are probably a number of options you can choose for this algorithm, but I’ve developed a relatively-simple one that has gotten me very far.

First, let’s look at how “semantic sizes” are expressed in this algorithm:

```c++
enum UI_SizeKind
{
  UI_SizeKind_Null,
  UI_SizeKind_Pixels,
  UI_SizeKind_TextContent,
  UI_SizeKind_PercentOfParent,
  UI_SizeKind_ChildrenSum,
};

struct UI_Size
{
  UI_SizeKind kind;
  F32 value;
  F32 strictness;
};
```

This `UI_Size` type can encode a number of options for a widget’s size on a single axis.

- `UI_SizeKind_Pixels` allows us to encode a direct size in pixels.
    
- `UI_SizeKind_TextContent` allows us to encode that we’d like the size to be determined by the required dimensions to display whatever string is attached to the widget in question.
    
- `UI_SizeKind_PercentOfParent` allows us to encode that we’d like a certain percentage value of the parent widget’s size on the same axis.
    
- `UI_SizeKind_ChildrenSum` allows us to encode that the size on a given axis should be computed by summing the sizes of children widgets when they are laid out in order.
    

The `value` can be used to express meaningful content in all of those cases except for `UI_SizeKind_ChildrenSum` (in which case, the value is simply unused). The `strictness` is used in the autolayout algorithm. In short, it encodes “what percentage of the final computed size value do you _refuse to give up?_” This becomes important in our algorithm when we’d like to _adjust the size_ of certain widgets when space runs out. A pixel-perfect size of `100` but with a strictness of `0.0` would be “okay with” being reduced to `100*0 = 0`. If the strictness were `1.0`, then the size _must necessarily remain_ at `100`.

To encode the full “semantic size” of a widget, you’d just need one on two axes:

```c++
enum Axis2
{
  Axis2_X,
  Axis2_Y,
  Axis2_COUNT
};

struct UI_Widget
{
  // ...
  UI_Size semantic_size[Axis2_COUNT];
  // ...
};
```

I’ll talk more about the `UI_Widget` type shortly—it will be the way we build the widget hierarchy as a data structure, which is important for a number of reasons, as well as having a data structure to use as input to our offline autolayout algorithm. But for now, let’s focus on the autolayout.

Now that we can encode a “semantic size”, let’s add a few more members to `UI_Widget`:

```c++
// recomputed every frame
F32 computed_rel_position[Axis2_COUNT];
F32 computed_size[Axis2_COUNT];
Rng2F32 rect;
```

In the above code, `computed_rel_position` is the computed position _relative to the parent position_. `computed_size`, perhaps unsurprisingly, is the computed size _in pixels_. Finally, `rect` is just the final on-screen rectangular coordinates that are produced when taking into account the former two values, as well as those of the rest of the hierarchy.

Even though these things are all in a single type (`UI_Widget`), they form—in effect—the input (the semantic size) and the output (the computed position, size, and final rectangle) of the autolayout algorithm. The `rect` member can then be used in both _input event consumption_ on the frame immediately following that of the autolayout pass, as well as the _rendering pass_ of the current frame. This is because `UI_Widget` doubles as a _cache_, and an _immediate-mode data structure_. On the following frame, the `UI_Widget` correlated from the _previous frame_ can be used, with its hierarchical placement (and thus the entire hierarchical structure) being potentially reorganized. Despite the fact that these `UI_Widget`s are being cached as if it is a “retained-mode” data structure, the _API_ remains immediate-mode.

So, now that we’ve defined and set up the inputs and outputs to the autolayout algorithm, I’ll briefly describe each step to actually doing the work. Each stage of the algorithm iterates over the widget hierarchy in a recursive, depth-first fashion. Pay careful attention to which stages require pre-order iterations or post-order iterations.

For each axis:

1. (Any order is acceptable) Calculate “standalone” sizes. These are sizes that do not depend on other widgets and can be calculated purely with the information that comes from the single widget that is having its size calculated. (`UI_SizeKind_Pixels`, `UI_SizeKind_TextContent`)
    
2. (Pre-order) Calculate “upwards-dependent” sizes. These are sizes that strictly depend on an ancestor’s size, other than ancestors that have “downwards-dependent” sizes on the given axis. (`UI_SizeKind_PercentOfParent`)
    
3. (Post-order) Calculate “downwards-dependent” sizes. These are sizes that depend on sizes of descendants. (`UI_SizeKind_ChildrenSum`)
    
4. (Pre-order) Solve violations. For each level in the hierarchy, this will verify that the children do not extend past the boundaries of a given parent (unless explicitly allowed to do so; for example, in the case of a parent that is scrollable on the given axis), to the best of the algorithm’s ability. If there is a violation, it will take a proportion of each child widget’s size (on the given axis) proportional to both the size of the violation, and `(1-strictness)`, where `strictness` is that specified in the semantic size on the child widget for the given axis.
    
5. (Pre-order) Finally, given the calculated sizes of each widget, compute the relative positions of each widget (by laying out on an axis which can be specified on any parent node). This stage can also compute the final screen-coordinates rectangle.
    

_**Note:** Even though I’m expressing the algorithm as going over a single, fat node type, and the fact that it must iterate recursively, this does not imply either **(a)** that the algorithm must literally use recursive functions, nor **(b)** that the data must actually be stored in a single type. To optimize the autolayout algorithm codepath, you’d want to apply simple data-oriented principles in organizing the input and output data channels for the problem. Frankly, however, I never bother to do that, because one property of a good user-interface is that there just aren’t really that many widgets on the screen. So, it’s very unlikely for it to be a problem with realistic user interface scenarios (and I have never found it to be so, in many real scenarios)._

That covers the general overview of the algorithm. It doesn’t solve everything for you. For example, it does not do anything related to wrapping, and instead leaves that up to builder code. I have extended this algorithm with wrapping capabilities in the past, but it adds a fair amount of complexity to each stage, and it is a rare-enough case that I haven’t bothered since. But, aside from that, it provides a (in my experience) fairly useful, basic set of building blocks that allow a very large number of high-level semantic-sizing scenarios.

## Immediate Mode Build, + Cache

For many immediate mode APIs, data structures are rebuilt on every pass of the code. There is no continuation between the last time the code ran, and the current time it is running. A great example of this would be an immediate mode rendering API:

```c++
DrawSprite(...);
DrawShadow(...);
DrawRectangle(...);
DrawText(...);
```

It’d be awfully weird if the _last frame’s draw calls_ were somehow influencing the _current frame’s_ draw calls. For our user interface requirements, however, that is more-or-less exactly what we need.

Ignore the problem of an offline autolayout algorithm, which is already one obvious reason why persistent widget data caching is necessary. If you ever want any persistent per-widget data that is not passed down by the builder code on every build, then you now require a persistent, cross-frame cache that is able to store that data. It’s of course _possible_ to move all state storage to being the responsibility of usage code. But in this case, “usage code” is “builder code”, which we intend to keep small, flexible, and simple. So, it’s often required (in keeping with our goals) to introduce more state-management responsibility to the _core code._

One example of per-widget data that you’d (very often) like implicit (as opposed to explicitly managed by builder code) is animation data. Any data that is required for, say, a button’s hover, press, smoothly-moving, or smoothly-scrolling effects can fall into this category very easily.

_**Note:** Extra animations are always possible in builder code when required. But nearly 100% of all animations I find myself needing can be boiled down to two simple concepts:_ `hot_t` _and_ `active_t`. _These are both persistent floats that are stored per-widget. The_ `_t` _is just my naming convention for denoting that they’re for “transitions”, but the names “hot” and “active” come from Casey Muratori’s original video that I linked earlier. They are just values that smoothly animate across frames, that eventually match the question of “is this widget hot?” or “is this widget active?”. I more-or-less always recommend self-correcting exponential animation curves for animating these two values, because their sharp initial motion fits with the user’s expectation of instantaneous feedback._

Imagine, first, that the `UI_Widget` type was purely an immediate-mode data structure that was rebuilt every frame, and we had no requirement of caching per-widget data across frames. As explained earlier, this data structure must encode an n-ary tree (to adequately encode the widget hierarchy). A very simple way to do that is with the following members:

```c++
struct UI_Widget
{
  UI_Widget *first;
  UI_Widget *last;
  UI_Widget *next;
  UI_Widget *prev;
  UI_Widget *parent;
  // ...
};
```

The important members of this structure are `first` and `next`. Those two members are capable of expressing the entire hierarchy. The other pointers are simply useful pointers that point in other directions (`prev` and `last` form a doubly-linked-list for children, so it’s easy to iterate backwards; `last` is also useful when “appending” to a widget’s children; `parent` is useful for iterating upwards the hierarchy). They are not required, just very often useful, and the alternative (e.g. finding a parent without `parent`) is possibly worse.

_**Note:** Ultimately, this is just a simple binary tree, just with different semantic meanings for each link on a node. Instead of_ `left` _and_ `right`_, it’s_ `first` _and_ `next`_. For whatever reason, binary trees were always presented to me as the former, and never the latter, so it was an embarrassingly-large realization to me one day that n-ary trees could just be obviously done this way. I’m passing on this information now, even if it’s obvious, because sometimes obvious information is the most difficult to notice!_

With that structure, it should be fairly easy to imagine writing the core code that builds the per-frame hierarchy from scratch.

Now, ignore that structure. Imagine that `UI_Widget` is instead the type that is _only_ used for caching persistent data across frames. What does that look like?

```c++
struct UI_Key { ... }; // some keying mechanism

struct UI_Widget
{
  UI_Widget *hash_next;
  UI_Widget *hash_prev;
  UI_Key key;
  U64 last_frame_touched_index;
  // ...
};
```

This is just one simple way to do it, but the basic idea is to just throw these into a hash-table, keyed by `key`. At the end of every frame, if a widget’s `last_frame_touched_index < current_frame_index` (where, on each frame, the frame index increments), then that widget should be “pruned”.

So, if we just want _both_ of these capabilities in a single data structure, we just combine those two requirements into a single structure:

```c++
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

  // ...
};
```

On every frame, the `tree links` section will be rewritten from scratch for the entire hierarchy. The `hash links` are used to look up the persistent part of the structure every frame.

## Keying Strategies

So what is a `UI_Key` and how can we produce those? This is another one of those “it depends” moments. Each strategy has a number of pros and cons. They all need to solve one problem: correlating one frame’s call-site with another frame’s call-site.

This is not as trivial as it first may seem. Many people will initially try to cleverly use source code coordinates (e.g. `__LINE__` and `__FILE__` in a C macro expansion) as a way to generate keys. But that, then, raises the question of what happens here:

```c++
for(int i = 0; i < 100; i += 1)
{
  UI_Button("I am a button!");
}
```

It’s also possible to just have the builder code specify keys manually, and use whatever makes sense to it. This is possible, but inflates and complicates builder code.

Ultimately, I’ve settled on using a strategy that comes from the very popular [Dear ImGui library](https://github.com/ocornut/imgui). This strategy just involves generating keys by hashing the passed string for widgets that require cross-frame persistence. It’s slightly more complicated in that you can adjust the hashing rules by forming the string differently. In short:

- Anything after a `##` is hashed, but not displayed
    
- If a `###` occurs in the string, then only everything after it is hashed, and only anything before it is displayed
    

Once you learn that mechanism, then the above example fairly simply extends to:

```c++
for(int i = 0; i < 100; i += 1)
{
  UI_Button("I am a button!##%i", i);
}
```

Which isn’t, ultimately, much worse.

It’s also possible to have rules that change the way the hash is seeded. In the above example, it may be interesting to try seeding the hash by the previous sibling’s seed. It’s also often desirable to seed the hash with the parent key’s hash as well. There’s a tradeoff to these tricks, however, which is that it becomes more difficult to generate a key “out-of-context”, but this may be mitigated by offering an explicit “opt-out-of-hash-seed-tricks” path in the API.

One final approach is to use another pointer that is provided to you as the key, or something derivative of it. For example, if a pointer to a `B32` is passed to a `UI_Checkbox` call, then that pointer may be reliable as a way to uniquely identify that specific checkbox. I don’t prefer this approach, personally, because it enforces specific types in trivial cases. If my `UI_Checkbox` call only takes a `B32*`, then it’s much higher friction to use it to modify anything that isn’t a `B32` (e.g., a flag in a bitset). For that reason, I don’t prefer nor recommend this approach, but it may be a useful trick to keep around in some cases.

Unfortunately, I don’t know of a strategy here that ends up being perfect and avoiding all issues. But, at the end of the day, the simple solutions are just not high-friction enough to be a deal breaker.

## Conclusion

That was hopefully a useful introduction to immediate mode user interface API design, as well as some of the useful tricks and ideas that I’ve gathered while writing them over the years. I’ve tried to address a number of the concerns that I often hear when discussing this topic throughout the article, but feel free to comment if there are other concerns or questions you have.

I am of the opinion that, in current programming environments and with current programming languages, immediate mode user interface APIs get closer to achieving the goals and constraints I outlined in Part 1. Someday, we will probably find a way to do things in a better way. But for now, the techniques I’ve presented have been very useful to me, and I hope they’re useful to you also.

This is another natural stopping point, but I’ve got a lot more to say on this subject, so stay tuned for Part 3.

Thanks for reading!