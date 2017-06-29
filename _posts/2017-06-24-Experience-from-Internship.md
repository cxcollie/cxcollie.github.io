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
2. Careful when branch from other branch without review (it can be messy). When doing this, should create a new branch when pull request, then **cherry-pick** the commits from branch
3. Remember to rebase on master before pull request to merge to master

### June 27
1. 9:00-10:00 meeting; 10:00-17:00 work on 1315(new feature); 17:30-18:00 modification based on 1282 code review
2. Commit hash is calulated based on code change, time, message and so on. So cherry-pick will produce commit with different hash
3. A <a href="https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs">git alias</a><sup>[1]</sup> for displaying commits in graph:
```
	lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all	
```

### June 28
1. 9:00-10:00 meeting; 10:00-15:00 work on scrollable tbody of table (new feature); 15:00-17:00 merge 1282 to master and rebase current ticket on master
2. Working on changes which rebase on some unsubmitted branch is painful (like rebase 1282 on 1281, which is the other ticket which does not pass code review yet). When 1281 is finished, I should squash change in 1282, then create a new temporary branch, cherry-pick the 1282 commit to new branch, then remove old branch
3. Scrollable tbody: We tried to create a table with fixed header, but scrollable tbody. Simply adding _overflow: auto_ to tbody does not help, as the it only applies <a href="https://www.tjvantoll.com/2012/11/10/creating-cross-browser-scrollable-tbody/">when **display: block**</a><sup>[1]</sup>, while the default for table is **display: table** (something like that). So there are 2 ways to do that (and we choose the first later as our system does not need to run on many screen size):
	- Use **display: block**. The disadvantage is we need to fix the column width, as it does not sync header width with body in  **display: block**.
	- Use html trick: use a <div> as a 'fake header' to hide the header element. And the <div> width is so long that they overlap with each other, create the illusion of the <div> width. The disadvantage is as the <div> width is long, we cannot center the text, and can only left aligned the text (otherwise it will be overlapped by next <div>).