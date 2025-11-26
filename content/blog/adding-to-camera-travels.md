+++
title = "Adding to camera.travels"
date = 2025-11-26
[taxonomies]
  tags = ["work", "life", "photos", "memories"]
[extra]
  toc = false
+++

Just a little update on adding photos to [https://travels.carlocarfora.co.uk](https://travels.carlocarfora.co.uk) which has been fun to customise and devise a workflow for! More details below the line...
<!-- more -->

> Figuring out the workflow is half the fun

So now that I had this test site in place for a while, I've finally come back to it deciding it's time to figure out how the best way to add photos and thoughts to it. 

The first thing I wanted to do was customise the theme a little; it just needed to have my own personal links etc. and so creating a copy of the first theme was the easiest solution.

Second was making sure I understood the general workflow. The Expose readme file is great at explaining all of this, all I did was make a couple of predefined `title` and `post` markdown template files that I can reuse.

> Always read the docs

Once I had this, I wanted to go a little further and try to make it so that when I had portrait pictures I could always quickly stitch them together. I came across a neat feature with the Files application in Pop_OS! which is Nautlilus. You can run scripts from a right click menu. I put together (with some quick help from ChatGPT) a script that uses Imagemagick to collage 2 images together. Here it is below:

```bash
#!/bin/bash

# Require at least two selected files
if [ $# -lt 2 ]; then
    zenity --error --text="Select at least two images."
    exit 1
fi

img1="$1"
img2="$2"

# Extract names before renaming (for output filename)
base1=$(basename "$img1")
name1="${base1%.*}"

base2=$(basename "$img2")
name2="${base2%.*}"

# Output directory
outdir=$(dirname "$img1")

# Output file
outfile="$outdir/${name1}-${name2}.jpg"

# --- Create combined image BEFORE renaming ---
convert \
    \( "$img1" -resize 2448x3264^ -gravity center -extent 2448x3264 \) \
    \( "$img2" -resize 2448x3264^ -gravity center -extent 2448x3264 \) \
    +append "$outfile"

# --- Now rename the originals ---
mv "$img1" "$outdir/_$base1"
mv "$img2" "$outdir/_$base2"

zenity --info --text="Created: $(basename "$outfile") and renamed originals."
```

Now I could easily put 2 portrait pics together and not break the flow of the webstie with large images to scroll past (although that can sometimes be an aesthetic choice!). Also learnt about the cool zenity command!

I also used VS Code's Tasks feature to be able to automatically create an .md file from a selection in the Explorer with the same name as the photo. This is how you add text to a photo, and so this task makes it trivial to add text to a story as you go along. I did this by using a shell script as a Task.

```bash
#!/bin/bash

# Check argument
if [ -z "$1" ]; then
  echo "Usage: $0 path/to/file"
  exit 1
fi

file="$1"

# Check file exists
if [ ! -f "$file" ]; then
  echo "File not found: $file"
  exit 1
fi

# Directory and base name
dir=$(dirname "$file")
base=$(basename "$file")
name="${base%.*}"

# Markdown file path
md="$dir/$name.md"

# Default content
cat > "$md" <<EOL
---
top: 80
left: 60
width: 38
height: 20
textcolor: #ffffff
---
<span class="post_text"></span>
EOL

echo "Created Markdown file: $md"
```

Finally, if you're wondering what I'm using to actually edit and go through photos I'm using this wonderful fork of RawTherapee called ART. It's pretty much exactly what I've been looking for in a photo editor and you can find it [here](https://github.com/artraweditor/ART). I can't recommend it enough!

Still a few more travels to put up as I work my way backwards...

Carlo
