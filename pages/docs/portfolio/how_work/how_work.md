---
title: How I Work
sidebar: second_sidebar
toc: false
permalink: how_work.html
---

When I started at my previous employer in 2007 there were 11 employees and no technical writer. Any user documentation was done either by someone in Sales or by one of the application maintainers. I started taking on the tech writer responsibility in addition to my normal tech support and system admin duties, and eventually technical writing became my full-time job. 

I was responsible for creating and maintaining all user-facing documentation and release notes, as well as doing the majority of all testing and QA. I also worked with Sales and Marketing to help present a consistent message between the website copy for new products and services and the information presented in the user documentation. 

Because I was not only the lone writer but the very first employee in that position, I was able to develop my own procedures and methods for managing my writing projects and moving my documentation from draft into production, as well as decide on the tools to use to actually create the documentation. 

## Project management 

The two owners of the company were both former CPAs, with Master’s degrees in Accounting. Excel was their native language, and since they both relied on spreadsheets to manage their projects and company projects, I decided to do the same rather than try to introduce a new tool into the company workflow. In order to avoid having to e-mail a new copy of a spreadsheet every time I updated something, I created spreadsheets using the company Google Apps account. This meant that anyone who needed to check my progress on a particular project or document was able to do so. 


Because I was a lone writer and involved in projects in every department in the company, it was essential that I had a way to track where I was on a particular document or project. I would start my Monday with a carefully planned list of what I was going to work on that week, only to have everything turned upside down within the first hour of my workday because the dev team was ready to move a new service into production and I needed to switch gears and work on that. Sometimes weeks would go by before I could get back to what I was originally working on. 

Over time, I noticed that all my documentation moved through four distinct phases: pre-production, production, review, and publication, and that each phase had several high-level steps that I needed to work through. It took several years and a number of iterations to put together the current version of my publication checklist, but I’ve found that it to be an extremely valuable tool to help me manage my work. 

**Publication Checklist Template**- [Google Sheet](https://docs.google.com/spreadsheets/d/1nx0tdGrUMTLd5dLr8ioB4WH5YXAhnSlF80btHBDX1Tk/edit?usp=sharing "Publication Checklist Template - Google Sheet") / [LibreOffice Calc](/pages/docs/portfolio/how_work/PublicationChecklists.ods "Publication Checklist Tempate - LibreOffice") - this has a bare copy to use as a template, and second sheet that shows how the checklist looks in production

**Note** - feel free to download, use, remix, or redo these checklists for your own use. 

The Publication Checklist makes use of conditional formatting, meaning that I could type in `done`, `ip` (in progress), `na` (not applicable), or `prob` (problem) into a cell and that cell would change color accordingly. This allowed anyone to see at a glance the status of a particular document. (Note - the conditional formatting may not survive the download). 

I’ve used the same general concept to manage other documentation projects. For example, I used a spreadsheet to manage the production for the [Uses This API](/uses-this-api.html "Uses This API") that is included with this portfolio: [Google Sheet](https://docs.google.com/spreadsheets/d/1cWlj3rzlLWvxKwFRGX6fF0QUBLq-8yAazQNwmgTTX9Y/edit?usp=sharing "Uses This Publication Checklist"). 



## Documentation tools 

As a web hosting company, my former employer relied heavily on open source software. Servers were usually CentOS Linux, and the company specialized in Java application server hosting: Tomcat, JBoss, GlassFish, and WildFly. 

Coming from a Linux system admin background, I decided to stay with the tools I was already familiar with, such as Vim and Git. I needed a way to create HTML output, because the documentation I created would be hosted on a knowledge base platform bundled with our ticket system and it only understood HTML. I also needed a way to keep everything under revision control in case I needed to revert a document back to a previous version. 

After some experimentation, I settled on the following toolset: 

* [**Vim**](http://www.vim.org "Vim") - for text editing   
* [**Markdown**](https://en.wikipedia.org/wiki/Markdown "Markdown") - for formatting   
* [**Pandoc**](http://pandoc.org/ "Pandoc") - to convert from Markdown to HTML (with sections added to my .vimrc to help automate this task)  
* [**Gitolite**](http://gitolite.com/gitolite/index.html "Gitolite") - to manage all document version control   

Overall, this is a fairly simple toolset, but it did the job I needed it to do. Using lightweight tools allowed me to move quickly and without a lot of application overhead. This toolset also meant that anyone with a passing familiarity with Vim (something very common amongst Linux system admins) could easily step in and create or update documentation, instead of having to learn a more complex tool like FrameMaker. 

