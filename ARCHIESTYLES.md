# Using ArchieML to edit data stories

This document suggests a way to use [ArchieML](http://archieml.org/) for editing long-form data stories. This is not a definitive guide. For more ways to use ArchieML see the [official site](http://archieml.org/).

### What is ArchieML?

ArchieML is a "markup/markdown" language created by the New York Times to make it easier for multiple editors to work on a single story through a familiar interface, Google Docs in this case.

### Benefits

For editors, ArchieML is a "lightly structured" format. That means the format is relatively tolerant of things like extra white space or multiple blank lines. That makes ArchieML documents easy to write, even without any experience in structured content.

For developers, ArchieML produces a structured JSON output that can be used to build websites and apps. Also it means that editors can work on the text without needing access to the underlying code.

### Use case and example

Media Hack Collective uses ArchieML to edit documents in Google Drive. THe structured content in pulled into the website code using a Node task script.

### ArchieML document structure

There are many different ways to structure an ArchieML document.

In its most common form ArchieML defines blocks of text/content using either square brackets [] or curly braces {}. In almost all cases content blocks need a starting and an ending set of brackets/braces. The exceptions are single lines of text. This is an example of an ArchieML document including the most common elements:

    headline: Headline text
    subhead: Sub headline text
    introduction: Introductory or blurb text

    [+section1]

    {.crosshead}
    text: Crosshead text
    {}

    Text in here
    Text in here

    {.image}
    link: https://link
    caption: Caption text
    {}

    Text in here.
    Text in here.

    [.list]
    - Item one
    - Item two
    []

    Text in here.
    Text in here.

    {.quote}
    text: Quote text
    credit: Quote credit
    {}

    Text in here
    Text in here

    {.comment}
    Comment in here
    {}

    []

In most cases ArchieML marks blocks of text and content with a set of starting brackets/braces and a closing set, much like HTML.

To compare, a single line of text is marked using a `key:` and a `value`

For example:

    headline: This is a headline

A longer block of text, for example a section with multiple paragraphs, is marked using opening and closing brackets. For example:

    [+section]
    Some text in here

    Some text in here
    []

### More examples

#### Sections/blocks of text

Blocks of text like sections are marked with square brackets (before and after like:

    [+sectionname]
    Text in between these.

    Another paragraph.

    []

#### Single lines

Single lines of text (including multiple sentences, are labelled without brackets, like this:

    headline: This is the headline

#### Links

ArchieML doesn't (appear) to have a way to handle links. One option is to mark links using a Markdown-style format. The usual Markdown link format is to include the link text (in square brackets) followed by the link URL (in round brackets). For example:

    [link text](http://linktext)

See the related document with a suggested Javascript function to [convert Markdown-format links to HTML](https://github.com/mediahackza/svelte-template/blob/main/MDLINKSTOHTML.md).

#### Images

Images are added using curly braces (at the start and the end) and a dot:

    {.image}
    link: http://imagelink
    caption: Image caption text
    {}

Lists are similar to images but use square brackets and a dot. Use asterisks to mark each item in the list. For example:

    [.list]
    *Item one
    *Item two
    []

Comments in ArchieML are deprecated. They may still be available in various parsers using the `:skip` and `:endskip` keywords but these are likely to be removed entirely in the near future. One solution is to mark a comment using curly braces and then then filter these out of the final layout. For example:

    {.comment}
    Comment in here
    {}

### Related documents

- [ArchieML site](http://archieml.org/)
- [Media Hack Svelte template](https://github.com/mediahackza/svelte-template) which uses ArchieML
- Suggested function to [convert Markdown links to HTML](https://github.com/mediahackza/svelte-template/blob/main/MDLINKSTOHTML.md).

### Example with comments

    headline: Headline text // A single line doesn't need brackets
    subhead: Sub headline text
    introduction: Introductory or blurb text

    [+section1] // opening set for a section.

    {.crosshead} // opening for a crosshead
    text: Crosshead text
    {} // closing for a crosshead

    Text in here
    Text in here

    {.image} // opening for an image
    link: https://link
    caption: Caption text
    {} // closing for an image

    Text in here.
    Text in here.

    [.list] // opening for a list
    - Item one
    - Item two
    [] // end of list

    Text in here.
    Text in here.

    {.quote} // opening for quote
    text: Quote text
    credit: Quote credit
    {} // end of quote

    Text in here
    Text in here

    {.comment} // opening for comment
    Comment in here
    {} // closing for a quote

    []  // closing brackets for [+section1]
