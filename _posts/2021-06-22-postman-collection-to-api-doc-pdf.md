---
layout: post
ads: true
comments: true
published: true
title: Postman collection to API doc pdf
categories:
  - linux
  - tooltips
tags:
  - postman
  - API Doc
---
### Ref:
- https://apievangelist.com/2020/06/08/the-open-source-community-tooling-built-on-postman/

### Tools
- https://github.com/thedevsaddam/docgen
```bash
curl https://raw.githubusercontent.com/thedevsaddam/docgen/v3/install.sh -o install.sh \
&& sudo chmod +x install.sh \
&& sudo ./install.sh \
&& rm install.sh
```

- Sublime
	- https://packagecontrol.io/packages/MarkdownEditing
    - https://packagecontrol.io/packages/MarkdownPreview
    
### Steps
- Convert collection to markdown   
```bash
docgen build -i input-postman-collection.json -o ~/Downloads/index.md -m
```
- Open generated markdown
- Open "Markdown Preview: Preview in browser"
- Ctrl + P to print to PDF

