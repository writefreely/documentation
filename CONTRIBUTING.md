# Contributing Documentation

WriteFreely documentation aims to communicate how WriteFreely works in an approachable way that doesn't waste readers' time. It should be plain, concise, and follow the general style we've developed for these guides.

## General notes

### Prioritize clarity

These guides should seek to convey a depth of information, but not in a meandering or rambling way. Remove unnecessary words, and leave out fluff that doesn't teach the reader anything. Avoid excessively conversational writing. Speak plainly, and use active voice instead of passive.

### Specific audience

Avoid generalizations, and be precise about who you're speaking to. In each article, there should only be a single type of user you're referring to as "you." If you use this pronoun in any given article, use it consistently, and then choose unambiguous terms to describe the different people outside of the relative "you." Always choose specific terms for "users" before reverting to the more generic term. In order of priority:

1. The _kind_ of user (e.g. "admin", "writer", "reader", "subscriber", "follower", "commenter")
2. "User"

### Specific steps

Be specific about what you're instructing the reader to do, and explain actionable steps in unambiguous terms. For example:

* ✔ **Good**: "Click the Share button, then copy the address in the window that appears."
* ❌ No: "Copy the link." 
* ✔ **Good**: "Click the Reader checkbox to enable it." Or "Toggle the Reader checkbox." Or "Enable the Reader checkbox."
* ❌ No: "Turn it on."

### Self-contained sections

Remember the reader who wants to jump into the article section most important to them, skim the material, and then get out of here. Support them by making each section stand on its own as much as possible.

### Technical terms

To keep our guides accessible to technical and non-technical readers alike, always aim for non-technical terminology, only reverting to technical jargon when absolutely necessary (this should be rare). Common examples:

* "address" instead of "URL"
* "site" or "website" instead of "instance"
* "writer," "reader," etc. instead of "user"

## Style

### Abbreviations

Aim to avoid abbreviations as much as possible, unless they're commonly known. When using abbreviations, expand unknown abbreviations at their first encounter, e.g.

* ✔ **Good**: We support ActivityPub (AP) mentions. AP is a social web protocol.

### Links

Link text should describe the nature of the linked document (e.g. "Admin guide" instead of "click here"). Do not include terminal punctuation in the link text. Aim for shorter links over longer ones.

### Images

All included images should include `alt` text that describes the image for those who cannot see it.

### Code and technical terms

For bits of source code, variable names, and typed text (such as configuration values), surround the text with a single backtick (<code>\`</code>).

For a full line or more of source code or configuration text, surround the text with three consecutive backticks on each end (<code>\`\`\`</code>), with the backticks living on their own lines. If this _code block_ contains source code, include the language name immediately following the first three backticks to enable syntax highlighting (e.g. <code>\`\`\`go</code>).

### UI elements

User interface elements should be _italicized_. 

* ✔ **Good**: Scroll down and press _Save Changes_.
* ❌ No: Scroll down and press "Save Changes."
* ✔ **Good**: Press the _Publish_ button.
* ❌ No: Press "Publish."

Likewise, application sections / pages should be italicized when referred to as part of the UI.

* ✔ **Good**: Next, click on _Blogs_ to go to the Blogs page.

Otherwise, application sections / pages should simply be capitalized as proper nouns.

* ✔ **Good**: Next, go to the Blogs page.

### Punctuation

* **Commas** - use the [serial / Oxford comma](https://en.wikipedia.org/wiki/Serial_comma).
* **Ampersand** - only use the ampersand in titles, if you use them at all.
* **Em dashes** - insert em dashes as two hypens (`--`) surrounded by one space on each side. They will be converted to proper em dashes by our Markdown parser.
* **Quotations** - insert a double prime character (`"`) instead of "curly" (or "smart") quotation marks. Position punctuation according to Chicago rules (periods on the inside, etc.).

## Contributing

Once you've finished writing, you'll want to commit your changes and open a pull request on GitHub.

### Commit messages

We highly value commit messages that follow established form within the project. See our [WriteFreely commit guidelines](https://github.com/writefreely/writefreely/blob/develop/CONTRIBUTING.md#commit-messages) for best practices.
