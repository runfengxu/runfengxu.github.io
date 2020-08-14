---
layout:     post
title:      Python Notes
subtitle:   
date:       2020-07-14
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - Python
---

https://docs.python-guide.org/dev/virtualenvs/
## Python Virtual Environment

### virtual environment
`$ pip install virtualenv`

test your installation

`$ virtualenv --version`

>This does a user installation to prevent breaking any system-wide packages. If pipenv isn’t available in your shell after installation, you’ll need to add the user base’s binary directory to your PATH.
- **On Linux and macOS** you can find the user base binary directory by running python -m site --user-base and adding bin to the end. For example, this will typically print ~/.local (with ~ expanded to the absolute path to your home directory) so you’ll need to add ~/.local/bin to your PATH. You can set your PATH permanently by modifying ~/.profile.
- **On Windows** you can find the user base binary directory by running py -m site --user-site and replacing site-packages with Scripts. For example, this could return C:\Users\Username\AppData\Roaming\Python36\site-packages so you would need to set your PATH to include C:\Users\Username\AppData\Roaming\Python36\Scripts. You can set your user PATH permanently in the Control Panel. You may need to log out for the PATH changes to take effect.

create virutalenv

`$ virtual my_name`

Now after creating virtual environment, you need to activate it. Remember to activate the relevant virtual environment every time you work on the project. This can be done using the following command:

`$ source virtualenv_name/bin/activate`

then you can install whatever packages you need into this virtual env. And the pakcage will be isolated from the complete system. and once you done you can deactivate.

`deactivate`

### pipenv  : project level virtual environment

`$ pip install --user pipenv`

INstall packages for your project

`$ cd project_folder`

`$ pipenv install requests`

Pipenv will install the library and create a Pipfile for you in your5 project's directory. The pipfile is used to track which dependencies your project needs 