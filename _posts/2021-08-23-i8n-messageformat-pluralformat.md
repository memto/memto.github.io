---
layout: post
ads: true
comments: true
published: true
title: I8N messageformat PluralFormat
categories:
  - linux
  - program
  - tooltips
tags:
  - i8n
  - ng-translate
  - messageformat
---
### Ref:

	- https://github.com/messageformat/messageformat/issues/170

### Error

```bash
Error: Invalid key one for argument count. Valid plural keys for this locale are other, and explicit keys like =0.
```

### Fix: change `one` to `=1`

  ![](https://i.imgur.com/ZZ5uYjy.png)
