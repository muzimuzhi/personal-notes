# LaTeX Tips

## Format

### LaTeX

#### Docs

Only `texdoc` names listed.

 - Basic: `source2e`, `classes`, `usrguide`, `fntguide`, `encguide`
 - LaTeX3: `interface3`, `source3`, `expl3`
 - Hook management\*: `lthooks`, `ltcmdhooks`, `ltfilehook`, `ltshipout`, `ltpara` 
 - New modules\*: `ltmarks`, `usrguide3`
 - LaTeX-lab: `documentmetadata-support`\*, `latex-lab-footnotes`, `latex-lab-new-or`, `latex-lab-prototype`, `latex-lab-testphase`

\* with variants `-doc` and `-code`

#### Get format version

* The format version appears in the first line of every `.log` file <br />
  (note the date **`2018.5.3`** following `preloaded format=xelatex `):

   ```
   This is XeTeX, Version 3.14159265-2.6-0.99999 (TeX Live 2018)
   (preloaded format=xelatex 2018.5.3)  2 OCT 2018 11:39
   ```
* Put `\fmtversion` in your `.tex` file, and it will out put the format version info into the `.pdf` file, in a format of `yyyy/mm/dd` (e.g. `2018/04/01`).
* Since the format version is restored in command `\fmtversion`, you can get the value of it from the terminal, with the help of [`texdef`](https://ctan.org/pkg/texdef?lang=en):

    ```bash
    $ latexdef fmtversion

    \fmtversion:
    macro:->2018-04-01
    ```

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

`lualatex` can use fonts in directory `/Library/Fonts`, but not those added to new library of `Font Book.app`. Setting TeX Live variable `OSFONTDIR` by

 - adding line `OSFONTDIR = <path>` to file `TEXMFROOT/texmf.cnf`, or
 - using command `tlmgr conf texmf OSFONTDIR <path>`

may both solve the problem.

Further reading: [every variable that `texmf.cnf` can contain](https://github.com/TeX-Live/texlive-source/blob/trunk/texk/kpathsea/texmf.cnf).


## Packages

### `latex3`

CHANGELOG
 * [`l3kernal`](https://github.com/latex3/latex3/blob/master/l3kernel/CHANGELOG.md)
 * [`l3packages`](https://github.com/latex3/latex3/blob/master/l3packages/CHANGELOG.md)

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


## PDF

### Produce uncompressed PDF

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
