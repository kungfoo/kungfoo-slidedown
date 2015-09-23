# Ergon Slidedown

- [Gradle Goodies Versioning Plugin](?src=versioning_plugin.md)
- [Another Presentation](?src=sample.md)
- [original](http://danieltao.com/slidedown)

***

# The basic idea

Write your presentations in a text editor.

- No fancy WYSIWIG editor
- No hand-written HTML
- Just text (Markdown)

Separate slides with `***`.

***

# For example

This is the source for the previous slide:

```markdown
***

# The basic idea

Write your presentations in a text editor.

- No fancy WYSIWIG editor
- No hand-written HTML
- Just text (Markdown)
- Publish by pushing via `git`.

Separate slides with `***`.

***
```

***

# How it works

Slidedown parses Markdown, then splits up the HTML into slides by splitting at
every `<HR>` tag.

***

# How it works

Every slide has a *layout* which is inferred by the elements making up that
slide.

***

# For example, this slide has an `<h1>` and an `<h2>`
## So Slidedown infers that the layout should be like this

***

# Pros

- easy
- saves time
- looks great

# Cons

- none?

***

# Navigation

You can navigate left/right through the slides using the keyboard.

You can also click on either side of the screen, or use swipe gestures
on mobile devices.

***

# How to use it

- Clone the following repository:

    https://stash.ergon.ch/projects/STAFF/repos/ergon-slidedown/browse

- Make changes
- Add you markdown file to the top of this file
- Load index.html in browser
- Push to master branch
- Wait 1 min: Published to http://egge.ergon.ch/static/ergon-slidedown/
- Have fun

***

# The End

Cheers, Silvio