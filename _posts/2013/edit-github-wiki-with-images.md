---
ID: 411
post_title: >
  How do I edit my GitHub Wiki locally and
  include relative images?
post_name: edit-github-wiki-with-images
author: David
post_date: 2013-12-12 14:34:45
layout: post
link: >
  https://blog.davidchristiansen.com/2013/12/edit-github-wiki-with-images/
published: true
tags:
  - github
  - github-wiki
  - github-wiki-images
categories:
  - Development (General)
---
This post sets out to describe how to edit a GitHub wiki on your computer as an alternative to editing directly within the GitHub web UI.
<h3><strong>Cloning the GitHub Wiki to your computer</strong></h3>
The wiki content is unsurprisingly stored within a Git repository and GitHub give you direct access to this repository which can enable you to edit the content locally on your computer. First though we need to clone the git repository to our computer.
<ul>
	<li>From your repository homepage on GitHub, click the Wiki link on the right hand side <img style="display: inline; border-width: 0px;" title="Wiki link example" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2013/12/image.png" alt="Wiki link example" width="60" height="17" border="0" /></li>
	<li>If you already have content in your wiki, Click the <strong>Clone URL </strong>button.</li>
</ul>
<img style="margin-left: 0px; display: inline; margin-right: 0px; border: 0px;" title="image" src="http://davidchristiansenblog.azurewebsites.net/wp-content/uploads/2013/12/image2.png" alt="image" width="240" height="77" border="0" />
<ul>
	<li>If you have yet to create a page on your wiki you only see the green New Page button. Simply click <strong>New Page</strong>, set a page title (for example Home) and put some filler blurb content in the page body. Hit Save, and the <strong>Clone URL</strong> button should appear.</li>
</ul>
Hitting the Clone URL button will copy the git url for your wiki repository into your clipboard.
<ul>
	<li>Now, go to your favourite git client and clone the repository to your computer.</li>
</ul>
<h3><strong>Editing your wiki content efficiently (Windows)</strong></h3>
GitHub wiki content can be written in a number of different markup languages. My preference is markdown.

<img style="border-width: 0px;" src="http://code52.org/DownmarkerWPF/screenshot.png" alt="" border="0" />

For this I use an awesome editor (if I do say so myself) called <strong><a href="http://code52.org/DownmarkerWPF/" target="_blank">MarkPad</a></strong> which was one of the projects <a href="http://code52.org/" target="_blank">we</a> worked on as part of the <a href="http://code52.org/#/goals" target="_blank">Code52</a> effort.

MarkPad is available for windows desktop (<a href="http://ginnivan.blob.core.windows.net/markpadrelease/MarkPad.application" target="_blank">download</a>) as well as a windows store app (<a href="http://apps.microsoft.com/webpdp/app/9a6d2b74-a7d9-4edf-9ea4-29d8f21b4c29" target="_blank">install from Windows Store</a>) – <a href="http://github.com/Code52/DownmarkerWPF" target="_blank">the source is also available</a> for all if you want to have a poke about.

<em>Sidenote: MarkPad is open source and as such is open to contributions from anyone. We are <strong>very </strong>open to community contributions so if you use it and think of a way to make it even better then please do get in touch via GitHub issues.</em>

Instructions on how to write Markdown is out of the scope of this article but anyone familiar with editors such as MSWord will pick up thinks pretty quickly.

Ok, so one of the best life-hacks (read: makes your life simple) offered by MarkPad is its handling of images. You can of course construct links to images on the web in markdown but what is extra handy is that MarkPad can also generate image files on the fly from your clipboard.

Simply take a screenshot using something like <a href="http://shotty.devs-on.net/en/Overview.aspx" target="_blank">Shotty</a> (copy to clipboard) or <a href="http://cropper.codeplex.com/" target="_blank">Cropper</a> (set output to clipboard)  and <strong>Paste (</strong>&lt;ctrl&gt;+v) the contents of your clipboard. MarkPad will automatically generate an image file appropriately named for where you pasted within your document (in relation to the closest header) and link to it in markdown. Very cool, very easy.

<strong>Note: </strong>When linking to images in markdown that have paths relative to the wiki page be sure to use  “\” as the path separator and not “/”
<h3>Push your changes to GitHub</h3>
Commit your changes, making sure to include (add) any new content and/or images generated then just perform a GIT push to submit your changes to github. Your changes will be visible on the wiki immediately – go marvel in their splendour.