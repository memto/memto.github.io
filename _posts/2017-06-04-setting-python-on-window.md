---
layout: post
ads: true
comments: true
published: true
title: Setting python working environment on window
categories:
  - program
  - tooltips
tags:
  - python
  - window
  - python2x-3x
---

#### Download and install python 2x, 3x
- Download both python 2x, 3x from [python download](http://bluenik.com/2YAP) page
- The installation steps on window are easy, just click next then next.... please notice the installation path
- Adding installed python **scripts** directory into System path. This folder contains command files for certain pakages (ex: pip, virtualenv)
1. Go to start menu > right click on computer > properties > advanced system settings > Environment variables 
2. Create new variables: PYTHON2_SCRIPTS (C:\Python27\Scripts), PYTHON3_SCRIPTS (C:\Python36-32\Scripts)
3. Add newly created varibale into Path: append to the end %PYTHON2_SCRIPTS%;%PYTHON3_SCRIPTS%

#### Working with mutiple version of python on window
After above steps we have both python 2x, python 3x installed in system.
![python-installed.png]({{site.baseurl}}/media/python-installed.png)

- How to specify which python version to run from cmd?

Use python launcher. From python 3x, python has a launcher called **py.exe** which you can specify python version to run (ex: py -2, py -3)
> usage: py [ launcher-arguments ] [ python-arguments ] script [ script-arguments ] <br>
py -h for more information <br>

- How to specify which pip version to run?

python already have separate pip version for each as in picture above. one more step that I did is change the **pip** from both to **pip-bak** to not wrongly use it without specify version. you can keep **pip** from which version if you want it will be the default.

#### How to use pip
```
Usage:
	pip <command [options]
pip -h for more help
```

Some common usage
- get help for specific pip command <br>
``` 
pip help install 
```

- search for package
```
pip search virtualenv 
```

- install package <br>
``` 
pip install virtualenv
pip install virtualenv==x.y # install specific version of package
```

- uninstall package
```
pip uninstall virtualenv
```

- show information about package
```
pip show virtualenv
```

- install multiple package from a requirements.txt file
```
pip install -r requirements.txt
```

- create requirements.txt file
``` 
pip freeze > requirements.txt
pip freeze --local > requirements.txt # if in virtualenv
```

- uninstall multiple package from a file (requirements.txt)
```
pip uninstall -r requirements.txt
```

- list installed package
``` 
pip list
pip list -o # list outdated package
```

- upgrade outdated package to latest <br>
```
pip install -U virtualenv
```


#### Install virtualenv package and setting a python virtual environment
For why we should use virtualenv you can check here: [Virtual Environments](http://atominik.com/3NGJ). The main idea is to have isolate working environment for each of your python project.

Because of functional are the same, you can install virtualenv for either python 2.x or 3.x (3.x for me). You can install it using **pip** as below:
- pip3 install virtualenv

Now you can use virtualenv to create a working environment for your project
- for python 3.x project: virtualenv -p C:\Python36-32\python.exe venv36
- for python 2.x project: virtualenv -p C:\Python27\python.exe venv27

Using newly created environment:
- venv27\Scripts\activate to start working
- python -V to check python version
- deactivate to stop working

#### Pycharm setup on window
