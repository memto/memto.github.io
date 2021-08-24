---
layout: post
ads: true
comments: true
published: true
title: Git submodule Permission denied (publickey)
categories:
  - linux
  - tooltips
tags:
  - git
  - git-submodule
---
##### Issue: git submodule Permission denied (publickey)
```bash
git submodule update --init
Cloning into 'deps/monkey'...
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
fatal: clone of 'git@github.com:monkey/monkey' into submodule path 'deps/monkey' failed
```

##### Solution
```bash
Edit your .gitmodule file with the https url, for example:

[submodule "deps/monkey"]
	path = deps/monkey
	url = https://github.com/monkey/monkey.git
```

Credit: [Stackoverflow](https://stackoverflow.com/questions/11410017/git-submodule-update-over-https/30885128?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
