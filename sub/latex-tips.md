# Tips About Using LaTeX

## Get format version

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


## [tlmgr] Show info about schemes, collections, and packages

* Show full list
  ```bash
  # list of schemes
  $ tlmgr info schemes

  # list of collections
  $ tlmgr info collections

  # list of all packages
  $ tlmgr info 
  ```
  With option `--only-installed`, only installed items are shown.
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
* Get space separated list of installed packages (use GNU `ggrep` for its `-P` option):
  ```bash
  $ tlmgr list --only-installed | ggrep -oP '(?<=i )\w+(?=:)' | tr '\n' ' '
  ```

## Font encodings

* Main doc: `texdoc fontenc`
* EU1 encoding (`eu1enc.def`): provided by package `euenc`
* T3 encoding (`t3enc.def`): provided by package `tipa`

## [xetex] Show full path of fonts

Add `\XeTeXtracingfonts=1` before `\documentclass`, and find full path of fonts in `.log`, like
```
Requested font "[lmroman10-bold]:mapping=tex-text;" at 10.0pt
 -> /usr/local/texlive/2018basic/texmf-dist/fonts/opentype/public/lm/lmroman10-
bold.otf
```

## Using extra fonts with `lualatex`

`lualatex` can use fonts in directory `/Library/Fonts`, but not those added to new library of `Font Book.app`. Setting TeX Live variable `OSFONTDIR` by

 - adding line `OSFONTDIR = <path>` to file `TEXMFROOT/texmf.cnf`, or
 - using command `tlmgr conf texmf OSFONTDIR <path>`

may both solve the problem.

Further reading: [every variable that `texmf.cnf` can contain](https://github.com/TeX-Live/texlive-source/blob/trunk/texk/kpathsea/texmf.cnf).

## Workaround: use `tikzmark` package with `xelatex`

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

## Workaround: use `tikz-feynman` package with `lualatex`

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

## [hyperref] Allow `unicode-math` math symbols in bookmark

```latex
\hypersetup{
    psdextra = true,
    unicode  = true
}
```

## [fontspec] Use with math font packages

Pass `no-math` option to `fontspec`, e.g.,
```latex
\usepackage[no-math]{fontspec}
\usepackage{stix2}
```

ref: [answer on TeX-SX](https://tex.stackexchange.com/a/69354) by Heiko Oberdiek

## [beamer] Doc of `pgfpicture` environment

Manual of `pgf` only documents `pgfpicture` environment without any arguments, while source code of `beamer` uses an old version of that environment, 

The usage of `pgfpicture` environment in `beamer`, with four braced arguments, is defined in `pgfcorescopes.code.tex` and not documented in pgf manual yet.

## [minted] Highlight `@` as command name letter

Modify python class `TeXLexer` in file [pygments/lexers/markup.py](https://github.com/dagwieers/pygments/blob/master/pygments/lexers/markup.py).

```python
class TexLexer(RegexLexer):
    ... ...

    tokens = {
        ...
        'root': [
            (r'\\\[', String.Backtick, 'displaymath'),
            (r'\\\(', String, 'inlinemath'),
            (r'\$\$', String.Backtick, 'displaymath'),
            (r'\$', String, 'inlinemath'),
            # (r'\\([a-zA-Z]+|.)', Keyword, 'command'), # before
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
        ...
    }
    
    ... ...
```

## [listings] Reset output style for `-` (U+002D)

By default, `listings` prints character `-` as in text mode if the current font family is `\ttfamily` and as in math mode (`$-$`) otherwise. One can [overwrite this scheme](https://tex.stackexchange.com/a/424193) by
```latex
\makeatletter
\lst@CCPutMacro
    \lst@ProcessOther{"2D}{-{}}
    \@empty\z@\@empty
\makeatother
```

## [listings] `literate`d input absorbs following space in `(full)flexible` mode

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

## [equation] Disregard indent of displayed equations inside list env

```latex
\everydisplay\expandafter{%
  \the\everydisplay
  \displayindent=0pt%
  \displaywidth=\hsize
}
```

