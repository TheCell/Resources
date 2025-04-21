---
tags:
  - software
  - utility
created: 21.04.2025
up: "[[Code]]"
---
[https://github.com/freemocap/freemocap](https://github.com/freemocap/freemocap)

![[307898342-da1af7fe-f808-43dc-8f59-c579715d6593.png]]
#### A free-and-open-source, hardware-and-software-agnostic, minimal-cost, research-grade, motion capture system and platform for decentralized scientific research, education, and training

![[Pasted image 20250421234120.png]]
![[Pasted image 20250421234131.png]]

## QUICKSTART
#### 0. Create a a Python 3.10 through 3.12 environment (python3.11 recommended)¶
#### 1. Install software via [pip](https://pypi.org/project/freemocap/#description):

```
pip install freemocap
```

#### 2. Launch the GUI by entering the command:

```
freemocap
```

#### 3. A GUI should pop up that looks like this:
![[239695690-90ef7e7b-48f3-4f46-8d4a-5b5bcc3254b3.png]]

#### 4. Have fun! It might break! Work in Progress lol

#### 5. [Join the Discord and let us know how it went!](https://discord.gg/nxv5dNTfKT)

---

## Install/run from source code (i.e. the code in this repo)

[](https://github.com/freemocap/freemocap#installrun-from-source-code-ie-the-code-in-this-repo)

Open an [Anaconda-enabled command prompt](https://www.anaconda.org) (or your preferred method of environmnet management) and enter the following commands:

1. Create a `Python` environment (Recommended version is `python3.11`)

```shell
conda create -n freemocap-env python=3.11
```

2. Activate that newly created environment

```shell
conda activate freemocap-env
```

3. Clone the repository

```shell
git clone https://github.com/freemocap/freemocap
```

4. Navigate into the newly cloned/downloaded `freemocap` folder

```shell
cd freemocap
```

5. Install the package via the `pyproject.toml` file

```shell
pip install -e .
```

6. Launch the GUI (via the `freemocap.__main__.py` entry point)

```shell
python -m freemocap
```

A GUI should pop up!

---

## Documentation
Our documentation is hosted at: [https://freemocap.github.io/documentation](https://freemocap.github.io/documentation)

That site is built using `writerside` from this repository: [https://github.com/freemocap/documentation](https://github.com/freemocap/documentation)

---

### Contribution Guidelines
Please read our contribution doc: [CONTRIBUTING.md](https://github.com/freemocap/freemocap/blob/main/CONTRIBUTING.md)

## Maintainers
- [Jon Matthis](https://github.com/jonmatthis)
- [Endurance Idehen](https://github.com/endurance)