# List of Interested LaTeX Packages

## Macro Packages

| Name              | Category            | Description                                                  |
| ----------------- | ------------------- | ------------------------------------------------------------ |
| `wiki`            | text markup         | Use wiki-style markup                                        |
| `relsize`         | font                | Changes font size relative to current size.                  |
| `savesym`         | font/math           | Redefines and restores name of symbol command                |
|                   |                     |                                                              |
| `nath`            | math                |                                                              |
| `mismath`         | math                | Miscellaneous mathematical macros                            |
| `nicematrix`      | math                | Improved typesetting of mathematical matrices with TikZ      |
| `systeme`         | math                | Provides a more intuitive way to enter systems of equations or inequalities. <br />Doc is in French only. |
| `autoaligne`      | math                | Aligns lines of math expressions by operators and relations. <br />Doc is in French only. |
| `mleftright`      | math                | Variants of delimiters that act as maths open/close          |
| `mathpunctspace`  | math                | Controls the space after punctuation in math expressions     |
| `letterswitharrows` | math & pgf        | Arrows over math letters                                     |
|                   |                     |                                                              |
| `flowframe`       | layout              | Create text frames that flow from one to another             |
| `paracol`         | layout              | Multiple columns with texts "in parallel"                    |
| `layouts`         | layout show         | Shows layout of document elements.                           |
| `floatrow`        | layout/float        | Float layouts                                                |
| `marginfix`       | layout/marginal     | A `\marginpar` patch to avoid overfull margins               |
| `extramarks`      | marks               | More marks other than `\markleft` and `\markboth`            |
| `updatemarks`     | marks               | Extract and update marks from boxes |
| `tocdata`         | table of contents   | Add a small amount of data to an entry in `toc`              |
| `fncychap`        | section style       | Seven predefined chapter heading styles                      |
| `centeredline`    | paragraph alignment | Extends latex2e macro`\centerline`                           |
|                   |                     |                                                              |
| `keyvaltable`     | tabular             | Re-usable table layouts separating content and presentation  |
| `fcolumn`         | tabular             | Adds column type `F/f` for aligning text and currency amounts, and`\sumline` command. |
| `boldline`        | tabular             | Variable-width h/v-lines in tabular. Contained in `shipunov` bundle |
| `hhline`          | tabular             | Better horizontal lines in tabulars and arrays               |
| `ehhline`         | tabular             | Extend the `\hhline` command                                 |
| `hlist`           | tabular/list        | Provides  `hlist` environment in which `\hitem` starts a horizontal and columned item. <br />Doc is in French only. |
|                   |                     |                                                              |
| `csvsimple`       | data processing     | Lightweight CSV processing including filtering and table generation |
|                   |                     |                                                              |
| `graphbox`        | graphics            | Adds options to `\includegraphicx`                           |
| `smartdiagram`    | graphics/pgf        | Generates (SmartArt-like) diagrams from lists                |
| `pgfgantt`        | graphics/pgf        | Draws Gantt charts                                           |
| `tikzlings`       | graphics/pgf        | Predefined small animals                                     |
|                   |                     |                                                              |
| `beamertheme-tcolorbox` | beamer        | Beamer inner theme reproducing standard beamer blocks in `tcolorbox` |
| `lstaddons`       | code listing        | `listings` add-ons: autogobble and line background           |
|                   |                     |                                                              |
| `changes`         | editorial           | Markup change in text manually                               |
|                   |                     |                                                              |
| `sesamanuel`      | class or template   | Class and package for sesamath books or paper                |
|                   |                     |                                                              |
| `expkv-def`       | key-value           | A key-defining frontend for `expkv`                          |
| `unravel`         | programming         | Watches TeX digest tokens                                    |
| `lisp-on-tex`     | programming         | Implements static scoping, dynamic typing, and eager evaluation LISP interpreter written in TeX macros |
| `texapi`          | programming         | Format-independent `etoolbox`                                |
| `lt3luabridge`    | programming         | Execute Lua code in any TeX engine that exposes the shell |
| `ltxcmds`         | programming         | Utility macros from LaTeX kernel                             |
| `letltxmacro`     | programming         | `\LetLtxMacro` takes care of robust or opt-arg macros        |
| `parselines`      | programming         | A simple input line parser                                   |
| `silence`         | compilation/log     | Filters errors and warnings produced by standard macros.     |
| `snapshot`        | compilation         | List the external dependencies                               |
| `hypdoc`          | PDF feature         | Adds more links of references.                               |
| `navigator`       | PDF feature         | format-independent `hyperref`                                |
| `lwarp`           | convert format      | Convert LaTeX output (PDF) to HTML.                          |

- math
  - `minim-math` - OpenType math for Lua(La)TeX

## Font Packages

| Name                  | Category   | Description                                  |
| --------------------- | ---------- | -------------------------------------------- |
| `xcharter`            | font & pkg | Extension of Bitstream Charter fonts         |
| `oldstandard`         | font & pkg | Serif, late 19th to early 20th century style |
| `stickstoo`           | font & pkg | Extra number styles and blackboard bold choices to STIX2 |
| `logix`               | font & pkg | Supplemental Unicode math symbols, esp. used in logic |
| `noto-emoji`          | font       | Google's emoji, Android and Linux only       |
| `twemoji-colr`        | font       | Twitter's emoji                              |

## Documentation Packages

| Name                  | Description            |
| --------------------- | ---------------------- |
| `around-the-bend`     | answered TeX exercises |
| `tex-nutshell`        | summary of TeX features and plain TeX macros |
| `tamethebeast`        | bibtex                 |
| `biblatex-cheatsheet` | biblatex, cheatsheet   |

## Supporting Tools

* [latexindent](https://ctan.org/pkg/latexindent) - a `Perl` script that indents `.tex` (and other) files according to an indentation scheme that the user can modify to suit their taste.
* [TLCockpit](https://ctan.org/pkg/tlcockpit) - A GUI frontend to TeX Live Manager (`tlmgr`), requiring JavaFX
* [ClutTeX](https://ctan.org/pkg/cluttex) - a program to automatically process LaTeX document without clutter working directory with `.aux`, `.log`, etc. files
* `mkjobtexmf` - a `Perl` script that generates a texmf tree for a particular job
* `dviasm` - DVI editing utility
    ```bash
    # convert dvi to human readable contents
    dviasm file.[dvi|xdv] > file.dviasm
    # create dvi
    dviasm file.dviasm -o file.[dvi|xdv]
    ```
- Log filters
  - `texlogsieve`
  - `texlogfilter`
  - `texloganalyser`
