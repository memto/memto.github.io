---
layout: post
ads: true
comments: true
published: true
title: Flask getting started
categories:
  - program
tags:
  - python
  - python tutorial
  - flask
---
Ref
> http://flask.pocoo.org/docs/0.12/installation/

Environment setting:
> Have both python 2.x and 3.x on machine = create a virtualenv to using python 3.x <br>
virtualenv and virtualenvwrapper-win both are installed for 3.x <br>
WORKON_HOME=D:\working\projects\learning\python\virtualenv <br>

Create virtualenv
> mkvirtualenv -p C:\Python36-32\python.exe flasktutor

Install flask
> pip install flask

This will install _flask_ and _flask related_ packages from this (https://www.palletsprojects.com/)
```
pip freeze
click==6.7
Flask==0.12.2
itsdangerous==0.24
Jinja2==2.9.6
MarkupSafe==1.0
Werkzeug==0.12.2
```

Create project folder and hello.py follow quickstart (http://flask.pocoo.org/docs/0.12/quickstart/)
> D:\working\projects\learning\python\flask\flaskhome_tutor\hello.py <br>
progress are track by git

Run application
```
$ export FLASK_APP=hello.py		# (set FLASK_APP=hello.py on window)
$ python -m flask run
```

Debug Mode
$ export FLASK_DEBUG=1		# enable debug options (set command on window)

This does the following things:

- it activates the debugger
- it activates the automatic reloader
- it enables the debug mode on the Flask application.

Static files
- Are files that are put under folder _static_ from app structure
- to get url for static file: url_for('static', filename='style.css')
- _static_ is reserved endpoint used for static files and cannot be used as function endpoint for route



