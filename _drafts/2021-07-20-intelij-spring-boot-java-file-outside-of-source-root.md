---
layout: post
ads: true
comments: true
published: false
title: InteliJ Spring Boot Java file outside of source root
categories:
  - linux
  - program
  - tooltips
tags:
  - InteliJ
  - Spring Boot
---
#### Issue1: create new maven project "prj1" in "parrent1" folder then open "parent1" folder in InteliJ
    - pom.xml show as XML file only

	![](https://i.imgur.com/zp3VtKi.png)
    
    - cannot create Java class/package/interface
    
    ![](https://i.imgur.com/ZKFM0Qt.png)

- Fix1:
	- Right click on pom.xml file > Add as Maven Project
    
    ![](https://i.imgur.com/Vlmfh4r.png)
    
    ![](https://i.imgur.com/zyB9UOT.png)

#### Issue2: InteliJ Spring Boot Java file outside of source root (project clone somewhere)

![](https://i.imgur.com/yf2kZFf.png)

- Fix2: mark "java" folder as source root

![](https://i.imgur.com/MO1kjPX.png)

