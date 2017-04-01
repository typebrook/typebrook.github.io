---
title: 在新電腦使用hexo寫blog
comments: false
categories: [Coding, blog]
tags: [hexo]
date: 2017-03-23 21:44:04
---

1. install git & node.js

2. Clone your repository(souce branch, not master branch)
   ```
   git clone -b source ~my_blog_repository~
   ```
3. In hexo folder:
   ```
   npm install -g hexo-cli
   npm install hexo-deployer-git --save
   ```
4. Set user info:
   ``` 
   git config --global user.email "you@example.com"
   git config --global user.name "Your Name"
   ```
5. Enjoy your deploy~
   ```
   hexo g -d
   ```