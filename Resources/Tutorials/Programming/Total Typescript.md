---
tags:
  - typescript
  - course
created: 21.04.2025
up: "[[Tutorials]]"
---
[https://github.com/total-typescript/total-typescript-book](https://github.com/total-typescript/total-typescript-book)

![[68747470733a2f2f7265732e636c6f7564696e6172792e636f6d2f746f74616c2d747970657363726970742f696d6167652f75706c6f61642f76313639393032323631302f626f6f6b2d6769746875622d62616e6e65725f32785f7a31616869742e706e67.png]]

## Quickstart
### Install PNPM
Because this course is _so big_ we're using `pnpm` as the package manager. It's like `npm`, but results in fewer `node_modules` saved to disk.

[Install `pnpm` globally](https://pnpm.io/installation).

### Install Dependencies
# Installs all dependencies
pnpm install

# Asks you which exercise you'd like to run, and runs it
pnpm run exercise

## How to take the course
You'll notice that the course is split into exercises. Each exercise is split into a `*.problem` and a `*.solution`.

To take an exercise:

1. Run `pnpm exercise`
2. Choose which exercise you'd like to run.

This course encourages **active, exploratory learning**. In the video, I'll explain a problem, and **you'll be asked to try to find a solution**. To attempt a solution, you'll need to:

1. Check out [TypeScript's docs](https://www.typescriptlang.org/docs/handbook/intro.html).
2. Try to find something that looks relevant.
3. Give it a go to see if it solves the problem.

You'll know if you've succeeded because the tests will pass.

**If you succeed**, or **if you get stuck**, unpause the video and check out the `*.solution`. You can see if your solution is better or worse than mine!