<a href="https://github.com/mgalgs/make-readme-markdown"><img src="https://www.gnu.org/software/emacs/images/emacs.png" alt="Emacs Logo" width="80" height="80" align="right"></a>
## make-readme-markdown.el
*Convert emacs lisp documentation to markdown all day every day*

---
[![License GPLv3](https://img.shields.io/badge/license-GPL_v3-green.svg)](http://www.gnu.org/licenses/gpl-3.0.html)

This tool will let you easily convert Elisp file comments to markdown text so
long as the file comments and documentation follow standard conventions
(like this file). This is because when you're writing an elisp module, the
module itself should be the canonical source of documentation. But it's not
very user-friendly or good marketing for your project to have an empty
README.md that refers people to your source code, and it's even worse if you
have to maintain two separate files that say the same thing.

### Features


* Smart conversion of standard Elisp comment conventions to equivalent
  markdown (section headers, lists, image links, etc)
* Public function documentation from docstrings
* License badge (auto-detected, see [Badges](#badges))
* MELPA and MELPA-Stable badges (auto-detected, see [Badges](#badges))
* Travis badge (auto-detected, see [Badges](#badges))
* Emacs Icon

### Installation


None

### Usage


The recommended way to use this tool is by putting the following code in
your Makefile and running `make README.md` (You don't even have to clone the
repository!):

    README.md: make-readme-markdown.el YOUR-MODULE.el
    	emacs --script $< <YOUR-MODULE.el >$@ 2>/dev/null

    make-readme-markdown.el:
    	wget -q -O $@ https://raw.github.com/mgalgs/make-readme-markdown/master/make-readme-markdown.el

    .INTERMEDIATE: make-readme-markdown.el

You can also invoke it directly with `emacs --script`:

    $ emacs --script make-readme-markdown.el <elisp-file-to-parse.el 2>/dev/null

All functions, macros, and customizable variables in your module with
docstrings will be documented in the output unless they've been marked
as private. Convention dictates that private elisp functions have two
hypens, like `cup--noodle`.

### Badges


A license badge is generated if a license can be detected.  Just include
the license in your file's comments like normal, taking care to
copy/paste the license from its source verbatim.

A MELPA badge is generated if a package is listed on MELPA whose URL
matches the URL in your file's pseudo-headers.  Specifically, the URL is
taken from that familiar chunk of key-value pairs near the top of your
file's pseudo-header comments that usually look something like this:

    ;; Author: Mitchel Humpherys <mitch.special@gmail.com>
    ;; Keywords: convenience, diff
    ;; Version: 1.0
    ;; URL: https://github.com/mgalgs/diffview-mode

In this case, we would search MELPA for a package whose listed URL
matches https://github.com/mgalgs/diffview-mode.  If such a package is
found, a MELPA badge is emitted.  The same approach is taken for
MELPA-Stable.

A Travis badge is generated by querying the Travis API for a project
whose `username/repo` key matches the one listed in the URL tag.  So in
our example above we would query Travis for a project named
`mgalgs/diffview-mode`.  Currently this only works for projects hosted
on GitHub.

### Syntax


An attempt has been made to support the most common Elisp file comment
conventions.  Specifically, following patterns at the beginning of a
line are special:

* `;;; My Header:` ⇒ Creates a header
* `;; o My list item` ⇒ Creates a list item
* `;; * My list item` ⇒ Creates a list item
* `;; - My list item` ⇒ Creates a list item

Everything else is stripped of its leading semicolons and its first
space, then is passed directly out.  This means that you can embed
markdown syntax directly in your comments.  For example, you can embed
blocks of code in your comments by leading the line with 4 spaces (in
addition to the first space directly following the last semicolon). For
example:

    (defun mrm-strip-comments (line)
      "Strip elisp comments from line"
      (replace-regexp-in-string "^;+ ?" "" line))

Or you can use the triple-backtic+lang notation, like so:

```elisp
(defun mrm-strip-comments (line)
  "Strip elisp comments from line"
  (replace-regexp-in-string "^;+ ?" "" line))
```

Remember, if you want to indent code within a list item you need to use
a blank line and 8 spaces. For example:

* I like bananas
* I like pizza

        (eat (make-pizza "pepperoni"))

* I like ice cream with pretty syntax highlighting

```elisp
(eat (make-ice-cream "vanilla"))
```

* I need to go for a run

We convert everything between `;;; Commentary:` and `;;; Code` into
markdown. See make-readme-markdown.el for a full example (you might
already be looking at it... whoa, this is really getting meta...).

If there's some more syntax you would like to see supported, submit
an issue at https://github.com/mgalgs/make-readme-markdown/issues

Many of the functions in this module should probably be "private" (named
with a double-hypen ("--") but are left "public" for illustration
purposes.



### Function and Macro Documentation

#### `(mrm-strip-comments LINE)`

Strip elisp comments from line

#### `(mrm-strip-file-variables LINE)`

Strip elisp file variables from the first LINE in a file.
E.g., `-*- lexical-binding: t; -*-'

#### `(mrm-trim-string LINE)`

Trim spaces from beginning and end of string

#### `(mrm-fix-symbol-references LINE)`

Fix refs like `this` so they don't turn adjacent text into code.

#### `(mrm-make-section LINE LEVEL)`

Makes a markdown section using the `#` syntax.

#### `(mrm-print-section LINE LEVEL)`

Prints a section made with `mrm-make-section`.

#### `(mrm-slurp)`

Read all text from stdin as list of lines

#### `(mrm-wrap-img-tags LINE)`

Wrap image hyperlinks with img tags.

#### `(mrm-print-formatted-line LINE)`

Prints a line formatted as markdown.

#### `(mrm-parse-docs-for-a-thing)`

Searches for the next defun/defmacro/defcustom and prints
markdown documentation.
Returns a list of the form (token token-name title-text docstring).
Example return value:
("defun" "document-a-defmacro" "(document-a-defmacro CODE)" "Takes a defmacro form and..."

#### `(mrm-document-a-defcustom CODE)`

Takes a defcustom form and returns documentation for it as a
string

#### `(mrm-document-a-defun CODE)`

Takes a defun form and returns documentation for it as a string

#### `(mrm-squeeze-spaces TXT)`

Coalesce whitespace.

#### `(mrm-print-badges LINES)`

Print badges for license, package repo, etc.
Tries to parse a license from the comments, printing a badge for
any license found.

#### `(mrm-print-emacs-icon)`

Print emacs icon to generate a fancy README.md.

-----
<div style="padding-top:15px;color: #d0d0d0;">
Markdown README file generated by
<a href="https://github.com/mgalgs/make-readme-markdown">make-readme-markdown.el</a>
</div>
