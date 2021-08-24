---
layout: post
ads: true
comments: true
published: true
title: Some git commands
categories:
  - linux
  - program
  - tooltips
tags:
  - git
  - git-submodule
---
### Q/A

- How to revert initial git commit?

```bash
# https://stackoverflow.com/a/6637891/5715800

# You just need to delete the branch you are on. You can't use git branch -D as this has a safety check against doing this. You can use update-ref to do this.

git update-ref -d HEAD

# Do not use rm -rf .git or anything like this as this will completely wipe your entire repository including all other branches as well as the branch that you are trying to reset.
```

- Git unset config

```
git config --list
git config --list --global
git config --global --unset user.email
```

### Git submodules

- create and config submodules

```bash
$ tree -L 2
.
├── elasticsearch
│   ├── custom
│   └── elk-local
├── kafka-local
│   ├── config
│   ├── docker-compose.yml
│   ├── docs
│   ├── LICENSE
│   └── README.md
├── mongodb
│   └── mongo-compose.yaml
└── README.md

# Usage: git submodule add <repo-url> <submodule-path>

# Ex
$ git submodule add https://github.com/memto/elk-local.git elasticsearch/elk-local
$ git config -f .gitmodules submodule.elasticsearch/elk-local.ignore untracked

$ git submodule add https://github.com/memto/kafka-local.git kafka-local
$ git config -f .gitmodules submodule.kafka-local.ignore untracked

$ cat .gitmodules 
[submodule "elasticsearch/elk-local"]
	path = elasticsearch/elk-local
	url = https://github.com/memto/elk-local.git
	ignore = untracked
[submodule "kafka-local"]
	path = kafka-local
	url = https://github.com/memto/kafka-local.git
	ignore = untracked
```

- clone and update submodules

```bash
# Ref: 
# - https://stackoverflow.com/a/4438292/5715800

# With version 2.13 of Git and later, --recurse-submodules can be used instead of --recursive:

git clone --recurse-submodules -j8 git://github.com/foo/bar.git

# With version 1.9 of Git up until version 2.12 (-j flag only available in version 2.8+):

git clone --recursive -j8 git://github.com/foo/bar.git

# With version 1.6.5 of Git and later, you can use:

git clone --recursive git://github.com/foo/bar.git

# For already cloned repos, or older Git versions, use:

git clone git://github.com/foo/bar.git
cd bar
git submodule update --init --recursive
```
