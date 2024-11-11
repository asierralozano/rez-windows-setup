# rez-windows-setup
# How to setup REZ in a windows environment
## Basic setup
- Install REZ using the official docs
- Install [rez-quickstart-win](https://github.com/instinct-vfx/rez-quickstart-win)
- In a terminal (CMD or Powershell), execute `rez-quickstart --python-version "PYTHONVERSION" --release`. This will build 4 main packages for you on the release packages path:
  - os
  - platform
  - arch
  - python
- Lets bind the missing core packages. Execute the following commands:
  - rez: `rez bind -r --no-deps rez`
  - setuptools: `rez bind -r --no-deps setuptools`
- At this point, you should be able to resolve a simple `python` environment, and test that you have access to it (`rez env python -- python`)

## rez-pip and rez-pip2
There is a new alternative to the old rez-pip, that aims to be more functional. I'm talking about [this](https://github.com/JeanChristopheMorinPerso/rez-pip/tree/__update_pip__?tab=readme-ov-file) one. I've tried installing the package `PySide6`, and I got some issues, so I'm not sure how functional it is right now. I'll keep an eye on it

## PySide6
After several tries, I end up installing `PySide6` in a shared folder, and creating a simple package that appends that folder to the `PYTHONPATH` env variable.

`package.py`
```python

name = "PySide6"

version = "6-native"

description = \
    """
    Python binding of the cross-platform GUI toolkit Qt.
    """

requires = [
    "python-3.11+"
]

build_command = False

uuid = "recipes.pyside6"

def commands():
    env.PYTHONPATH.append(r"D:\path\to\shared\folder\pyside6")
    # Qt.py support
    env.QT_PREFERRED_BINDING = "PySide"

_native = True
```
