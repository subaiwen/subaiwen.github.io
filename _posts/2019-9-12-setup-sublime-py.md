---
title: "Set up Sublime Text 3 for Python"
layout: post
date: 2019-09-12 21:58
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- Python
- Sublime
category: blog
author: subaiwen
description: Sublime for Python
---

## Packages and Setup
- [Sublime Text 3 Setup - Most Important Packages](https://www.youtube.com/watch?v=oHmPrjSzmwU)
- [Setting Up Sublime Text 3 for Full Stack Python Development](https://realpython.com/setting-up-sublime-text-3-for-full-stack-python-development/)

---

## Type Checker: mypy
#### 1. Install mypy
[Documet](https://mypy.readthedocs.io/en/latest/getting_started.html)

```bash
python -m pip install mypy-lang
```
Alternatively, you can

```bash
pip install mypy-lang
```

#### 2. Test mypy
```bash
mypy test.py
```
Error pops up:

```
Traceback (most recent call last):
  File "//anaconda3/bin/mypy", line 10, in <module>
    sys.exit(console_entry())
  File "//anaconda3/lib/python3.7/site-packages/mypy/__main__.py", line 4, in console_entry
    main()
TypeError: main() missing required argument 'script_path' (pos 1)
```
[Solution](https://github.com/python/mypy/blob/master/mypy/__main__.py):

```python
main(None, sys.stdout, sys.stderr)
```

#### 3. Install Anaconda
Package Control -> Install Package -> Anaconda

#### 4. Configure Anaconda
Preferences -> Package Settings -> Anaconda -> Settings-User

```
{

    "python_interpreter": "/usr/bin/python",
    "anaconda_linting_behaviour": "load-save",
    "anaconda_linter_phantoms": true,
    "mypy": true,
}
```

#### 5. SublimeLinter-contrib-mypy
[Instruction](https://github.com/fredcallaway/SublimeLinter-contrib-mypy):  
(1) Package Control -> Install Package -> SublimeLinter  
(2) Package Control -> Install Package -> SublimeLinter-contrib-mypy  

---

## Run Python
[Run Python3 on Sublime Text 3](https://medium.com/@hariyanto.tan95/set-up-sublime-text-3-to-use-python-3-c845b742c720):  
Tools -> Build System -> New Build System:
#### UNIX
```
{
 "cmd": ["/usr/local/bin/python3", "-u", "$file"],
 "file_regex": "^[ ]File \"(...?)\", line ([0-9]*)",
 "selector": "source.python"
}
```

#### WINDOWS
```
{
 "cmd":["C:/Users/<user>/AppData/Local/Programs/Python/Python37-32/python.exe", "-u", "$file"],
 "file_regex": "^[ ]File \"(...?)\", line ([0-9]*)",
 "selector": "source.python"
}
```
Save this file as new-python.sublime-build

Go to Tools -> Build System -> Python  
Press **CTRL + B** to run the code on Sublime

## Terminal
[Terminus](https://packagecontrol.io/packages/Terminus)


