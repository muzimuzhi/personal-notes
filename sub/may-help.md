# Something May Help in the Future

Core
  * Full [list of primitives](https://zhuanlan.zhihu.com/p/59151033) by engine
  * [List of tracing commands](https://tex.stackexchange.com/a/60494)

  * [List of expandable primitives](https://tex.stackexchange.com/a/467372)
    * For expandable TeX primitives, see also *TeX by Topic*, Sec. 12.2.
  * [List of tracing commands](https://tex.stackexchange.com/a/60494)

Font
  * OpenType Math Fonts
    * BachoTEX 2009, Ulrik Vieth. [OpenType Math Illuminated](https://www.tug.org/~vieth/papers/bachotex2009/ot-math-paper.pdf)
    * BachoTEX 2019, Ulrik Vieth. [OpenType Math Fonts: What’s new or noteworthy?](http://www.gust.org.pl/bachotex/2019-pl/presentations/uvieth-1-2019.pdf)
    * 2018, Xiangdong Zeng. [Bibliography](https://github.com/firamath/firamath.github.io/blob/master/bibliography.md) of font firamath

Equation
  * [Multiline Root Symbol (`\sqrt`)](https://tex.stackexchange.com/a/111433)

PGF/TikZ
  * [Reflecting a line and/or point with named coordinates](https://tex.stackexchange.com/q/467295)

Meta
  * [Minimal working example](https://tex.meta.stackexchange.com/q/228) (MWE), what and how
    * [Prefer `article` to `minimal` LaTeX class](https://tex.stackexchange.com/a/42115) in MWE
  * [How to upgrade TeX distribution?](https://tex.stackexchange.com/q/55437)

## TeX Live

* [SVN repo](http://www.tug.org/svn/texlive/trunk/Master/texmf-dist/), [GitHub mirror](https://github.com/TeX-Live/texlive-source)
* [Historic images](ftp://tug.org/historic/systems/texlive/)

## XeTeX

* Precedence of font names used by XeTeX ([ref](https://tex.stackexchange.com/a/43819))
  * `Full name > Family-Style > PostScript name > Family`
  * To let XeTeX auto-load variations, `Family` is recommended
* Show font info `aat-info.tex` and `opentype-info.tex`, located in `$TEXMFDIST/tex/xetex/xetexfontinfo`

## dvipdfm-x

* [SVN repo](http://www.tug.org/svn/texlive/trunk/Build/source/texk/dvipdfm-x/) (as part of TeX Live) and [GitHub repo](https://github.com/shirat74/dvipdfm-x/) of Shunsaku Hirata: 

## MiKTeX - A LaTeX Distribution

* MiKTeX now [supports Win/Mac/Linux](https://miktex.org/download), and provides Docker image.
* Unique TeX features, see [doc](https://docs.miktex.org/2.9/manual/texfeatures.html) for full description
  * automatic package installation
  * list of package usage
  * additional input directories
  * specific directory for auxiliary files

## ConTeXt - A LaTeX Format

* Recommended ConTeXt Docs, [Q&A on TeX-SX](https://tex.stackexchange.com/questions/2839/where-can-i-find-good-context-documentation)
* Auto-generated [list](http://www.pragma-ade.com/general/qrcs/setup-en.pdf) of all commands, without descriptions

## Projects implementing TeX by Different Programming Languages

### JavaTeX Project, by Timothy Murphy

An introduction to this Project is published on [1999 TUGboat](https://www.tug.org/TUG99-web/pdf/murphy.pdf). This project is also uploaded to CTAN as a package [named `javatex`](https://ctan.org/pkg/javatex?lang=en). The performance is so poor that the author "considers it unusable in practice", by its readme on CTAN.

## Journal

[The PracTeX Journal](http://tug.org/pracjourn/2012-1/toc.html), the online journal of the TeX Users Group, has published [20 issues](http://tug.org/pracjourn/archive.html) from 2005 to 2012.
