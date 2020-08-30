# Something May Help in the Future

TeX Core
  * Full [list of primitives](https://zhuanlan.zhihu.com/p/59151033) by engine
  * [List of tracing commands](https://tex.stackexchange.com/a/60494)
  * `whatsits`
    * ... represent commands whose execution is delayed or are special commands associated with a particular device or system and are not part of TeX's normal processing flow. ([ref](https://tex.stackexchange.com/a/43174))
    * _TeX by Topic_, sec. 30.3

Expansion
  * [List of expandable primitives](https://tex.stackexchange.com/a/467372)
    * For expandable TeX primitives, see also *TeX by Topic*, Sec. 12.2.
  * Use `\romannumeral` to get full expansion [[1](http://texhacks.blogspot.com/2010/12/forcing-full-expansion.html), [2](https://www.texdev.net/2011/07/05/expansion-using-romannumeral/)]

Record file i/o
  * `-recorder` command line option<br />
     record every file input and output into `.fls` file
  * `\listfiles` control sequence<br />
     record every file input caused by `\input{...}` and `\include` (but not `\input <filename>`) into the very end of `.log` file

Font
  * OpenType Math Fonts
    * BachoTEX 2009, Ulrik Vieth. OpenType Math Illuminated ([preprint](https://www.tug.org/~vieth/papers/bachotex2009/ot-math-paper.pdf), [tugboat vol. 30](http://tug.org/TUGboat/tb30-1/tb94vieth.pdf))
    * BachoTEX 2019, Ulrik Vieth. [OpenType Math Fonts: What’s new or noteworthy?](http://www.gust.org.pl/bachotex/2019-pl/presentations/uvieth-1-2019.pdf)
    * 2018, Xiangdong Zeng. [Bibliography](https://github.com/firamath/firamath.github.io/blob/master/bibliography.md) of font firamath

Equation
  * [Multiline Root Symbol (`\sqrt`)](https://tex.stackexchange.com/a/111433)
  * [`amsmath`] Use `\ifmeasuring@` to determine the current phase (measure or production), see [example](https://tex.stackexchange.com/a/548004)

Log message
  * Understand log of an [overfull `\hbox`](https://tex.stackexchange.com/a/171219)

PGF/TikZ
  * [Reflecting a line and/or point with named coordinates](https://tex.stackexchange.com/q/467295)

Color Stack
  * In split footnote, [latex2e#1](https://github.com/latex3/latex2e/issues/1)

Meta
  * [Minimal working example](https://tex.meta.stackexchange.com/q/228) (MWE), what and how
    * [Prefer `article` to `minimal` LaTeX class](https://tex.stackexchange.com/a/42115) in MWE
  * [How to upgrade TeX distribution?](https://tex.stackexchange.com/q/55437)

## TeX Live

* [SVN repo](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/), [GitHub mirror](https://github.com/TeX-Live/texlive-source)
* [Historic images](ftp://tug.org/historic/systems/texlive/)
* Latest package files (compressed) [distributed by CTAN mirrors](https://ctan.org/tex-archive/systems/texlive/tlnet/archive)

### `tlmgr` Usages

* Show list of request type
  ```bash
  # list of schemes
  $ tlmgr info schemes

  # list of collections
  $ tlmgr info collections

  # list of all packages
  $ tlmgr info
  ```
   - With option `--only-installed`, only installed items are shown.
* Show contents of specific item
  ```bash
  # list contents of specified package
  $ tlmgr info --list <pkg-name>

  # list contents of specified scheme
  # e.g., tlmgr info --list scheme-medium
  $ tlmgr info --list <scheme-name>

  # list contents of specified collection
  $ tlmgr info --list <collection-name>
  ```
* Set `max_print_line` (see explanations in [texstudio-org/texstudio#325 (comment)](https://github.com/texstudio-org/texstudio/issues/325#issuecomment-649918868))
  ```bash
  # show
  tlmgr conf texmf max_print_line

  # set
  tlmgr conf texmf max_print_line 2000

  # delete
  tlmgr conf texmf --delete max_print_line
  ```
* Get space separated list of installed packages (use GNU `ggrep` for its `-P` option):
  ```bash
  $ tlmgr list --only-installed | ggrep -oP '(?<=i )\w+(?=:)' | tr '\n' ' '
  ```

## pdfTeX

* TUG page: https://www.tug.org/applications/pdftex/
* Mailing List Archives: https://tug.org/pipermail/pdftex/
* Repository: https://tug.org/svn/pdftex/branches/stable
* Release news: on [TUG website](http://www.tug.org/applications/pdftex/NEWS) or from [source repo](http://tug.org/svn/pdftex/branches/stable/source/src/texk/web2c/pdftexdir/NEWS?view=markup)

## XeTeX

* Git repo [hosted on sourceforge](https://sourceforge.net/projects/xetex/) and the mirror [on GitHub](https://github.com/TeX-Live/xetex)
* Precedence of font names used by XeTeX ([ref](https://tex.stackexchange.com/a/43819))
  * `Full name > Family-Style > PostScript name > Family`
  * To let XeTeX auto-load variations, `Family` is recommended
* Show font info: `aat-info.tex` and `opentype-info.tex`, located in `$TEXMFDIST/tex/xetex/xetexfontinfo`

## dvipdfm-x

* [SVN repo](https://www.tug.org/svn/texlive/trunk/Build/source/texk/dvipdfm-x/) (as part of TeX Live) and [GitHub repo](https://github.com/shirat74/dvipdfm-x/) of Shunsaku Hirata
* Build and test: _Building TeX Live (2020)_, [Sec. 4.5](https://www.tug.org/texlive/doc/tlbuild.html#Build-one-package).
    * Update binary file. Substitute file `xdvipdfmx` resides in `` `kpsewhich --var-value TEXMFDIST`/../bin/<os-dependent-dir> ``.
* Config file: `dvipdfmx.cfg`, located in
    ```bash
    kpsewhich --progname=dvipdfmx --format=othertext [-all] dvipdfmx.cfg
    ```
* Set single letter command line options from inside tex file: `\special{dvipdfmx:config <opt> <val>}`.
    * Example: Use `\special{dvipdfmx:config C 0x0010}` to not optimize PDF destinations. (option `-C` is documented in `texdoc dvipdfmx`, sec. 6.1)

## MiKTeX - A LaTeX Distribution

* MiKTeX now [supports Win/Mac/Linux](https://miktex.org/download), and provides Docker image.
* Unique TeX features, see [doc](https://docs.miktex.org/2.9/manual/texfeatures.html) for full description
  * automatic package installation
  * list of package usage
  * additional input directories
  * specific directory for auxiliary files
* [List of packages](https://miktex.org/packages) contained in MiKTeX, and the current (compressed) packages files [distributed by CTAN mirrors](https://ctan.org/tex-archive/systems/win32/miktex/tm/packages)

## ConTeXt - A LaTeX Format

* Recommended ConTeXt Docs, [Q&A on TeX-SX](https://tex.stackexchange.com/questions/2839/where-can-i-find-good-context-documentation)
* Auto-generated [list](http://www.pragma-ade.com/general/qrcs/setup-en.pdf) of all commands, without descriptions

## TeX in Other Programming Languages

### JavaTeX Project, by Timothy Murphy

An introduction to this Project is published on [1999 TUGboat](https://www.tug.org/TUG99-web/pdf/murphy.pdf). This project is also uploaded to CTAN as a package [named `javatex`](https://ctan.org/pkg/javatex?lang=en). The performance is so poor that the author "considers it unusable in practice", by its readme on CTAN.

## Journal

[The PracTeX Journal](http://tug.org/pracjourn/2012-1/toc.html), the online journal of the TeX Users Group, has published [20 issues](http://tug.org/pracjourn/archive.html) from 2005 to 2012.
