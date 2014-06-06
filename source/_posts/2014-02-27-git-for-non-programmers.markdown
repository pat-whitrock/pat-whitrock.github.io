---
layout: post
title: "Git for Non-Programmers"
subtitle: "Version control your life"
date: 2014-02-27 20:51:38 -0400
comments: true
categories: 
---

Git is an amazing tool for collaboration and version control, but its merits are largely unknown to the general public. Over the past few years, amazing collaborative tools (e.g. Google Drive) have become popular for word processing, spreadsheets and other common needs for the average person. However, these tools don’t offer sophisticated version control — they are limited to moving linearly backwards and forwards a certain number of steps.
<!--more-->

This limits our ability to create different versions of the same document to test different approaches to a problem or work on the same content at the same time without stepping on each other’s feet or creating entirely new files. Any non-developer who has worked in a normal office setting has probably encountered a project that quickly devolved into folders containing timestamped versions of the same file. If you’re like me, you probably thought putting that on a shared drive in the cloud put you one step ahead of the game. Sadly, that’s probably true, but that’s no excuse not to step up your game some more with Git. That’s right — Git can handle more than just code.

This may seem like a crazy idea — Git wasn’t created to handle spreadsheets or word processing. Well, you know what sounds even crazier? Having a ton of folders with timestamped files on everyone’s hard drive! Using `git add`, `commit`, `branch`, `merge` and `push` is simple to learn and comes with a number of added benefits.

### Version Control for Spreadsheets

Let’s try out some version control in MS Excel. We’ll start out by creating a simple spreadsheet as an XLSX (Excel’s modern default filetype), an XLS (preceded XLSX) and a CSV (comma-separated values). For each set of changes, we’ll create a new branch, add and commit our changes, then switch back to our master branch and merge the two.

![Original .xls, .xlsx & .csv files](https://d262ilb51hltx0.cloudfront.net/max/800/1*dbNPHgeMBz6ByWCnXA75Jg.png)

First, let’s try making some simple edits to our spreadsheets. To start off, we’ll make some small edits and delete a row.

![Text-edited .xls, .xlsx & .csv files](https://d262ilb51hltx0.cloudfront.net/max/800/1*ISkg5HyYMI1U1f2rJBU4Nw.png)

No problems here. Next, let’s try something a bit more advanced. We’ll be adding some simple formulas (not in our CSV, it doesn’t support formulas).

![Formula-edited .xls &.xlsx files](https://d262ilb51hltx0.cloudfront.net/max/800/1*G-XhUMnrDCmE84l6RfMDmw.png)

Still good! One more quick test — let’s see what happens when we edit some styles, like text color and background color.

![Style-edited .xls &.xlsx files](https://d262ilb51hltx0.cloudfront.net/max/800/1*9u1PbzWq9u-nHe8TP3JqCg.png)

That works, too! Awesome — Git can understand our spreadsheets well enough for us to branch off, make changes and then merge them back in.

### Version Control for Word Processing

Next up, word processing. We’re going to follow that same workflow from our spreadsheets — we’ll create a new branch, add and commit our changes, then switch back to our master branch and merge the two.

![Original .docx](https://d262ilb51hltx0.cloudfront.net/max/800/1*wi454P_UTyJk9M_VGbm72A.png)

We’re starting out with a DOCX file — MS Word’s standard filetype. It has a few quotes in there — nothing crazy. Let’s see what happens when we remove the middle one and add a new one at the end.

![Text-edited .docx](https://d262ilb51hltx0.cloudfront.net/max/800/1*VtNtSVi2tScJ1Nmc-v39UA.png)

All good. If you think about it, the contents of a word document are basically the same as the contents of an HTML document — they’re both just plain text in different languages. Or at least they are until you do something else to your word doc, like change up all the spacing, text styles, colors and whatnot. Then what happens?

![Style-edited .docx](https://d262ilb51hltx0.cloudfront.net/max/800/1*iGxrobm17YmuBxso5m1Gjw.png)

Well, that’s no good. Surprisingly, we were able to merge without having to resolve any conflicts, but it looks like Git made some wrong assumptions. Still, we were able to edit our content no problem. So long as we work on our content before our presentation, we should be fine.

### Collaborative Benefits

We’ve really only covered Git’s version control benefits, but its collaborative features can be useful, too. If you incorprate Github into the mix, you and your collaborators have access to even more great features. For example, you could all fork and clone the same repository, work on the same file or collection of files, then push and send a pull request for your team’s leader (e.g. an editor) to review. The leader can even choose to include pieces of individual pull requests and exclude others. And he doesn’t have to download twenty versions of the same file or folder, all with the same name, and clutter his desktop.

### Things to Keep in Mind

Git has its limitations. If you make a lot of drastic changes to a large file, Git might freak out and have no idea what to do, so it’ll declare basically everything a merge conflict. While this is really annoying, it’s still better than it making a bunch of assumptions and breaking your file.

It’s good practice to separate your work on content and style. Git is built to handle changes to code, which is text. So long as your work is limited to your content, you should be fine. Also, the simpler your filetype, the easier it will be for git to manage.

Github’s pull request interface is amazing. Unfortunately, you can’t preview pull requests for certain types of documents like DOCX, XLSX and other filetypes that most people use everyday. But, that interface might exist somewhere else — go find it! Or work your way around it by converting your file to something Github can understand.

### TL;DR

Should I just timestamp a bunch of files, throw them in folders and email everyone I work with?

<iframe width="770" height="480" src="//www.youtube.com/embed/31g0YE61PLQ" frameborder="0" allowfullscreen></iframe>