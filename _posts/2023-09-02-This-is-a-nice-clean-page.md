---
layout: post
title: "This is a nice clean page"
author: "Raman Adlakha"
categories: story
tags: [tag1,tag2]
image: planters.jpg
---

I have some text to introduce the concept. There is also some code
below.

`code -help -but -you -can -do -it`

This is lorem ipsum djl akld flsa lksd flks dflak sdflsk dlkf lskd jflk
jsdaflk sdaflk asldkfjlaskfd lak jsdflk sadlkf jals kfjlka sfdlk sfdla
jsdflksd jfalk;a dfskj al;j sdklf lasjdkflak jsflak faksfd lasfd lka;
sflka sfjlasflsadl fas;lfalsdklasjflka sfjlaj dslfkjalksfdlaksdflafl

Now we refer to another page [Bombay in August
2023](Bombay%20in%20August%202023.md)

> This is a call to action. We will figure emojis later

Below you will see an image but before that here is a reference to a
Book ‘How to take smart notes’ (Caulfield 2020) You can also attempt to
pull another reference. Let us say this book is based on \#zettelkasten
method (Ahrens 2017)

So we use Zotero Pandoc Integration here using cmd-P

Now we will attempt to insert an image below

![](_assets/Pasted%20image%2020230902112454.png)

Above was directly pasted into the editor. If this works the image
should also appear when we export using Pandoc.

Next, run with Pandoc plugin: Export as Pandoc Markdown first with
option: `--extract-media=E:\works\coderepos\root`

and then without! See settings screenshot below. Press CTRL-P (CMD-P)
and type pan mar to highlight Export as Pandoc Markdown option

Alternatively, run the following two commands from command line.
`pandoc --verbose --from markdown --to markdown -o "E:\works\coderepos\root\This is a nice clean page.pandoc.md" "E:\works\DaZettelkasten\This is a nice clean page.md" -t markdown_strict --citeproc --bibliography=E:\works\DaZotero\MyLibrary.bib --extract-media=E:\works\coderepos\root`pandoc
–verbose –from markdown –to markdown -o “E:is a nice clean
page.pandoc.md” “E:is a nice clean page.md” -t markdown\_strict
–citeproc –bibliography=E:.bib –extract-media=E:

`pandoc --verbose --from markdown --to markdown -o "E:\works\coderepos\root\This is a nice clean page.pandoc.md" "E:\works\coderepos\root\This is a nice clean page.pandoc.md" -t markdown_strict --lua-filter=replace_path.lua`
![](_assets/Pasted%20image%2020230902130619.png) *Pandoc Plugin settings
used by me*

Lastly you should see list of references below.

*References*

Ahrens, Sönke. 2017. *How to Take Smart Notes: One Simple Technique to
Boost Writing, Learning and Thinking: For Students, Academics and
Nonfiction Book Writers*. North Charleston, SC: CreateSpace.

Caulfield, Jack. 2020. “A Quick Guide to Harvard Referencing | Citation
Examples.” *Scribbr*.
https://www.scribbr.co.uk/referencing/harvard-style/.
