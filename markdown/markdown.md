# Markdown Cheat Sheet

## Table of content

* [GitHub Specifics](#github-specifics)
  * [Note](#note)
  * [Tip](#tip)
  * [Important](#important)
  * [Warning](#warning)
  * [Caution](#caution)
* [Headings](#headings)
* [Text Styling](#text-styling)
  * [Bold](#bold)
  * [Italic](#italic)
* [Quoting Text](#quoting-text)
* [Code](#code)
  * [Codeblocks](#codeblocks)
  * [Inline Code](#inline-code)
* [Links](#links)
  * [Linking Words](#linking-words)
  * [Inline Links](#inline-links)
* [Tables](#tables)
  * [Basic Tables](#basic-tables)
  * [Table Formatting](#table-formatting)

## GitHub Specifics

Github added special *Alerts* which can be rendered in the markdown files on GitHub.

### Note

The note can be used like this:

```markdown
> [!NOTE]
> Useful information that users should know, even when skimming content.

```

The rendered result should look like this:

> [!NOTE]
> Useful information that users should know, even when skimming content.

### Tip

Tip can be used like this:

```markdown
> [!TIP]
> Helpful advice for doing things better or more easily.
```

The rendered result looks like this:

> [!TIP]
> Helpful advice for doing things better or more easily.

### Important

Important can be used like this:

```markdown
> [!IMPORTANT]
> Key information users need to know to achieve their goal.
```

The rendered result:

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

### Warning

Warning can be used like this:

```markdown
> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.
```

The rendered result:

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

### Caution

Caution can be used like this:

```markdown
> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.
```

The rendered result:

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.

## Headings

Headlines are created by a leading `#`. If you use multiple hashtags the headings gets smaller.

> [!IMPORTANT]
> Keep in mind that you have to use a specific order to make your markdown files compliant to the markdownlinter.

Examples of different headings:

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
```

## Text Styling

### Bold

**Bold** text can be easily implemeted like this:

```markdown
**Bold Text** can be used like this!
```

### Italic

*Italic* text can be implemeted like this:

```markdown
*Italic Text can be used like this!
```

## Quoting text

To have a quote like this:

> This is quoted text.

You can implement it like this:

```markdown

The following text is quoted:

> This text is quoted!
```

## Code

### Codeblocks

Codeblocks are simply implemented with three backticks:

```text
this is a codeblock
```

It can be used like this:

````markdown
```bash
This is a bash codeblock
```
````

You can and should always define the script language of your code blocks so the code highlighting is correct and your markdown is compliant
to the markdown linter.

Here are some example languages which are available (of course there are many more):

* markdown
* bash
* json
* python

### Inline Code

To use inline code like `this`you can simply surround the word(s) by backticks like this:

```markdown
This is some `inline` code
```

## Links

### Linking Words

This link will lead to [google.com](https://google.com). It can be implemented like this:

```markdown
This link will lead to [google.de](https://google.de)!
```

### Inline Links

An inline Link would look like this: <https://google.com> It can be implemeted with brackets like this:

```markdown
Here is an inline link: <https://google.com>
```

## Tables

### Basic Tables

Tables can also be used, here is a pretty simple example:

| Position | Cost | Currency |
| --- | --- | --- |
| Rent | 250 | $ |
| Gasoline | 50 | $ |
| Groceries | 200 | $ |

The above table can be implemented like this:

```markdown
| Position | Cost | Currency |
| --- | --- | --- |
| Rent | 250 | $ |
| Gasoline | 50 | $ |
| Groceries | 200 | $ |
```

### Table Formatting

PEr default the text within table cells is sticking to the left. Of course you can format a table so all text is sticking to the right for example:

| Position | Cost | Currency |
| ---: | ---: | ---: |
| Rent | 250 | $ |
| Gasoline | 50 | $ |
| Groceries | 200 | $ |

This can be realised like this:

```markdown
| Position | Cost | Currency |
| ---: | ---: | ---: |
| Rent | 250 | $ |
| Gasoline | 50 | $ |
| Groceries | 200 | $ |
```

If you want the text to be centered like this:

| Position | Cost | Currency |
| :---: | :---: | :---: |
| Rent | 250 | $ |
| Gasoline | 50 | $ |
| Groceries | 200 | $ |

Simply modify the table head like this:

```markdown
| Position | Cost | Currency |
| ---: | ---: | ---: |
| Rent | 250 | $ |
| Gasoline | 50 | $ |
| Groceries | 200 | $ |
```
