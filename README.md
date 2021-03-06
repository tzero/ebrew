# EBREW

EBREW lets you create EPUB books and documents using Markdown and a simple JSON manifest format. To get started, install `ebrew` via npm and start a new book:

    > npm i -g ebrew

    > cd ~/projects
    > mkdir lorem-ipsum && cd $_
    > ebrew init
    ...

Follow the prompts to set up a `book.json` manifest, which stores information about your book. Then, get writing!

    > cat >book.md
    # Lorem

    Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
    veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
    commodo consequat. Duis aute irure dolor in reprehenderit in voluptate
    velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat
    cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id
    est laborum.

    ## Ipsum

    Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut
    aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in
    voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur
    sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt
    mollit anim id est laborum.
    ^D

When you're done (or you just want to see how things are looking), use `ebrew` to generate your book:

    > ebrew
    Generated Lorem-Ipsum.epub

You can specify the output filename and the path to your manifest, but EBREW picks sensible defaults so you usually won't have to.

# Sections

As your book gets larger, it's nice to be able to split it up into manageable chunks. The `contents` manifest key lets you put each section in its own `.md` file and join them together in the final product. For example:

```json
{
  "title": "Lorem Ipsum",
  "contents": [
    "lorem.md",
    "ipsum.md"
  ]
}
```

Each section starts on a new page.

# Images

You can add images to your markdown documents as you normally would.

    ![Three circles](images/circles.svg)

EBREW automatically copies the images you use into the resultant EPUB document and adjusts image URLs to correctly refer to them.

# Reference

The above tutorial should give you a pretty good idea of how to use EBREW; the following sections provide a comprehensive reference of the command line interface, the manifest format, and EBREW's Markdown extensions.

## The `ebrew` command

    > ebrew help

#### `ebrew init`
Takes no options. Runs an interactive wizard for creating a new `book.json` manifest.

#### <code>ebrew [make\] [<em>options</em>\] [output=<em>title</em>.epub]</code>
Generates an EPUB file from the given manifest.

**--input, -i** — Path to the book manifest. Pass `-` for standard input. Default `./book.json`.

## The manifest format

| key | description |
|----:|:------------|
| `uuid` | A universally unique identifier for the book. Required. |
| `isbn` | An ISBN for the book. Optional. |
| `doi` | A DOI for the book. Optional. |
| `contents` | A path or list of paths corresponding to sections in the book. Required. |
| `cover` | The path to the cover image for the book. Optional. |
| `coverPage` | Specify `false` to omit the generated cover page from the spine of the book. The cover image will still be present as metadata, but will not appear in the book content. Default: `true`. |
| `css` | A path or list of paths to stylesheets for the book. Default `[]`. |
| `title` | The book's title. Default: `"Untitled"`. |
| `sortTitle` | The book's title. Default: generated from `title`. |
| `subtitle` | The book's subtitle, usually displayed below or beside the title, separated from it by a colon. Default: `""`, i.e., no subtitle. |
| `onlyTitle` | Specify `true` to omit the subtitle from the full title of the book, i.e., only include it on the title page and not in the metadata title. Default: `false`. |
| `language` | An RFC 3066 language identifier indicating the primary language of the book's content. Default: `"en"`. |
| `author`/`authors` | A string, object, or list of strings and/or objects indicating the authors of the book. Valid object fields are `name` (required), `sort` (sort order name, default derived from `name`), and `role` (a [MARC relator](http://www.loc.gov/marc/relators/relacode.html) default `'aut'`). Default: `""` or `[]`, i.e., no authors. |
| `publisher` | The book's publisher. Default: `""`, i.e., no publisher. |
| `rights` | A statement about rights. Default: <code>"Copyright ©<em>year</em> <em>authors</em>"</code>. |
| `date` | The date of publication of the book. Default: today. |
| `created` | The date on which the book was created. Default: same as `date`. |
| `copyrighted` | The copyright date of the book. Default: same as `date`. |
| `toc` | Specify `false` to omit the generated table of contents from the spine of the book. The table of contents will still be present as metadata, but will not appear in the book content. Default: `true`. |
| `tocDepth` | The maximum nesting level of the generated table of contents. Default: `6`, i.e., no limit. |
