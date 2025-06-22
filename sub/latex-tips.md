# (La)TeX Tips

## Distributions

### TeX Live

* [SVN repo](https://www.tug.org/svn/texlive/trunk/Master/texmf-dist/), [GitHub mirror](https://github.com/TeX-Live/texlive-source)
  * Revision webview, for example [revision 61285](https://tug.org/svn/texlive?view=revision&revision=61285)
  * Package dependencies, for example [ctex.tlpsrc](https://www.tug.org/svn/texlive/trunk/Master/tlpkg/tlpsrc/ctex.tlpsrc)
    Can be updated by [`DEPENDS.txt`](https://www.tug.org/texlive/pkgcontrib.html#deps).
* [Historic images](ftp://tug.org/historic/systems/texlive/)
* Latest package files (compressed) [distributed by CTAN mirrors](https://ctan.org/tex-archive/systems/texlive/tlnet/archive)

#### `tlmgr`

- TUG page https://www.tug.org/texlive/tlmgr.html
- Online doc https://www.tug.org/texlive/doc/tlmgr.html
- Configuration file `kpsewhich -a texmf.cnf`
  in texlive repo https://github.com/TeX-Live/texlive-source/blob/trunk/texk/kpathsea/texmf.cnf

- Show info of one or more of scheme/collection/package
  https://www.tug.org/texlive/doc/tlmgr.html#info
  ```
  $ tlmgr info [option...] [name...]
  ```
  - `--only-installed`
  - `--list`, list contents
```bash
# FIXME: need no indent to enable highlighting
# list all schemes (installed are prefixed with "!")
tlmgr info schemes

# list all collections
tlmgr info collections

# list all installed packages
tlmgr info --only-installed

# show package info
tlmgr info amsmath

# show content of a scheme/collection/package under "depends" or "run files" key
tlmgr info --list scheme-basic

tlmgr info --list --json collection-basic | jq --compact-output '.[].depends[0:5]'
# ["amsfonts","bibtex","cm","colorprofiles","dvipdfmx"]
```
  - Get space separated list of installed packages (GNU `ggrep` used for its `-P` option):
    ```bash
    $ tlmgr list --only-installed | ggrep -oP '(?<=i )\w+(?=:)' | tr '\n' ' '
    ```

- Show or modify user configurations
  https://www.tug.org/texlive/doc/tlmgr.html#conf
  ```bash
  # show all config settings
  $ tlmgr conf [texmf | tlmgr | updmap]

  # show, set, and delete one `texmf` (TeX and Metafont) config
  $ tlmgr conf texmf max_print_line
  $ tlmgr conf texmf max_print_line 2000
  $ tlmgr conf texmf --delete max_print_line
  ```

#### `texdoc`

- TUG page https://tug.org/texdoc/
- Repo https://github.com/TeX-Live/texdoc
- Doc `texdoc texdoc`
- Remark: command `texdoc` installed by MiKTeX is just a shortcut for an independent program [`mthelp`][miktex-mthelp].

- Show results with corresponding scores
  ```bash
  # long option: texdoc --list --machine amsmath
  $ texdoc -lM amsmath | head -5
  amsmath	11.5	/path/to/texmf-dist/doc/latex/amsmath/amsldoc.pdf	en	User guide (English)
  amsmath	5.5	/path/to/texmf-dist/doc/latex/amsmath/amsmath.pdf
  amsmath	5.0	/path/to/texmf-dist/doc/latex/amsmath/subeqn.pdf	en	Sub-equation usage
  amsmath	5.0	/path/to/texmf-dist/doc/latex/amsmath/technote.pdf	en	Technical details
  amsmath	5.0	/path/to/texmf-dist/doc/latex/amsmath/testmath.pdf	en	Examples paper
  ```

[miktex-mthelp]: https://docs.miktex.org/manual/mthelp.html

#### `kpsewhich`

- `texdoc kpathsea`, sec. 5.6.1 "Path searching options"

<!-- to please markdown parser -->
  ```bash
  # --format
  kpsewhich --format=texmfscripts arara.sh
  ```

### MiKTeX

* MiKTeX now [supports Win/Mac/Linux](https://miktex.org/download), and provides Docker image.
* [Unique TeX features](https://docs.miktex.org/manual/texfeatures.html)
  * automatic package installation: `--disable-installer` and `--enable-installer`
  * list of package usage: `--record-package-usages=<file>`
  * additional input directories: `--include-directory=<dir>`
  * specific directory for auxiliary files: `--aux-directory=<dir>`
* [List of packages](https://miktex.org/packages) contained in MiKTeX, and the current (compressed) packages files [distributed by CTAN mirrors](https://ctan.org/tex-archive/systems/win32/miktex/tm/packages)

### BasicTeX

- https://tug.org/mactex/morepackages.html
  `brew info basictex`

- upgrade basictex

  ```bash
  # backup package list
  tlmgr list --only-installed | sed -e 's/^i \([^:]*\): .*$/\1/' > tl_packages

  brew upgrade basictex

  # set ctan repository
  sudo tlmgr option repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet

  # install docs
  sudo tlmgr option docfiles 1
  sudo tlmgr install --reinstall $(tlmgr list --only-installed | sed -e 's/^i \([^:]*\): .*$/\1/')

  sudo tlmgr install "$(cat tl_packages)"

  # rename versioned directory used by TEXMFVAR and TEXMFCONFIG
  #     TEXMFCONFIG=~/Library/texlive/2023basic/texmf-config
  #     TEXMFVAR=~/Library/texlive/2023basic/texmf-var
  mv ~/Library/texlive/{2023,2024}basic

  # for xetex: (re)create hard links for fonts so they can be found by xetex with family name
  rm -f ~/Library/Fonts/texlive-opentype/*
  rm -f ~/Library/Fonts/texlive-truetype/*
  find `kpsewhich -var-value TEXMFDIST`/fonts/opentype -name '*.otc' -type f \
    -exec ln \{\} ~/Library/Fonts/texlive-opentype \;
  find `kpsewhich -var-value TEXMFDIST`/fonts/truetype -name '*.ttf' -type f \
    -exec ln \{\} ~/Library/Fonts/texlive-truetype \;

  # for luatex
  sudo tlmgr conf texmf OSFONTDIR /System/Library/AssetsV2/com_apple_MobileAsset_Font7
  ```


## Engines and Backends

### general

- width of box overflows at ~`2\maxdimen`
  ```tex
  \documentclass{article}
  \begin{document}
  \setbox0=\hbox{\hskip\maxdimen}
  \the\wd0 % 16383.99998pt

  \setbox0=\hbox{\hskip\maxdimen\hskip\maxdimen}
  \the\wd0 % 32767.99997pt

  \setbox0=\hbox{\hskip\maxdimen\hskip\maxdimen\hskip1pt}
  \the\wd0 % -32767.00003pt, overflows
  \end{document}
  ```
  posted in https://github.com/gpoore/fvextra/issues/28#issuecomment-2510129720, for https://github.com/T-F-S/tcolorbox/issues/304

### pdfTeX

* TUG page: https://www.tug.org/applications/pdftex/
* Mailing List Archives: https://tug.org/pipermail/pdftex/
* Repository: https://tug.org/svn/pdftex/branches/stable
* Release news: on [TUG website](http://www.tug.org/applications/pdftex/NEWS) or from [source repo](http://tug.org/svn/pdftex/branches/stable/source/src/texk/web2c/pdftexdir/NEWS?view=markup)

### XeTeX

* Git repo [hosted on sourceforge](https://sourceforge.net/projects/xetex/) and the mirror [on GitHub](https://github.com/TeX-Live/xetex)
* Precedence of font names used by XeTeX ([ref](https://tex.stackexchange.com/a/43819))
  * `Full name > Family-Style > PostScript name > Family`
  * To let XeTeX auto-load variations, `Family` is recommended
* Show font info: `aat-info.tex` and `opentype-info.tex`, located in `$TEXMFDIST/tex/xetex/xetexfontinfo`

Specials
 - `\special{x:gsave}` and `\special{x:grestore}`: https://tug.org/pipermail/xetex/2004-May/000220.html

### LuaTeX

- the LuaTeX Reference manual
  - source files of the doc
    https://tug.org/svn/texlive/trunk/Master/texmf-dist/doc/luatex/base/
    https://ctan.org/tex-archive/systems/doc/luatex
  - compile from source `context -luatex luatex.tex`
    https://tug.org/pipermail/tex-live/2024-February/049963.html
- primitives
  - `\immediateassignment` and `\immediateassigned`, `texdoc luatex` sec. 2.8.8
    ```tex
    \newcount\mycount

    \edef\TestMe{\advance\mycount 1 foo:\the\mycount}
    \show\TestMe % ->\advance \mycount 1 foo:0.

    \mycount=0
    \edef\TestMe{\immediateassignment\advance\mycount 1 foo:\the\mycount}
    \show\TestMe % ->foo:1.

    \mycount=0
    \edef\TestMe{\immediateassigned{\advance\mycount 1 }foo:\the\mycount}
    \show\TestMe % ->foo:1.
    ```
- Lua
  Lua 5.3.* is used (LuaTeX 1.18.0, TeX Live 2024)
  - `os.execute()` in LuaTeX still returns a single value, representing the exit code
    while the vanilla `os.execute()` returns three values, since Lua 5.2
    https://www.lua.org/manual/5.3/manual.html#pdf-os.execute

### LuaMetaTeX

- uses Lua 5.4
  `texdoc luametatex`, sec. 2.2.1 "The Lua version"

### dvipdfm-x

* Source directory in TeX Live's [SVN repo][dvipdfm-x-svn] and [GitHub mirror][dvipdfm-x-github]
   * Shunsaku Hirata's fork https://github.com/shirat74/dvipdfm-x/
* Mailing list: `dvipdfmx@tug`, [archives](https://tug.org/pipermail/dvipdfmx/)
* Build and test: _Building TeX Live (2020)_, [Sec. 4.5](https://www.tug.org/texlive/doc/tlbuild.html#Build-one-package).
    * Update binary file. Substitute file `xdvipdfmx` resides in `` `kpsewhich --var-value TEXMFDIST`/../bin/<os-dependent-dir> ``.
* Config file: `dvipdfmx.cfg`, located in
    ```bash
    kpsewhich --progname=dvipdfmx --format=othertext [-all] dvipdfmx.cfg
    ```
* Set single letter command line options from inside tex file: `\special{dvipdfmx:config <opt> <val>}`.
    * Example: Use `\special{dvipdfmx:config C 0x0010}` to not optimize PDF destinations. (option `-C` is documented in `texdoc dvipdfmx`, sec. 6.1)

* Undocumented `\special{<cmd>}` commands
  - `pdf:xann` and `dvipdfmx:catch_phantom`\
    https://tug.org/pipermail/dvipdfmx/2020-June/000077.html
  - `pdfcolorstackinit` and `pdfcolorstack`\
    https://tug.org/pipermail/dvipdfmx/2020-September/000105.html


[dvipdfm-x-svn]: https://www.tug.org/svn/texlive/trunk/Build/source/texk/dvipdfm-x/
[dvipdfm-x-github]: https://github.com/TeX-Live/texlive-source/tree/trunk/texk/dvipdfm-x

## Formats

### LaTeX

#### Docs

Only `texdoc` names listed.

- Basic: `source2e`, `classes`, `clsguide`, `usrguide`, `fntguide`, `encguide`
- Per module
  - Hook management\*: `lthooks`, `ltcmdhooks`, `ltfilehook`, `ltshipout`, `ltpara`
  - Other new modules\*: `ltsockets`, `lttemplates`, `ltproperties`, `ltmarks`
  - First aid: `firstaid`
  - see also https://www.latex-project.org/help/documentation/ and https://ctan.org/tex-archive/macros/latex/base
- LaTeX-lab: `documentmetadata-support`\*, `latex-lab-footnotes`, `latex-lab-new-or`, `latex-lab-prototype`, `latex-lab-testphase`
- LaTeX3:
  - `expl3.pdf`, `interface3` (aliases `expl3` and `l3`), `l3doc`, `source3`, `l3backend`
    - due to new `texdoc` aliases added in texdoc v4.1, now `texdoc expl3` opens `interface3.pdf` and to open `expl3.pdf` one needs `texdoc expl3.pdf`.\
      [TeX-Live/texdoc@de1ebc7][texdoc-new-alias] (aliases for expl3 and xparse, requested by LaTeX team, 2024-02-23)
  - `l3prefixes`, `l3styleguide`, `l3syntax-changes`, `l3term-glossary`
  - historic: `l3docstrip` (moved to `docstrip`), `l3news`, `l3news(01..12)`

\*: with variants `-doc` and `-code`

[texdoc-new-alias]: https://github.com/TeX-Live/texdoc/commit/de1ebc7919e3a505988b2d3184df79ff1aec777e

#### Dependencies of `latex.ltx`

- `latex.ltx`
  - `expl3.ltx` (near beginning)
    - `expl3-code.tex`
  - `latex2e-first-aid-for-external-files.ltx` (near end)
- extra
  - `documentmetadata-support.ltx` loaded by `\DocumentMetadata`, usually used _before_ `\documentclass`
  - `l3backend-<engine>.def` loaded at the beginning of `\document`, or in the preamble by `\sys_load_backend:n` or `\sys_ensure_backend:`

#### Format version

- `\fmtversion`, `\patch@level`, and `\ExplFileDate`
- Typeset release info, all-in-one
  ```tex
  % by default "\the\LaTeXReleaseInfo" only writes to log
  {\UseName{@namedef}{show@release@info}#1{#1\par}\ttfamily\the\LaTeXReleaseInfo}
  % LaTeX2e <2024-06-01> patch level 2
  % L3 programming layer <2024-05-27>
  ```

#### Hooks

- Default labels
  - `texdoc lthooks`, sec. 2.1.5 Hook names and default labels
  - `\AddToHook{<hook>}{<code>}` _executed_ in the main document uses label `top-level`; otherwise uses current filename as label
    - More precisely, in files loaded with `\@onefilewithoptions` (thus `\@pushfilename` is invoked), default label is the current filename. In the main document, files loaded with `\input`, `\include`, and `\InputIfFileExists`, default label is `top-level`.
  - current default label can be altered (to non-`top-level`) by `\PushDefaultHookLabel` or `\SetDefaultHookLabel`
  - `\AddToHook{<hook>}{<code>}` has arg-spec `mo+m` hence is equivalent to `\hook_gput_code:non {<hook>} {\c_novalue_tl} {<code>}`, which is further equivalent to `\hook_gput_code:non {<hook>} {.} {<code>}` using the dot-syntax

- Execution order
  code chunks (all other labels), `top-level` label, next invocation
  - reversed hooks uses reversed order: next, `top-level`, chunks
  - ordering rules (`\DeclareHookRule`) only affect execution order _inside_ code chunks
  - next invocation code (`\AddToHookNext`) is not orderable

#### Commands for class and package writers

* `\disable@package@load{<pkg>}{<alternate-code>}`, requires LaTeX2e 2020/10/01

#### Notable LaTeX NEWS

- ltnews28, 2018-04-01, A general rollback concept for packages and classes
  - example usage
    ```tex
    % in "doc.pkg"
    \DeclareRelease{v2}{2021-06-01}{doc-2021-06-01.sty}

    % in "main.tex"
    \usepackage{doc}[=v2]
    ```
  - Full description: Frank Mittelbach, [A rollback concept for packages and classes](https://www.latex-project.org/publications/2018-FMi-TUB-tb122mitt-version-rollback.pdf)

### ConTeXt

* Recommended ConTeXt Docs, [Q&A on TeX-SX](https://tex.stackexchange.com/questions/2839/where-can-i-find-good-context-documentation)
* Auto-generated [list](http://www.pragma-ade.com/general/qrcs/setup-en.pdf) of all commands, without descriptions
* Renamed primitives `\normal<primitive csname>` (done in source files `norm-*.mkii` in `TEXMFDIST/tex/context/base/mkii/`)


## Fonts

### Font encodings

* Main doc: `texdoc fontenc`
* EU1 encoding (`eu1enc.def`): provided by package `euenc`
* T3 encoding (`t3enc.def`): provided by package `tipa`

### [xetex] Show full path of fonts

Add `\XeTeXtracingfonts=1` before `\documentclass`, and find full path of fonts in `.log`, like
```
Requested font "[lmroman10-bold]:mapping=tex-text;" at 10.0pt
 -> /usr/local/texlive/2018basic/texmf-dist/fonts/opentype/public/lm/lmroman10-
bold.otf
```

### [macOS] Using extra fonts with `lualatex`

`lualatex` can use fonts in directory `/Library/Fonts`, but not those added to new library of `Font Book.app`.
```bash
$ tlmgr conf texmf OSFONTDIR <path>
```

## Tools

### `explcheck`

- Use developing `explcheck`
  ```bash
  cd /path/to/explcheck-repo
  # generate "explcheck-obsolete.lua" from l3kernel "l3obsolete.txt"
  l3build tag
  export LUAINPUTS=explcheck/src
  texlua explcheck/src/explcheck.lua <options> <files>
  # or
  # texlua "$(kpsewhich explcheck.lua)" <options> <files>
  ```

## Packages

### HTML docs

- tikz https://tikz.dev/
- pgfplots https://tikz.dev/pgfplots
- beamer https://www.beamer.plus/home.html

### `latex3`

CHANGELOG
  - [`l3kernel`](https://github.com/latex3/latex3/blob/master/l3kernel/CHANGELOG.md)
  - [`l3packages`](https://github.com/latex3/latex3/blob/master/l3packages/CHANGELOG.md)

Docs
  - `l3backend`
  - `l3kernel` (directory `$(kpsewhich --var-value TEXMFDIST)/doc/latex/l3kernel`)
    - `expl3.pdf`, `interface3.pdf`
    - `l3term-glossary.pdf`, `l3styleguide.pdf`
    - `l3news.pdf`, `l3syntax-changes.pdf`, `l3obsolete.pdf`
    - `l3prefixes.pdf`
    - `l3doc.pdf` (for `l3doc.cls`), `l3docstrip.pdf` (stub file, see `docstrip.pdf`)
  - `l3experimental`
  - `l3packages`
  - `l3trial`

Modules
  - `l3debug.dtx`, generates `l3debug.def` which will be loaded by `\sys_load_debug:`
    - no `l3debug.pdf` generated nor part of `interface3.pdf` as of 2023-05
    - in `l3debug.def`, `\debug_on:n` is redefined to do the real setting.
    - starting from `l3kernel` 2023-05-23 (commit [1ec8adb6](https://github.com/latex3/latex3/commit/1ec8adb66edbdf480d0a8fcfed25de146d94f271)), `\sys_load_debug:` is integrated into `\debug_on:n`.
    - enable debugging without explicitly loading `expl3` package
      ```tex
      %% before
      \sys_load_debug:
      \debug_on:n { ... }
      %% from `l3kernel` 2023-05-23 on
      \debug_on:n { check-declarations , check-expressions , deprecation , log-functions }
      % or equivalently
      \debug_on:n { all }
      ```


### Workaround: use `tikzmark` package with `xelatex`

According to [discussions on TeX-SX](https://tex.stackexchange.com/questions/229500/) and [the bug report to `pgf` project](https://sourceforge.net/p/pgf/bugs/354/), this is a `pgf` driver bug. A workaround, which recovers the definition of `\pgfsys@hboxsynced` from `dvipdfmx` version to the common driver version, is firstly suggested in [an answer on TeX-SX](https://sourceforge.net/p/pgf/bugs/354/#72e2).

Workaround: redefine `\pgfsys@hboxsynced`, see below
```latex
\usepackage{tikz}
\usetikzlibrary{tikzmark}
% WORKAROUND:
% Definition copied from pgfsys-common-pdf-via-dvi.def
% http://tex.stackexchange.com/a/339975/
% Compare http://tex.stackexchange.com/q/229500 and comments!
\makeatletter
\def\pgfsys@hboxsynced#1{%
  {%
    \pgfsys@beginscope%
    \setbox\pgf@hbox=\hbox{%
      \hskip\pgf@pt@x%
      \raise\pgf@pt@y\hbox{%
        \pgf@pt@x=0pt%
        \pgf@pt@y=0pt%
        \special{pdf: content q}%
        \pgflowlevelsynccm%
        \pgfsys@invoke{q -1 0 0 -1 0 0 cm}%
        \special{pdf: content -1 0 0 -1 0 0 cm q}% translate to original coordinate system
        \pgfsys@invoke{0 J [] 0 d}% reset line cap and dash
        \wd#1=0pt%
        \ht#1=0pt%
        \dp#1=0pt%
        \box#1%
        \pgfsys@invoke{n Q Q Q}%
      }%
      \hss%
    }%
    \wd\pgf@hbox=0pt%
    \ht\pgf@hbox=0pt%
    \dp\pgf@hbox=0pt%
    \pgfsys@hbox\pgf@hbox%
    \pgfsys@endscope%
  }%
}
\makeatother
```

### Workaround: use `tikz-feynman` package with `lualatex`

According to the [discussions (and workaround) under project `luaotfload`](https://github.com/u-fischer/luaotfload/issues/6), an updated version of function `pgf_lookup_and_require` causes this problem. The [bug](https://sourceforge.net/p/pgf/bugs/493/) has been reported to `pgf`.

```latex
\documentclass{article}
\usepackage{tikz}
\usetikzlibrary{graphdrawing}

\usepackage{luacode}
\begin{luacode*}
function pgf_lookup_and_require(name)
    local sep = package.config:sub(1,1)
    local function lookup(name)
        local sub = name:gsub('%.',sep)
        if kpse.find_file(sub, 'lua') then
            require(name)
        elseif kpse.find_file(sub, 'clua') then
            collectgarbage('stop')
            require(name)
            collectgarbage('restart')
        else
            return false
        end
        return true
    end
    return
        lookup('pgf.gd.' .. name .. '.library') or
        lookup('pgf.gd.' .. name) or
        lookup(name .. '.library') or
        lookup(name)
end
\end{luacode*}

\usegdlibrary{circular}
\usepackage[compat=1.1.0]{tikz-feynman}
\begin{document}
% normal feynman usage
\feynmandiagram[ ... ]{ ... };
\end{document}
```

### [amsmath] [amstext] Commands silenced by `\text`

`\text` uses `\mathchoice` to typeset text in current math style. To avoid 4 executions of specific codes, in the three non-`\displaystyle` cases, following commands are silenced

- `\stepcounter` and `\addtocounter`
- `\GenericInfo`, `\GenericWarning`, and `\GenericError`

Most recently hit when digging into
https://github.com/gpoore/minted/issues/423 "minted loops in amsmath \text".

BTW, since 2016, both `amsmath` and `amstext` are maintained in the latex2e repo, under `./required/amsmath`.
https://github.com/latex3/latex2e/commits/develop/required/amsmath/

### [graphics bundle] Graphics driver may change page size to letter paper

By default TeX Live chooses to use A4 paper (check stdout of `tlmgr paper`), but LaTeX wants to use letter paper. When graphics driver is not loaded, TeX Live wins; otherwise, LaTeX wins.
https://github.com/latex3/graphics-def/issues/36

### [hyperref] Allow `unicode-math` math symbols in bookmark

```latex
\hypersetup{
    psdextra = true,
    unicode  = true
}
```

### [hyperref] Package options
 - Package option: `hyperindex`


### [hyperref] Package internals

 - backend files: `hpdftex.def`, `hxetex.def`, `hluatex.def`
 - common internals:
```tex
\section{title}\label{key}

\ref{key} -> \hyper@link{link}{section.1}{1}
```

```tex
\contentsline -> \hyper@linkstart
biblatex.sty: \hyper@natlinkstart -> \hyper@linkstart
```

```tex
% only in hxetex.def, \hyper@link depends \hyper@linkstart
\def\hyper@link#1#2#3{%
  \hyper@linkstart{#1}{#2}#3\Hy@xspace@end\hyper@linkend
}
```

### [hyperref] Page hyperlinks in index

Auto page hyperlinks added to index entries may affect order of row pages and encaped pages. A special `.ist` can be used to overcome this, see [gh/latex3/hyperref#243].

```tex
% special .ist file
\begin{filecontents}[noheader, force]{hyperpage.ist}
delim_0 ", \\hyperpageIdx{"
delim_1 ", \\hyperpageIdx{"
delim_2 ", \\hyperpageIdx{"
delim_n "}\\nil, \\hyperpageIdx{"
delim_t "}\\nil"
encap_prefix "\\"
encap_infix "}{{"
encap_suffix "}"
\end{filecontents}

% preamble settings
\usepackage{etoolbox} % for \ifstrempty
\usepackage{makeidx}
\makeindex
\usepackage[hyperindex=false]{hyperref}

% no encap,   e.g., \hyperpageIdx{1}\relax
% with encap, e.g., \hyperpageIdx{\seealso{bar}}{{2}}\relax
\def\hyperpageIdx#1#2\nil{%
  \ifstrempty{#2}
    {\hyperpage{#1}}
    {\ignorespaces#1{\hyperpage#2}}%
}
```

### [fontspec] Use with math font packages

Pass `no-math` option to `fontspec`, e.g.,
```latex
\usepackage[no-math]{fontspec}
\usepackage{stix2}
```

ref: [answer on TeX-SX](https://tex.stackexchange.com/a/69354) by Heiko Oberdiek

### [beamer] Doc of `pgfpicture` environment

Manual of `pgf` only documents `pgfpicture` environment without any arguments, while source code of `beamer` uses an old version of that environment,

The usage of `pgfpicture` environment in `beamer`, with four braced arguments, is defined in `pgfcorescopes.code.tex` and not documented in pgf manual yet.

### [minted] Highlight `@` as command name letter

Modify python class `TeXLexer` in file [pygments/lexers/markup.py](https://github.com/dagwieers/pygments/blob/master/pygments/lexers/markup.py).

```python
class TexLexer(RegexLexer):
    # [... ...]
    tokens = {
        # [...]
        'root': [
            (r'\\\[', String.Backtick, 'displaymath'),
            (r'\\\(', String, 'inlinemath'),
            (r'\$\$', String.Backtick, 'displaymath'),
            (r'\$', String, 'inlinemath'),
            # (r'\\([a-zA-Z]+|.)', Keyword, 'command'),  # before
            (r'\\([a-zA-Z@]+|.)', Keyword, 'command'),  # after
            (r'\\$', Keyword),
            include('general'),
            (r'[^\\$%&_^{}]+', Text),
        ],
        'math': [
            # (r'\\([a-zA-Z]+|.)', Name.Variable), # before
            (r'\\([a-zA-Z@]+|.)', Name.Variable),  # after
            include('general'),
            (r'[0-9]+', Number),
            (r'[-=!+*/()\[\]]', Operator),
            (r'[^=!+*/()\[\]\\$%&_^{}0-9-]+', Name.Builtin),
        ],
        # [...]
    }
    # [... ...]
```

Alternatively, provide a custom lexer.
```python
#!/usr/bin/env python3

from pygments.lexers.markup import TexLexer
from typing import List, Tuple


def update_token_regex(token: List[Tuple], idx,
                       regex_old=r'\\([a-zA-Z]+|.)',
                       regex_new=r'\\([a-zA-Z@]+|.)'):
    node = token[idx]
    idx2 = 0  # index of regex str
    if node[idx2] == regex_old:
        node = list(node)
        node[idx2] = regex_new
        token[idx] = tuple(node)


class TexLexer2(TexLexer):
    """
    Improved lexer for the TeX and LaTeX typesetting languages.
    Character "@" is treated part of command names.
    """

    tokens = TexLexer.tokens
    update_token_regex(tokens['root'], 4)
    update_token_regex(tokens['math'], 0)
```

Then modify `minted` (see [minted/176](https://github.com/gpoore/minted/issues/176)) to execute `pygmentize` with custom lexer and/or formatter.

### [listings] Reset output style for `-` (U+002D)

By default, `listings` prints character `-` as in text mode if the current font family is `\ttfamily` and as in math mode (`$-$`) otherwise. One can [overwrite this scheme](https://tex.stackexchange.com/a/424193) by
```latex
\makeatletter
\lst@CCPutMacro
    \lst@ProcessOther{"2D}{-{}}
    \@empty\z@\@empty
\makeatother
```

### [listings] `literate`d input absorbs following space in `(full)flexible` mode

Example (with [workaround](https://tex.stackexchange.com/a/445764) included):
```latex
\documentclass{article}
\usepackage{listings}
\usepackage{etoolbox}

% adapted from example in `texdoc listings`, Sec. 5.4
\lstset{
  literate={:=}{{$\gets$}}1 {<=}{{$\leq$}}1
}

\makeatletter
\patchcmd\lst@Literate
  {\lst@XPrintToken}
  {\lst@XPrintToken\lst@whitespacefalse}
  {}{\fail}
\makeatother

\begin{document}
\begin{lstlisting}[columns=flexible]
flexible:
  if (i <= 0) i := 1;
\end{lstlisting}

\begin{lstlisting}[columns=fullflexible]
fullflexible:
  if (i <= 0) i := 1;
\end{lstlisting}
\end{document}
```

### [memoize] Install required Perl library `PDF::API2`

```
$ sudo cpan PDF::API2
```
This is easier than installing it through `perlbrew`, and works for TeXstudio with no `PATH` modifications.

### [equation] Disregard indent of displayed equations inside list env

```latex
\everydisplay\expandafter{%
  \the\everydisplay
  \displayindent=0pt%
  \displaywidth=\hsize
}
```

### [pstricks] Get Ghostscript permission required by `xdvipdfmx`

From:
 - https://tex.stackexchange.com/a/554111
 - https://discourse.brew.sh/t/mactex-ghostscript-permission-issue-on-macos-10-15-catalina/6086

Example:
```tex
\documentclass{article}
\usepackage{pstricks}

\begin{document}
\pscircle(.5,.5){1.5}
\end{document}
```
`xelatex` produces
```tex
GPL Ghostscript 9.52: Unrecoverable error, exit code 1
```
`xdvipdfmx` produces more detailed
```
[1Error: /invalidfileaccess in --run--

[...]

Last OS error: Permission denied
Current file position is 74
GPL Ghostscript 9.52: Unrecoverable error, exit code 1

xdvipdfmx:warning: Filtering file via command -->[...]<-- failed.
xdvipdfmx:warning: Image format conversion for PSTricks failed.
```

 - Ghostscript uses flag `-dSAFER` by default from [9.50](https://www.ghostscript.com/doc/9.50/History9.htm#Version9.50), which forbids reading `pstricks` header files located in `$TEXMFDIST/dvips/pstricks`.
 - Edit `dvipdfmx.cfg` by inserting `-dNOSAFER` to line starting with `D  "rungs -q`.
 - Flag `-dNOSAFER` or finer `--permit-file-read` can be added to TeX Live's wrapper `rungs`, which is a Lua script.

### [pythontex]

```bash
xelatex main.tex
# Ideally, "pythontex main" is enough.  But since pythontex.py starts with
#     #!/usr/bin/env python
# pythontex will call python2, which has no pygments to use.
python3 `which pythontex` main
xelatex main.tex
```

### Replacements

- [moloch] Revival (fork) of Metropolis beamer theme
  - intro https://jolars.co/blog/2024-05-30-moloch/
  - repo https://github.com/jolars/moloch
  - CTAN https://ctan.org/pkg/moloch
  - seen from https://github.com/samcarter#2024

## Topics: table

Variable Rule width
 - vertical rule: `!{\vrule width 2pt}`
 - horizontal rule: `\specialrule{2pt}{0em}{0em}` from `booktabs`, compatible with `longtable`\
   Full syntax: `\specialrule{<width>}{<above skip>}{<below skip>}`
 - see example [tex-sx/646876](https://tex.stackexchange.com/a/646876/)


## PDF

### Produce uncompressed PDF

```tex
\sys_ensure_backend:
\pdf_uncompress:
% or
\DocumentMetadata{uncompress}
```
https://tex.stackexchange.com/a/581841

old
```tex
\ifdefined\directlua
  % if luatex
  \edef\pdfcompresslevel{\pdfvariable compresslevel}
  \edef\pdfobjcompresslevel{\pdfvariable objcompresslevel}
\fi
\ifdefined\pdfcompresslevel
  % if pdftex and luatex
  \pdfcompresslevel=0
  \pdfobjcompresslevel=0
\else
  % if xetex
  \special{dvipdfmx:config z 0}
\fi
```


## General

* Category code<br />
  TeX doesn't change category codes of once tokenized text (unless you use `\scantokens`).
  ([question comment](https://tex.stackexchange.com/q/498864/#comment1259946_498864) on TeX-SX)
* Behavior of `\uppercase` and `\lowercase`<br />
  The uppercase/lowercase table in tex (unfortunately) is fixed based on the T1 font encoding.
  ([issue comment](https://github.com/latex3/latex2e/issues/103#issuecomment-450581689) on GitHub)
* Special tokens
  * frozen `\relax` ([`texdoc interface3`, sec XVI.7](https://github.com/latex3/latex3/blob/c297e780ff706dab7b30f9ad5153f2f58f542de9/l3kernel/l3token.dtx#L1162-L1164) and [TeX-SX Q&A](https://tex.stackexchange.com/a/57417))
* Significance of MWE<br />
  Writing up a good issue report including a clear MWE (Minimal Working Example) takes some effort, but it is also essential to help us identifying and fixing issues. ([LaTeX news article](https://www.latex-project.org/news/2018/12/10/issue29-of-latex2e-news-released/) on latex-project.org)
* How to report bug to LaTeX2e core<br />
  Details on how to report bugs can be found in the article ["New rules for reporting bugs in the LaTeX core software"](https://www.latex-project.org/publications/2018-FMi-TUB-tb121mitt-bug-reporting.pdf). ([news article](https://www.latex-project.org/news/2018/12/10/issue29-of-latex2e-news-released/) on latex-project.org)
* Purposes of initializing `.aux` file with a `\relax`<br />
  1) catch impossible-to-write error as soon as possible  2) ensure `.aux` file is never empty ([answer](https://tex.stackexchange.com/a/398751) on TeX-SX)
* PGF-TikZ
  * TikZ pictures can't be nested. Sometimes it works but that is a pure coincidence. [by hmenke](https://github.com/pgf-tikz/pgf/issues/743#issuecomment-530999926), one of maintainers of tikz
  * `tkz-euclide` and `tkz-base` are just 4k+ lines. They uses deprecated `arrows` tikz library, and `fp` package instead of `fpu` tikz library.
* l3kernel
  * Either fully-expandable or `\protected`
    > The LaTeX3 approach is that everything should be fully-expandable or `\protected`
    >
    > [by Joseph Wright](https://github.com/latex3/latex3/issues/375#issuecomment-310894595)
* l3packages/xtemplate
    > It's pretty clear that xtemplate is a set of good ideas but won't go forward: I'm not sure what's best here.
    >
    > [by Joseph Wright](https://github.com/latex3/latex3/issues/656#issuecomment-571098206)

    > meaning that most of us (all? I?) think that `xtemplate` was a good prototype with good ideas but not necessarily the best or the most appropriate ones in the end and that by the end of the days the final interface will look different even though it will probably incoprporate several of the ideas in xtemplate in one way or the other. And that's what Joseph referred to.
  >
  > by Frank Mittelbach
* DocStrip
  * `\endinput` has a dual purpose: if used at the start of the line it is interpret as "please end the file here"; in any other position it is considered part of code and will be handled like any other code that is docstripped. So if you need `\endinput` as part of your code make sure it is not at the beginning of a line. ---[by Frank Mittelbach](https://github.com/latex3/latex2e/issues/244#issuecomment-571224849)

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

Record file I/O
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

Logging message
  * Understand log of an [overfull `\hbox`](https://tex.stackexchange.com/a/171219)

PGF/TikZ
  * [Reflecting a line and/or point with named coordinates](https://tex.stackexchange.com/q/467295)

Color Stack
  * In split footnote, [latex2e#1](https://github.com/latex3/latex2e/issues/1)

Meta
  * [Minimal working example](https://tex.meta.stackexchange.com/q/228) (MWE), what and how
    * [Prefer `article` to `minimal` LaTeX class](https://tex.stackexchange.com/a/42115) in MWE
  * [How to upgrade TeX distribution?](https://tex.stackexchange.com/q/55437)

### TeX in Other Programming Languages

#### JavaTeX Project, by Timothy Murphy

An introduction to this Project is published on [1999 TUGboat](https://www.tug.org/TUG99-web/pdf/murphy.pdf). This project is also uploaded to CTAN as a package [named `javatex`](https://ctan.org/pkg/javatex). The performance is so poor that the author "considers it unusable in practice", by its readme on CTAN.

## Journals

[The PracTeX Journal](http://tug.org/pracjourn/2012-1/toc.html), the online journal of the TeX Users Group, has published [20 issues](http://tug.org/pracjourn/archive.html) from 2005 to 2012.

## Books

### The LaTeX Companion, 3rd Edition (tlc3)

- Examples https://github.com/FrankMittelbach/tlc3-examples
- Errata https://github.com/latex3/latex2e/blob/develop/base/doc/tlc3.err
