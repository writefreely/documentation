# Writing Posts

Learn how to do more than publish plain text in this guide for writers.

## Adding a title

Titles on WriteFreely are optional, but easy to add by including them in the body of your post.

**Option 1.** Explicitly add a title by starting your post with a Markdown header. That is, type a hash symbol (**#**), a space, and then your title. _The title will show up as a large heading._

```text
# Title of my Post

By starting a line with the hash symbol (#) and a space immediately
after it, Write.as knows that you wanted to use the following text on
that line ("Title of my Post") as the true title of your post.

Now, not only will "Title of my Post" show up in the browser's title 
bar, but it will also show in big letters at the top of this post's page.
```

**Option 2.** Write it on the first line, separated from the rest of the content. _The title will show up the same size as the rest of the post._

```text
Title of my Post

Content begins here, which was started after the blank line above. Since
there was a blank line, "Title" will be the title of the post that shows
up in the top of the browser window. But since we didn't go out of our
way to indicate that was our title, it will also display normally with
the rest of the text on the post itself.
```

## Formatting text

You can format text on WriteFreely with a special kind of syntax called _Markdown_, which uses special characters to indicate bold, italic, and other text. If you've used Markdown before, you'll be right at home here.

### Headers
```markdown
# This is the biggest header (h1)
## This is still a big header (h2)
### This is a smaller header (h3)
###### This is the smallest header you can make (h6)
```

# This is the biggest header (h1)
## This is still a big header (h2)
### This is a smaller header (h3)
###### This is the smallest header you can make (h6)

### Emphasis
```markdown
*This is italic*
_This is italic, too_

**This is bold**
__This is bold, too__

_Here's some **emphatic** text._
```

*This is italic*
_This is italic, too_

**This is bold**
__This is bold, too__

_Here's some **emphatic** text._

### Lists

**Bulleted**:
```markdown
* Hello
* Goodbye
  * Ciao
  * Au revoir
  * Auf Wiedersehen 
  * Arrivederci
```
* Hello
* Goodbye
  * Ciao
  * Au revoir
  * Auf Wiedersehen 
  * Arrivederci

**Numbered**:
```markdown
1. First, this
2. Then that
3. Lastly, this

1. First this
1. Then a second thing
1. Finally a third thing
1. And so on
```

1. First, this
2. Then that
3. Lastly, this

1. First this
1. Then a second thing
1. Finally a third thing
1. And so on

### Images
```markdown
![Cosmic radiation](https://i.snap.as/T05UTpx.jpg)
```

![Cosmic radiation](https://i.snap.as/T05UTpx.jpg)

### Links

```markdown
https://writefreely.org
[A user guide](https://writefreely.org/docs)
```
https://writefreely.org
[A user guide](https://writefreely.org/docs)

Link to your email by putting `mailto:` in front of it:

```markdown
[Contact me](mailto:hello@example.com)
```

[Contact me](mailto:hello@example.com)

### Quotes

```markdown
> Wherever you go,
> there you are.
```

> Wherever you go,
> there you are.

### Inline Code

```markdown
Download the command-line client and run `./writeas new`
```

Download the command-line client and run `./writeas new`

### Syntax-highlighted Code Blocks

<pre>```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world")
}
```</pre>

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world")
}
```
