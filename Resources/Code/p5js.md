---
tags:
  - javascript
  - software
  - utility
  - processing
created: 24.04.2025
up: "[[Code]]"
---
[https://github.com/processing/p5.js](https://github.com/processing/p5.js)

p5.js is a free and open-source JavaScript library for [accessible](https://p5js.org/contribute/access) creative coding. It is a nurturing community, an approachable language, an exploratory tool, an accessible environment, an inclusive platform, welcoming and playful for artists, designers, educators, beginners, and anyone else!

```javascript
function setup() {
  createCanvas(400, 400);
  background(255);
}

function draw() {
  circle(mouseX, mouseY, 80);
}
```

![[p5-readme-sketch.png]]

## About

p5.js is built and organized to prioritize [accessibility, inclusivity, community, and joy](https://p5js.org/community). Similar to sketching, p5.js has a full set of tools to draw. It also supports creating audio-visual, interactive, experimental, and generative works for the web. p5.js enables thinking of a web page as your sketch. p5.js is accessible in multiple languages and has an expansive [documentation](https://p5js.org/reference/) with visual examples. You can find [tutorials](https://p5js.org/tutorials/) on the p5.js website and start coding right now in the [p5.js web editor](https://editor.p5js.org/). You can extend p5.js with many community-created [libraries](https://p5js.org/libraries/) that bring different capabilities. Its community provides endless inspiration and support for creators.

p5.js encourages iterative and exploratory code for creative expression. Its friendly, diverse community shares art, code, and learning resources to help elevate all voices. We share our values in open source and access for all, to learn, create, imagine, design, share and code freely.

## Community

The p5.js community shares an interest in exploring the creation of art and design with technology. We are a community of, and in solidarity with, people from every gender identity and expression, sexual orientation, race, ethnicity, language, neuro-type, size, disability, class, caste, religion, culture, subculture, immigration status, age, skill level, occupation, and background. We stand in solidarity with justice and liberation movements. We work to acknowledge, dismantle, and prevent barriers to access p5.js code and the p5.js community.

Learn more about [our community](https://p5js.org/community/) and read our community statement and [code of conduct](https://github.com/processing/p5.js/blob/main/CODE_OF_CONDUCT.md). You can directly support our work with p5.js by donating to [the Processing Foundation](https://processingfoundation.org/support).

## 🌼 p5.js 2.0 Now Available for Community Testing & Development!

We are releasing p5.js 2.0 to the community for testing and development! Here’s what you need to know.

- For **reference**: p5.js 1.x reference will stay on [https://p5js.org/](https://p5js.org/), and p5.js 2.x documentation will be on [https://beta.p5js.org/](https://beta.p5js.org/)
- In the p5.js Editor: the **default will continue to be 1.x** until at least August 2026 - more information and discussion on timeline can be found on [this Discourse thread](https://discourse.processing.org/t/dev-updates-p5-js-2-0-you-are-here/46130) or [this GitHub thread](https://github.com/processing/p5.js/issues/7488)
- For updating sketches and add-on libraries: check out [the compatibility add-on libraries and guides](https://github.com/processing/p5.js-compatibility)
- For **contribution**: `npm latest` will default to 2.x, but the git branches are still separated with `main` on 1.x and `dev-2.0` on 2.x. We will switch the branches when we have updated all automations (including deploying updated documentation to the website). Want to contribute ideas or implementation? Check the [2.x project board](https://github.com/orgs/processing/projects/21/views/8) for an overview of what still needs discussion, and what’s ready for work!