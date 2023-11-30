---
Version: 0.1
---

# Markdown Annotations

## Introduction

Over the past year, AI took the world by storm. We observed [its effects on apps](https://ia.net/topics/no-feature), learned to [write with AI](https://ia.net/topics/writing-with-ai), and [released iA Writer 7](https://ia.net/topics/ia-writer-7), our answer to a lot of issues that AI creates for writers. We believe that authorship in plain text is new, useful, and prerequisite to working responsibly with AI text generation.

We experimented with authorship markup directly in text. The markup always got in the way when editing because every other word can have a different author. And the markup made the text difficult to read. 

We store authorship as a separate block at the end of a file. This keeps texts easy to read and share, but requires a tool to update and show authorship. We think it’s the right tradeoff.

```markdown
Markdown Annotations embed authorship in text while preserving its readability and portability.

---
Annotations: 0,95 SHA-256 1132bf5e376a605f5beed4b204456114  
@Human: 0,20 33,4 45,6 62,4  
&AI: 20,13 37,8 51,11 66,29  
...
```


We’ve put a lot of thought, time, and effort into this idea and the implementation. We are open to working with other apps to adapt the idea in some form, ideally establishing a standard.

While the format is open, avoid cloning our work. Draw inspiration from what we made. Change it. Improve it. Design it yourself. Work on it until it is substantially better. If you can’t beat our design, then let it be and do something else. 

## Annotations

A Markdown file consists of a text, followed by an annotation block, followed by the end of file. 

An annotation block consists of:

- a line of three dashes `---`
- followed by one or more [annotations](#annotation)
- a line of three dots `...`

An <a id="annotation" href="#annotation">annotation</a> consists of:

- an [annotation key](#annotation-key), optionally preceded and/or followed by one or more spaces or tabs
- followed by `:`, optionally followed by one or more spaces
- optionally followed by [annotation value](#annotation-value), optionally followed by one or more spaces
- optionally followed by one or more annotation value [continuation lines](#continuation-lines)

An  <a id="annotation-key" href="#annotation-key">annotation key</a> can consist of any characters other than a colon. Colons must be escaped as `\:` to be included in the annotation key. Annotation keys omit the leading and trailing whitespace.

An  <a id="annotation-value" href="#annotation-value">annotation value</a> can consist of any characters.

Annotation values can continue over multiple lines using <a id="continuation-lines" href="#continuation-lines">continuation lines</a>. Their indentation must be two spaces greater than the indentation of the preceding annotation key. The indentation of the first continuation determines the minimum indentation length of subsequent continuation lines. The indentation is excluded from the annotation value. Any initial spaces or tabs beyond the minimum indentation will be included in the annotation value, including in empty lines.

## Hash Annotation

A complete annotation block must begin with a hash annotation, which is used to validate annotation ranges. The hash annotation key should be “Annotations”, optionally translated into the language of the document. The hash annotation value consists of:

- a character range, followed by one or more spaces
- the name of the hashing algorithm which must be `SHA-256`, followed by one or more spaces
- followed by a hash of UTF-8 data of characters specified by the character ranges, optionally truncated to anywhere from 32 characters to the full 64 characters

The hash annotation character range should contain the  range of the entire text. The hash annotation character range must, at the very least, include all annotated character ranges. This allows tools that do not support annotations to append text at the end.

### Last Newline

The hash annotation character range can omit the last newline. When the last newline is omitted, it’s excluded from text. This allows tools to ensure that there’s an empty line between the text and the annotation block. It’s recommended to keep an empty line between the text and the annotation block because it prevents tools that are not aware of annotations from rendering the last line of the file as a heading.

### Error Handling

When the hash annotation is omitted or invalid, tools that support annotations should warn users that annotations may be misplaced, and allow users to review annotations. Users must choose to discard or keep the annotations before continuing editing the file.

## Author Annotations

Author annotation keys consist of:

- `@` for human authors
- `&` for other authors
- optionally followed by one or more spaces
- optionally followed by author name
- optionally followed by author identifier
- optionally followed by author annotation session, with a format to be announced in a future version of the spec, separated from the author name either by the author identifier or a comma

Author identifier consists of:

- an opening angle bracket `<`
- followed by one or more characters, including angle brackets only if (a) they are backslash-escaped or (b) they are part of a balanced pair of unescaped angle brackets
- a closing angle bracket `>`

Author annotation values consist of:

- zero or more character ranges
- optionally followed by author annotation content, with a format to be announced in a future version of the spec

A character range consists of:

- a range location consisting of one or more numbers
- optionally followed by a `,` and a range length consisting of one or more numbers
- when range length is omitted, the range length is assumed to be 1

Character ranges must always refer to grapheme cluster indexes in text to ensure that annotations remain valid regardless of environment or file encoding.

### Authorship

Author annotations that do not contain any content are considered to be authorship annotations.

### To Be Announced

We’re planning to introduce several types of author annotations in the future. We have clear vision for these upcoming author annotation types, and we will update the spec as they become available.

---
Annotations: 0,6310 SHA-256 0650af9e723d7401b1a63e81582eb7bd  
@Anton Sotkov: 15 20,107 128,186 490,642 1319 1562,632 2197,29 2227,18 2267,356 2624,15 2680,19 2705,21 2732,14 2752,4 2764,2846 5611,699  
@Oliver Reichenstein: 314,176  
@Iain Humm: 2194,3 2226 2245,22 2623 2639,40 2699,6 2726,6 2746,6 2756,8 5610  
...
