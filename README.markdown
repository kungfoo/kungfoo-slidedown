# kungfoo's Slidedown

- [git gud: A semi-deep dive into git...](?src=git-gud.md)
- [Gradle Goodies Versioning Plugin](?src=versioning_plugin.md)
- [Try giving a fuck](?src=giving-a-fuck.md)
- [RxJava: How I learned to stop worrying...](?src=rx-java.md)
- [Another Presentation](?src=sample.md)
- [original](http://danieltao.com/slidedown)

***

# The basic idea

Write your presentations in a text editor.

- No fancy WYSIWIG editor
- No hand-written HTML
- Just text (Markdown)
- Publish by pushing via `git`.

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

- Make changes
- Add you markdown file to the top of this file (to get a link)
- `docker-compose up -d`
- Navigate to `localhost:8080`, change code, reload. Have fun.

***

# The End

Cheers, Silvio
