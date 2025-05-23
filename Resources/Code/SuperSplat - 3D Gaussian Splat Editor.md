---
tags:
  - software
  - utility
  - gaussiansplat
created: 21.04.2025
up: "[[Code]]"
---
[https://github.com/playcanvas/supersplat](https://github.com/playcanvas/supersplat)

| [SuperSplat Editor](https://superspl.at/editor) | [User Guide](https://github.com/playcanvas/supersplat/wiki) | [Forum](https://forum.playcanvas.com/) | [Discord](https://discord.gg/RSaMRzg) |

SuperSplat is a free and open source tool for inspecting, editing, optimizing and publishing 3D Gaussian Splats. It is built on web technologies and runs in the browser, so there's nothing to download or install.

A live version of this tool is available at: [https://playcanvas.com/supersplat/editor](https://playcanvas.com/supersplat/editor)
![[402293223-b6cbb5cc-d3cc-4385-8c71-ab2807fd4fba.png]]

To learn more about using SuperSplat, please refer to the [User Guide](https://github.com/playcanvas/supersplat/wiki).

## Local Development
To initialize a local development environment for SuperSplat, ensure you have [Node.js](https://nodejs.org/) 18 or later installed. Follow these steps:

1. Clone the repository:
    
    ```shell
    git clone https://github.com/playcanvas/supersplat.git
    cd supersplat
    ```
    

Install dependencies:

```shell
git submodule update --init
npm install
```

Build SuperSplat and start a local web server:

```shell
npm run develop
```

1. Open a web browser at `http://localhost:3000`.
    

When changes to the source are detected, SuperSplat is rebuilt automatically. Simply refresh your browser to see your changes.

When running your local build of SuperSplat in Chrome, we recommend you have the Developer Tools panel open. Also:

1. Visit the Network tab and check `Disable cache`.
2. Visit the Application tab, select `Service workers` on the left and then check `Update on reload` and `Bypass for network`.