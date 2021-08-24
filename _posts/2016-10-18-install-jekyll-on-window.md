---
layout: post
tags:
  - jekyll
title: Install jekyll on Window
categorys: program tooltips
published: true
ads: true
comments: true
categories:
  - program
  - tooltips
---

- Download ruby installer (ex: Ruby 2.2.5 (x64)) and install
[ruby installer](https://rubyinstaller.org/downloads/ "ruby installer")
- Add ruby dir to environment variable

- Download and install NodeJS
[NodeJS](https://nodejs.org/en/ "NodeJS")

- Download and install python (ex: ver 2.7)
[python download](https://www.python.org/downloads/ "python download")
- Add python to environment variable

- Open cmd window and install jekyll
> gem install jekyll

- RubyGems SSL errors on Ruby for Windows (RubyInstaller)
[RubyGems SSL errors, RubyInstaller](https://gist.github.com/luislavena/f064211759ee0f806c88 "RubyGems SSL errors, RubyInstaller")

- Use gist in jekyll <br>
> gem install jekyll-gist <br>
add jekyll-gist into gems in *_config.yml* as below <br>
> gems: [jekyll-gist]

- Liquid Exception B: certificate verify failed <br>
> [Download a cacert.pem for RailsInstaller](https://gist.github.com/fnichol/867550 "Download a cacert.pem for RailsInstaller")

- Run jekyll <br>
> Go to github page source directory <br>
jekyll serve

#### github page bloging with prose.io
[prose.io](http://prose.io/) is a web based application that is a github content editor. it has support to make bloging on github page more easily. To use it just add _prose_ configuration in github page config.yml as mine here [sample config.yml](https://github.com/memto/memto.github.io/blob/master/_config.yml), go to [prose's wiki](https://github.com/prose/prose/wiki/Getting-Started) for more infomation.
