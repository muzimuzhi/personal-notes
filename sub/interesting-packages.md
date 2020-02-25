# List of Interesting and Interested Packages Not Familiar with

## Macro Packages

| Name              | Category            | Description                                                  |
| ----------------- | ------------------- | ------------------------------------------------------------ |
| `wiki`            | text markup         | Use wiki-style markup                                        |
| `relsize`         | font                | Changes font size relative to current size.                  |
| `xcharter`        | font                | Extension of Bitstream Charter fonts                         |
| `savesym`         | font/math           | Redefines and restores name of symbol command                |
| `stickstoo`       | font/math           | Extra number styles and blackboard bold choices, based on STIX2 |
|                   |                     |                                                              |
| `mismath`         | math                | Miscellaneous mathematical macros                            |
| `nicematrix`      | math                | Improved typesetting of mathematical matrices with TikZ      |
| `systeme`         | math                | Provides a more intuitive way to enter systems of equations or inequalities. <br />Doc is in French only. |
| `autoaligne`      | math                | Aligns lines of math expressions by operators and relations. <br />Doc is in French only. |
| `mleftright`      | math                | Variants of delimiters that act as maths open/close          |
| `mathpunctspace`  | math                | Controls the space after punctuation in math expressions     |
| `mismath`         | math                | (New) Miscellaneous mathematical macros                      |
| `letterswitharrows` | math & pgf        | Arrows over math letters                                     |
|                   |                     |                                                              |
| `flowframe`       | layout              | Create text frames that flow from one to another             |
| `paracol`         | layout              | Multiple columns with texts "in parallel"                    |
| `layouts`         | layout show         | Shows layout of document elements.                           |
| `extramarks`      | marks               | More marks other than `\markleft` and `\markboth`            |
| `tocdata`         | table of contents   | Add a small amount of data to an entry in `toc`              |
| `centeredline`    | paragraph alignment | Extends latex2e macro`\centerline`                           |
|                   |                     |                                                              |
| `keyvaltable`     | tabular             | Re-usable table layouts separating content and presentation  |
| `fcolumn`         | tabular             | Adds column type `F/f` for aligning text and currency amounts, and`\sumline` command. |
| `boldline`        | tabular             | Variable-width h/v-lines in tabular. Contained in `shipunov` bundle |
| `hhline`          | tabular             | Better horizontal lines in tabulars and arrays               |
| `ehhline`         | tabular             | Extend the `\hhline` command                                 |
| `hlist`           | tabular/list        | Provides  `hlist` environment in which `\hitem` starts a horizontal and columned item. <br />Doc is in French only. |
|                   |                     |                                                              |
| `graphbox`        | graphics            | Adds options to `\includegraphicx`                           |
| `pgfgantt`        | graphics/pgf        | Draws Gantt charts                                           |
| `floatrow`        | float               | Float layouts                                                |
|                   |                     |                                                              |
| `lstaddons`       | code listing        | add-on for `listings`: autogobble and line background        |
|                   |                     |                                                              |
| `changes`         | editorial           | Markup change in text manually                               |
|                   |                     |                                                              |
| `sesamanuel`      | class or template   | Class and package for sesamath books or paper                |
|                   |                     |                                                              |
| `unravel`         | programming         | Watches TeX digest tokens                                    |
| `lisp-on-tex`     | programming         | Implements static scoping, dynamic typing, and eager evaluation LISP interpreter written in TeX macros |
| `texapi`          | programming         | Format-independent `etoolbox`                                |
| `ltxcmds`         | programming         | Utility macros from LaTeX kernel                             |
| `silence`         | compilation/log     | Filters errors and warnings produced by standard macros.     |
| `snapshot`        | compilation         | List the external dependencies                               |
| `hypdoc`          | PDF feature         | Adds more links of references.                               |
| `navigator`       | PDF feature         | format-independent `hyperref`                                |
| `lwarp`           | convert format      | Convert LaTeX output (PDF) to HTML.                          |

## Documentation Packages

| Name                  | Category               |
| `around-the-bend`     | answered tex exercises | 
| `tamethebeast`        | bibtex                 |
| `biblatex-cheatsheet` | biblatex, cheatsheet   |

## Supporting Tools

* [latexindent](https://ctan.org/pkg/latexindent) - a `Perl` script that indents `.tex` (and other) files according to an indentation scheme that the user can modify to suit their taste.
* [TLCockpit](https://ctan.org/pkg/tlcockpit) - A GUI frontend to TeX Live Manager (`tlmgr`), requiring JavaFX
* [ClutTeX](https://ctan.org/pkg/cluttex) - a program to automatically process LaTeX document without clutter working directory with `.aux`, `.log`, etc. files
