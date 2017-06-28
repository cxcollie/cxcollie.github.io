---
layout: post
title: "Experiences from Internship"
date: 2017-06-24
comments: true
tags: [Thoughts]
---

<div class="post-teaser">  </div>
<!-- more -->

<hr/>

### June 26
1. 9:00-11:30 meeting; 11:30-14:30 work on 1315(new feature); 14:30-18:00 modification based on 1282 code review
1. Code fast, then refactor for code review (especially for UI). Done is better than perfect. Work only in work time
2. Careful when branch from other branch without review (it can be messy). When doing this, should create a new branch when pull request, then _cherry-pick_ the commits from branch
3. Remember to rebase on master before pull request to merge to master

### June 27
1. 9:00-10:00 meeting; 10:00-17:00 work on 1315(new feature); 17:30-18:00 modification based on 1282 code review
2. Commit hash is calulated based on code change, time, message and so on. So cherry-pick will produce commit with different hash
3. A <a href="https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs">git alias</a><sup>[1]</sup> for displaying commits in graph:
```
	lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all	
```