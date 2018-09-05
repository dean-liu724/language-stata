# Stata language support in Atom
[![Installs!](https://img.shields.io/apm/dm/language-stata.svg?style=flat-square)](https://atom.io/packages/language-stata)
[![Version!](https://img.shields.io/apm/v/language-stata.svg?style=flat-square)](https://atom.io/packages/language-stata)
[![License](https://img.shields.io/apm/l/language-stata.svg?style=flat-square)](https://github.com/kylebarron/language-stata/blob/master/LICENSE)

[![GitHub stars](https://img.shields.io/github/stars/kylebarron/language-stata.svg?style=social&label=Star)](https://github.com/kylebarron/language-stata)
[![GitHub forks](https://img.shields.io/github/forks/kylebarron/language-stata.svg?style=social&label=Fork)](https://github.com/kylebarron/language-stata)

- [News](#news)
- [Features](#features)
- [Installation](#installation)
- [Dynamic Documents](#dynamic-documents)
- [Running Code](#running-code)

## News

Use the new [Stata Jupyter Kernel](https://kylebarron.github.io/stata_kernel/) with Atom's [Hydrogen package](https://atom.io/packages/Hydrogen) to show Stata results inline. It works with Windows, macOS, and Linux. Example gif uses the [Atom Material Syntax](https://github.com/atom-material/atom-material-syntax) theme and [Fira Code](https://github.com/tonsky/FiraCode) font.

![](./img/stata_kernel_example.gif)

## Features

This package highlights:

- System commands, functions, and function arguments
- Macros, both global and local
    - Accurately colors nested macros and escaped macros in strings when you want the inner macro to evaluate at runtime
    - Colors macro extended functions inside `` `: ... '`` as well as after `local lname:`
- Comments, [more accurately than Stata's Do-file Editor](examples/comments.md).
- Regular expressions
    - Colors both the limited syntax provided through the `regexr()` and `regexm()` functions, as well as the vastly expanded regex syntax provided in Stata 14 and 15 through the `ustrregexm()`, `ustrregexrf()`, and `ustrregexra()` functions.
- Dynamic Markdown and LaTeX documents. [Instructions below.](#dynamic-documents)

Other nice features:

- Works with unicode identifiers. Use unicode anywhere it's legal Stata syntax.
- Autocomplete for built-in commands and functions, and for your macros as you write them.
- Alerts you if your variable name is illegal, i.e. if your variable name is more than 32 chars, starts with a number, or is a reserved name.
- Alerts you if you have any text other than } on a line ending a foreach/forvalues/if/else command
- Local macro back tick autocompletion. When you write a `, Atom automatically fills in a ' after your cursor
- Makes it easy to spot incorrect nesting of compound quotes
- Support for programming ligatures for all valid Stata syntax for fonts that support them, like the [Fira Code](https://github.com/tonsky/FiraCode) font.
- Highlights SQL queries used in the [`odbc`](https://www.stata.com/help.cgi?odbc) command. (The `language-sql` base package must be active.)
- Highlights [Docblockr](https://atom.io/packages/docblockr)-style keywords inside comments (anything like `@Note`)

## Installation

To install, do one of the following:

- Go to Preferences/Settings > Install > Packages; and then search for `language-stata`
- At the command line, type `apm install language-stata`

The local macro back tick autocompletion won't function until you fully restart Atom. Do `ctrl-shift-P` or `cmd-shift-P` to bring up the command palette, type `Window: Reload`, and click enter.

## Configuration

Atom allows you to toggle whether a line is commented using <kbd>ctrl</kbd>+<kbd>/</kbd>. As of version 1.6.5, the comment character this uses is `// ` by default. You can change this to use `/* */` or `*` characters to comment lines.

To change to `/* */` comments, you can put the following in your [`config.cson`](https://flight-manual.atom.io/using-atom/sections/basic-customization/) file.

```cson
'.source.stata':
  editor:
    commentStart: '/* '
    commentEnd: ' */'
```

To change to `*` comments, use the following. However I don't recommend using this character<sup>[1](#myfootnote1)</sup>.

```cson
'.source.stata':
  editor:
    commentStart: '* '
```

Note that in your `config.cson` file there can only be a single `'.source.stata'` top-level key, and only a single `editor` key under `'.source.stata'`. If you customize some other settings within the Stata grammar, you might already have a `'.source.stata'` key, and thus you would add the `commentStart` key to it.

## Dynamic Documents

![](img/dyntext_domd.png)

Stata 15 brought new features for working with dynamic documents. The [`dyndoc`](https://www.stata.com/help.cgi?dyndoc) command lets you write in Markdown and converts your file and code to HTML for viewing in a web browser.

It also added the [`dyntext`](https://www.stata.com/help.cgi?dyntext) command, which fills in Stata output for any text file, without touching the text itself. This lets you then use third-party document generators like [Pandoc](https://pandoc.org/) and [LaTeX](https://www.latex-project.org/) to generate documents.

This package now provides syntax highlighting for Stata code written inside Stata's [dynamic tags](https://www.stata.com/help.cgi?dynamic+tags) for Markdown and LaTeX documents.

By default, this package's Markdown and LaTeX syntax highlighting will be applied for files ending in `.domd` and `.dotex` respectively. **The [language-markdown](https://atom.io/packages/language-markdown) and [language-latex](https://atom.io/packages/language-latex) packages must be installed for the highlighting to work.**

If you name your file with a different extension, you can manually set the highlighting by clicking on the "Plain Text" button on the bottom right of the screen (or by pressing <kbd>CTRL</kbd>+<kbd>SHIFT</kbd>+<kbd>L</kbd>) and then selecting `Stata Dyndoc (Markdown)` or `Stata Dyndoc (LaTeX)` from the drop-down menu.

### Examples

An example of the PDF output of using `dyntext` and Pandoc is in the examples folder: [`dyntext.pdf`](examples/dyntext.pdf).

That file was created by running

```stata
dyntext dyntext.domd, saving(dyntext.md) replace
```
from inside Stata 15, and then with

```
pandoc dyntext.md -o dyntext.pdf
```

on the command line using [Pandoc](https://pandoc.org/).

The file [`dyntext.dotex`](examples/dyntext.dotex) is a proof-of-concept and should compile with LaTeX but the output is not shown here.

## Running Code

The [`stata-exec`](https://atom.io/packages/stata-exec) package has functionality to run Stata code on Windows, macOS, and Linux.

#### Footnotes

<a name="myfootnote1">1</a>: The following code is legal Stata code, but Atom will confuse the `*` used as multiplication with the `*` used for a comment. So if your cursor is on the second line and you press <kbd>ctrl</kbd>+<kbd>/</kbd>, Atom will remove the `*` symbol and the semantic meaning of the multiplication will be lost. Thus using `// ` as the comment symbol is safer.

```stata
display 2 ///
  * 2
```
