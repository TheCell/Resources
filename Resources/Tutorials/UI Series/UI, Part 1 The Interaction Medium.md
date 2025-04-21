---
tags:
  - ui
  - blog
created: 21.04.2025
up: "[[UI Series Table of Content]]"
---
[https://www.rfleury.com/p/ui-part-1-the-interaction-medium?utm_source=substack&utm_campaign=post_embed&utm_medium=web](https://www.rfleury.com/p/ui-part-1-the-interaction-medium?utm_source=substack&utm_campaign=post_embed&utm_medium=web)

### Introducing a set of concepts, principles, goals, and ideas that have dramatically improved my ability to write production-quality user interfaces from scratch. Base knowledge for later parts.

[Ryan Fleury](https://substack.com/@ryanfleury)

May 08, 2022

You navigated to this blog post in a web browser, so chances are, you’re familiar with software user interfaces. For users, they’re everywhere on a modern computer. And, when I say “everywhere”, I actually do mean **everywhere**—they are the only thing that a user ever actually interacts with. And yes, I’m talking to you, Arch Linux hermit, tightly gripping your IRC client. Even if you’re the kind of person that likes working with textual data in a terminal, you still interact with a software user interface, just a remarkably simplified and uniform one, which happens to have standardized characteristics across many programs, instead of being invented per-program.

The ubiquity of user interfaces in the modern computing world means that it’s useful to be able to easily develop them. But it’s also easy to let that get wildly out-of-hand. For many people, writing and maintaining a user-interface quickly becomes an explosion of complexity and time, which might be better spent on other parts of their projects.

I’ve iterated a lot on my approach to building and maintaining user interfaces over the past many years, and I believe I’ve learned and discovered much that can be useful for others who are looking to build their own user interfaces from scratch, so I’ve collected it into this blog series.

## The User/Programmer Information Cycle

Let’s start by framing our understanding of user interfaces.

Imagine that you’ve just been hired as the operator of a machine. You have a great coffee chat with your coworkers on your first day. You talk with them about something safe and inoffensive, like the weather, and shortly after you head off to the machine to start operating it. When you sit down to operate the machine, you see ten buttons.

But there’s a problem. You have no idea what the buttons do. In fact, you have no idea what the machine does. It’s not actually clear how you were hired at all, but never mind that for now. You were definitely sure that this machine did something really useful for the company that hired you, but that was about all you understood. Needless to say, this is not an operational scenario—something is broken.

Now, imagine another similar scenario. You’re the top of your field in machine operation. Similar to the last scenario, you’re hired, have a coffee chat with your coworkers on your first day, and sit down to start operating your machine. There are ten buttons on the machine. This was your element. You know these ten buttons like the back of your hand. You grew up around these ten buttons; they were almost like a father to you. You crack your knuckles, and start pressing those ten buttons.

But there’s a problem. The machine seems to starts wildly malfunctioning; each button does something entirely different than what you expect. It seems almost as though the buttons have been wired improperly, or they are mismatched. Needless to say, this is also not an operational scenario.

These two scenarios outline two different ways in which an interface—the ten buttons—is rendered unusable. It happens either when the _user_ fails to understand how the interface maps to outputs, or when the machine fails to understand how the user’s inputs maps to certain operations. The operational scenario is, of course, when the user understands the buttons, and they are wired up to the expected operations within the machine. In other words, in the working scenario, the _user_ of the machine and the _manufacturer_ of the machine have an _agreement_ about the structure of the interface.

Once we’re within a working scenario and this agreement has been established, we now have the ability to _communicate_ from one “side” of the interface to the other. If we’re the user, we can press our buttons with confidence. If we’re the manufacturer, we can interpret button presses correctly. When a button is pressed, the user _communicates_ some information. The machine can properly interpret that information, and communicate some information back (for example, through labeled lights, in this physical machine scenario), which can then be appropriately interpreted by the user.

Software user interfaces are fundamentally no different; you simply need to replace the word “manufacturer” with “programmer”, and replace physical buttons with software buttons on a computer screen. When you’re using a program and you click a button, drag a slider, or use your mouse wheel to scroll around, you communicate information, and when the visual representation on the screen changes, the program communicates information back.

Now that we know, on some fundamental level, _what_ a user interface is, let’s start considering the constraints that we can use to form a good one, which I’ll use to frame all parts of this series.

## The Cost of Transmitting Information

Okay, so user interfaces exist to transmit information. But there’s an additional concern that we need to consider, if we want to build a user interface: sending and receiving information is not free. And if we’re not careful, we can waste a lot of time and energy if we aren’t careful about achieving a good (small) ratio of **bits sent** to **useful bits received**.

To solidify this idea, let’s imagine the extreme case. Consider a program with a checkbox that, when using the program, you very frequently check or uncheck. But, to check or uncheck the checkbox, you can’t simply click it, like you might be used to. Instead, you must type the sentence: “Hello, Mr. Program. I would like you to toggle the checkbox, please.” The “please” is important—this program was developed by a member of the P.E.T.S. (People for the Ethical Treatment of Software) organization, and they believe in asking for the consent of the program nicely, instead of simply forcing your will upon the program by just clicking the checkbox.

There are many strings of that size (or shorter) that you could typed. You can replace each character at each position in that string with many other possible characters. So, this design forces the user to send _at least_ 68 bytes individually, where each byte corresponds to one press of a letter key. If you’re a fast typer, you might finish that in 5-10 seconds. But how much useful information is _extracted_ from that? The answer is: however much it takes to encode _which checkbox was toggled_, and that the checkbox was toggled. How much information that is exactly depends, but needless to say, requiring the user to type a 68-character-long string for this is massively wasteful in all practical scenarios.

A much better equivalent to typing this string (obviously) would be to have a special keybind that toggles this checkbox, in which case it’s a single keypress to express that information. The more common, and still better, equivalent is for the user to position their mouse over the representation of the checkbox on the screen (which encodes which one is selected), and for them to both press and release their mouse after that.

But what makes those options “better”? The answer should be obvious: it wastes less of the user’s time, because the user can spend less time encoding less information for an identical effect. It achieves a better **bits sent** to **useful bits received** ratio, and it achieves this in less time.

I use this extreme example to demonstrate a model that can allow us to (to some degree, and somewhat vaguely) consider how good, or how bad, certain choices can be when we set up our interfaces. Within this model, when we build a user interface, we have the goal of achieving the smallest possible **bits sent** to **useful bits received** ratio, and the goal of achieving the smallest possible time spent by the user, and the goal of achieving the smallest possible time spent by the program. Of course those three goals (and several other goals) can be in conflict with each other, which is when you must make decisions about tradeoffs. But understanding the _ideal_ goals is useful, because we can then understand what is _preferable_.

## The Common Language of User Interfaces

Another goal you may have is to minimize how much time the user needs to spend _learning_ about quirks in your own software. How much can they reuse existing knowledge, having worked with other programs?

You might have found a much better widget design that, when properly understood and used, wastes much less time than alternative designs. But if this widget design is very complicated and entirely unique to your program, and if the user very rarely needs to interact with this widget, it might actually waste the user’s time more than alternative, standardized options that they are already familiar with.

Software user interfaces have existed for a long time, and before that, there were physical interfaces that inspired the first software user interfaces, so there is a very long history that has led to a common set of widgets that the average user of a computer will be familiar with. Here’s a quick (non-exhaustive) catalogue of some of them:

- **Buttons**. These are widgets that the user hovers with their mouse (or finger). They then press the left mouse button (or their finger), then release it, and that causes an ‘event-like’ codepath to run once.
    
- **Checkboxes**. These are similar to buttons, but are specifically for the user toggling a boolean state. The boolean value is visualized with the button somehow, and clicking the button flips the state.
    
- **Radio Buttons**. Similar to checkboxes, but are used for a set of **mutually-exclusive** states. Within a set of radio buttons, only one may correspond to a true value.
    
- **Sliders**. These are often used for changing a numeric value. The user hovers them, presses down, and continues moving their mouse (or finger) to change the value along some axis.
    
- **Text Fields**. These are simply widgets that allow the user to input a string by typing, with a number of very standard keyboard shortcuts. They also allow the user to click and drag to select text.
    
- **Scroll Bars**. Not everything can fit on a computer screen at one time. These widgets can be clicked and dragged to move the ‘camera’ for a certain region on the screen, and view more information.
    

When you build a user-interface, these (and many others) are tools that you will very often need. Implementing them properly allows users to more quickly grasp how a user interface works, which will save them time; they are like a common, standardized “interface language” that users and programmers can easily rely on to communicate.

## The Importance of Appearance and Animation

There’s an entire body of knowledge about designing an interface (both physical and digital) to be quickly understandable, which is unquestionably one constraint we must consider when building user interfaces. My favorite book on the subject is _The Design of Everyday Things_ by Don Norman. Fairly early in the book, Norman discusses a concept called “signifiers”. These are hints, or clues, that a designer of a user interface provides the user, that communicate (ideally through non-linguistic means) _how_ certain objects are to be interacted with.

We have a number of signifiers that often go along with the aforementioned standard widgets, and we can introduce plenty more to help users of our interfaces. This can be as simple as mimicking the appearance of a physical button when rendering a digital button in your software user interface, by making it look embossed. In software, it’s also important to respond to user input that does not necessarily directly interact with a widget. For example, when the user hovers their mouse cursor over a button, it’s very common to transition the button’s appearance to appearing _more embossed_. When the user presses down, it then transitions the button’s appearance to appearing _debossed_, to mimic the behavior of a physical button.

You can get a long way by just instantaneously flipping between appearances, and a UI may still be very easily readable by the user. But this can be further improved by employing animation, which smoothly represents a change in state to the user, at a rate that the human brain can interpret (instead of a change happening near-instantaneously, from one frame to another).

So, when putting together a software user interface, it’s crucial that we have a plan for a variety of appearances that dynamically respond to user action (including user action that does not directly interact with a widget), and it’s even better if we also have a plan for animating those appearances. We’ll dive into exactly how this is going to work later, but for now, we’ll establish these as constraints on a suitable user interface solution.

## Minimizing Code Size and Complexity

This is all starting to sound like a lot of work! And, indeed, this is why user interface code often spirals out of control. Our entire user interface, and really our entire program, will suffer if managing our user interfaces is massively difficult, and gets more difficult overtime. Therefore, we cannot discount both code size and complexity as an important constraint on our solution. Ideally, we can spend as little time, and as little code, building and maintaining our user interfaces as possible, and keep the code that we _do_ require as simple, and as robust-to-change, as possible.

Importantly, interfaces rarely stay the same across the entirety of a project’s development, and it’s very common to require a new interface for a new purpose. If the process of changing a user interface, or introducing a new one, is high-friction, it may become a worse development bottleneck than development of the actual features that the user interface exposes.

So, how do we start compressing our code size, and reducing our code complexity?

We can start by realizing the fact that we do not want to write many things on a per-widget basis. If you can imagine rewriting clicking, rendering, animation, and layout behavior for each individual button, perhaps you can also imagine spiraling downward into a deep state of depression. Needless to say, all of that can be pulled out into common codepaths. We still must specify _some information_ on a per-widget basis.

What follows from this is that our solution is subdivided into at least two parts. We will have a **core**, which implements common codepaths and helpers, and **builder code**, which will use the core and will be in charge of arranging _instances_ of widgets to produce interfaces for various purposes.

The **builder code**, in totality, will be much larger than the **core code**, and it will need to change more frequently. Therefore, it’s crucial to make **builder code** be as small and easily-controllable as possible, and it’s less of a disaster to have more complexity in the **core** (but, of course, it’s better to not introduce complexity without reason).

## Escape Hatches

Our **core** can provide common codepaths and helpers for our **builders**, and it can even provide fastpaths for certain widget designs. Earlier, I mentioned the de-facto standardized “common language” of user interfaces, comprised of widgets like buttons, checkboxes, sliders, and so on. The **core** can provide very easy options for **builder code** to instantiate these widgets in a given user interface design.

But we can now introduce another constraint to our solution. It’s almost certain that, when we write the core, we cannot assume we know the total and closed set of widget designs that builder code will need. If you use sufficiently complicated programs, you’ll know that some interfaces often require something very specialized for a certain purpose. A 3D modeling program cannot implement its primary editor with only buttons, sliders, checkboxes, and other standardized widgets. Therefore, from the **core**, we must also provide **builder code** with the ability to access what I call an “escape hatch”, which is a method by which usage code of an API can dispense with API-provided common codepaths, and take more control by composing its own code with a smaller subset of said API-provided common codepaths.

## Conclusion

Now that we’ve explored the shape of the user interface problem, and understood some of the constraints on any solution we write for it, we can start getting specific about how to write some code for one. This is a natural stopping point for now, though, so I’ll leave it at that.

Thanks for reading! I hope you enjoyed this introduction to user interfaces. Stay tuned for the next one.