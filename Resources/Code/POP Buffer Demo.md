---
tags:
  - protocol
  - 3d
created: 21.04.2025
up: "[[Code]]"
---
[https://github.com/bhaettasch/pop-buffer-demo](https://github.com/bhaettasch/pop-buffer-demo)

This is a sample implementation for the POP Buffer introduced by the team of X3DOM in 2013. It consists of a simple WebGL renderer and a small writing component that creates POP Buffer binaries from obj files.

Attention: This is currently **Work in Progress**. The implementation currently **does not fullfill the specification of the pop buffer**.

POP Buffer:
- [http://x3dom.org/pop/](http://x3dom.org/pop/)

X3DOM:
- [https://github.com/x3dom](https://github.com/x3dom)
- [http://x3dom.org](http://x3dom.org)

## Example
Live Demo: [https://bhaettasch.github.io/pop-buffer-demo/](https://bhaettasch.github.io/pop-buffer-demo/)

![[level_raw.png]]
![[level_middle.png]]
![[level_finer.png]]
![[level_finest.png]]

Bunny model with i) 1410 ii) 5826 iii) 78876 and iv) 208884 of overall 208890 vertices (reordered) and active vertex clustering.