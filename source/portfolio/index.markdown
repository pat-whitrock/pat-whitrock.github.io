---
layout: page
title: "portfolio"
date: 2014-06-06 15:57
comments: false
sharing: true
footer: true
---

<br>

##[GitShoes](http://gitshoes.com/)

GitShoes makes user feedback even more useful. It creates customizable feedback widgets to submit user feedback to a developer's GitHub repository. Best of all, it provides the developer with the browser, OS, path, and a screenshot from the page where the bug occurred.

* Allows users to submit issues directly to an application’s Github repository without signing in
* Dynamically generated scripts for feedback widgets that post GitHub issues using ERB templates and JavaScript views
* Implemented Devise and OAuth for authentication via GitHub
* Retrieved and cached users’ public, private and organization repositories from GitHub using the Octokit API wrapper
* Implemented JavaScript pagination and search functions to accommodate large collections of repositories
* Accounted for multiple users collaborating on the same repository via before filters and Arel queries
* Allowed users to fully customize widgets without regenerating a script
* Deconstructed Rails and Devise conventions to allow cross-domain AJAX requests by anonymous users

![GitShoes](https://flatiron-showcase.s3.amazonaws.com/uploads/project_image/image/59/large_temp-pic.png =780x)

---

##[GemIt](http://gemit.us/)

GemIt is a single page application for generating simple scraping gems through a non-technical interface. Your gem can scrape text, media, CSS or all three with both a command-line and Ruby implementation. Gems are fully documented and automatically uploaded to your GitHub account.

* Captured node paths with jQuery to create Nokogiri scrapers of unknown elements on unknown pages
* Scraped, stripped and reserved web pages to bypass default Iframe actions of most sites
* Deployed to Digital Ocean and executed Ubuntu shell commands to create and push files to users’ repositories with Git
* Utilized jQuery and Unobtrusive Javascript in Rails to avoid the need for multiple pages
* Combined Bootstrap with a WrapBootstrap theme and custom styles

![GemIt](https://flatiron-showcase.s3.amazonaws.com/uploads/project_image/image/62/large_temp-pic.png =780x)

---