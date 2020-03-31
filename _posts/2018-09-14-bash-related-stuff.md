---
layout: post
ads: true
comments: true
published: true
title: Bash related stuff
categories:
  - linux
  - tooltips
tags:
  - bash
  - process substitution
---
##### Pipe and redirection

- Ref:
	- [Pipe and stdin redirection to cat](https://superuser.com/a/999611)

- Piping
```bash
$ echo "hello world" | cat
```

- Redirection
```bash
$ cat < file.txt
```

- Process Substitution
```bash
$ cat <(echo "hello world")
```

##### Subshell (COMMAND EXECUTION ENVIRONMENT)

- Ref:
	- [Do parentheses really put the command in a subshell?](https://unix.stackexchange.com/a/138498)

```bash
$ (command1; command2; command3) | command4
```
