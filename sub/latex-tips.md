# Tips About Using LaTeX

## Get format version

* The format version appears in the first line of every `.log` file <BR>(note the date **`2018.5.3`** following `preloaded format=xelatex `):

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

## [tlmgr] List installed packages

```bash
$ tlmgr list --only-installed
```
Example output lines (`i` denotes "installed"):
```
i adjustbox: Graphics package-alike macros for "general" boxes
i ae: Virtual fonts for T1 encoded CMR-fonts
i amscls: AMS document classes for LaTeX
```

Get space separated list of installed packages (use GNU `ggrep` for `-P` option):
```bash
$ tlmgr list --only-installed | ggrep -oP '(?<=i )\w+(?=:)' --color=always | tr '\n' ' '
```


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

According to [discussions on TeX.SX](https://tex.stackexchange.com/questions/229500/) and [the bug report to `pgf` project](https://sourceforge.net/p/pgf/bugs/354/), this is a `pgf` driver bug. A workaround, which recovers the definition of `\pgfsys@hboxsynced` from `dvipdfmx` version to the common driver version, is firstly suggested in [an answer on TeX.SX](https://sourceforge.net/p/pgf/bugs/354/#72e2).

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
    psdextra          = true,
    unicode           = true
}
```

ref: [answer on tex.sx](https://tex.stackexchange.com/a/69354/79060) by Heiko Oberdiek