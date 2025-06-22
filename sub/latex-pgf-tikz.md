# PGF & TikZ

* pgfrcs
  * entrance
    * latex: `pgfrcs.sty`
    * plaintex: `pgfrcs.tex`
    * context: `t-pgfrcs.tex`
  * dependent
    * ConTeXt only: `t-pgfmod.tex`, synonym for pgf's ConTeXt module names
    * `pgfutil-common.tex`
    * format specific:
      * `pgfutil-latex.def`
        *
      * `pgfutil-plain.def`
      * `pgfutil-context.def`
    * `pgfrcs.code.tex`
  *

`\pgfutil@guessdriver` is defined in `pgfutil-(plain|latex|context).def`
 *

* tikz.sty
  * pgf.sty
    * pgfrcs.sty
      * pgfutil-common.tex
        * pgfutil-common-lists.tex
      * pgfutil-latex.def
        * everyshi.sty
        * defines `\pgfutil@guessdriver`:
          driver = "tex4ht" if `\HCode` is defined else `\Gin@driver`
      * pgfrcs.code.tex
        * pgf.revision.tex
    * pgfcore.sty
      * graphicx.sty
        * keyval.sty
        * graphics.sty
          * trig.sty
          * graphics.cfg
            * guess driver, stored in `\Gin@driver`:
          * xetex.def
      * pgfsys.sty
        * pgfsys.code.tex
          * pgfkeys.code.tex
            * pgfkeysfiltered.code.tex
          * calls `\pgfutil@guessdriver`
          * pgf.cfg
          * pgfsys-xetex.def
            * pgfsys-dvipdfmx.def
              * pgfsys-common-pdf.def
        * pgfsyssoftpath.code.tex
        * pgfsysprotocol.code.tex
      * xcolor.sty
        * color.cfg
      * pgfcore.code.tex
        * pgfmath.code.tex
        * pgfint.code.tex
        * pgfcore-points.code.tex
        * pgfcore-pathconstruct.code.tex
        * pgfcore-pathusage.code.tex
        * pgfcore-scopes.code.tex
        * pgfcore-graphicstate.code.tex
        * pgfcore-transformations.code.tex
        * pgfcore-quick.code.tex
        * pgfcore-objects.code.tex
        * pgfcore-pathprocessing.code.tex
        * pgfcore-arrows.code.tex
        * pgfcore-shade.code.tex
        * pgfcore-image.code.tex
          * pgfcore-external.code.tex
        * pgfcore-layers.code.tex
        * pgfcore-transparency.code.tex
        * pgfcore-patterns.code.tex
        * pgfcore-rdf.code.tex
    * pgfmodule-shapes.code.tex
    * pgfmodule-plot.code.tex
    * pgfcomp-version-0-65.sty
    * pgfcomp-version-1-18.sty
  * pgffor.sty
    * pgfkeys.sty
      * pgfkeys.code.tex (guarded)
    * pgfmath.sty
      * pgfmath.code.tex (guarded)
    * pgffor.code.tex
      * pgfmath.code.tex (guarded)
  * tikz.code.tex
    * pgflibrary-plothandlers.code.tex
    * pgfmodule-matrix.code.tex
    * tikzlibrary-topaths.code.tex



`graphics.cfg`: guess driver
```python
driver = dvips  # dvi_mode always leads to dvips
if not defined("\\luatexversion"):
    if defined("\\pdfoutput") and pdf_mode:
        driver = pdftex
    if defined("\\OpMode"):
        driver = vtex
    if defined("\\XeTeXversion"):
        driver = xetex
else:
    if luatex.version > 85:
        if pdf_mode:
            driver = luatex
    elif pdf_mode:
        # luatex.version <= 85 leads to pdftex
        driver = pdftex
```

* `pgfsys-pdftex.def`: pdftex or luatex <= 85 in pdf mode
* `pgfsys-luatex.def`: recent luatex in pdf mode
* `pgfsys-xetex.def`: xetex
* `pgfsys-dvips.def`: default


* pgfsys
  * loader
    * `\usepackage{pgfsys}`
      * require `pgfrcs.sty`
      * package option
        * `dvisvgm`: `\def\pgfsysdriver{pgfsys-dvisvgm.def}`
      * input
        * `pgfsys.code.tex`
        * `pgfsyssoftpath.code.tex`
        * `pgfsysprotocol.code.tex`
    * `\input pgfsys.tex`
      * input
        * `pgfrcs.tex`
        * `pgfsys.code.tex`
        * `pgfsyssoftpath.code.tex`
        * `pgfsysprotocol.code.tex`
    * `\usemodule[pgfsys]`

* `pgfsys.code.tex`
  * Discern the driver
    * if `\pgfsysdriver` is undefined, `\pgfutil@guessdriver`
