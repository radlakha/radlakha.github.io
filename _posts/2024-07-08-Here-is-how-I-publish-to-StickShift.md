---
author: Raman Adlakha
categories: story
hidden: "true"
image: boy-on-stairs.jpg
layout: post
tags:
- workflow
title: Here is how I publish to StickShift
---

I start with giving a title to the story or the note I am posting. I
also select a photo to go with the post from my Lightroom catalogue.
This information is enough to set up the YAML properties at the top of
my markdown file, which will fix all the settings needed by my blogging
software.

I also make a virtual copy of the selected photo and add it to ‘Featured
Images - StickShift’ publishing service I have set up to export the
image to the necessary folder on the local copy of git repo. Before
exporting, take care to: - Post process the photo as per the theme of
the blog. Currently, I am using black and white portraits of various
people - Crop the photo to 1024 × 682. This is the aspect ratio
preferred by the theme I am using on my blog - Give a suitable title to
photo in the metadata window. Use hyphens to separate words in the title
and all lowercase. (OCD). - The title will map to a file name that
should match the image tag in the property at the top of the post (minus
the .jpg part)

Of course, now is the easy part — get your content in there :-)

I use Obsidian for all my content creation. Once I have the post typed
out I have to transfer it over to my repo. Since the filenames have to
follow a certain pattern (include the date) for the blogging system to
work, I use a simple script to take care of this part.

This script also handles any embedded images in the article.

`..\.scripts\pandocpub.ps1 -output_path "H:\works\coderepos\radlakha.github.io\_posts" -input_file "Here is how I publish to StickShift.md"`

Once done, I switch over to the repo folder, stage modified files and
commit followed by a push to GitHub where the GitHub actions take care
of publishing the post. I prefer to do this from terminal within the
Visual Studio Code or, sometimes I use Git Bash.

That’s it! I know I know — this could be simpler and only reason I have
it this way is that I like all documents in one place and images in Lr.
Then I use a git repo, which I like to keep out of my documents.
