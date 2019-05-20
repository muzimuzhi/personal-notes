# List of Interesting and Interested Packages Not Familiar with

## Macro Packages

| Name           | Category            | Description                                                  |
| -------------- | ------------------- | ------------------------------------------------------------ |
| `relsize` | font | Changes font size relative to current size. |
| `savesym` | font/math | Redefines and restores name of symbol command|
|  |  |  |
| `mismath`      | math                | Miscellaneous mathematical macros                            |
| `systeme`      | math                | Provides a more intuitive way to enter systems of equations or inequalities. <br />Doc is in French only. |
| `autoaligne`   | math                | Aligns lines of math expressions by operators and relations.<br />Doc is in French only. |
| `mleftright` | math | Variants of delimiters that act as maths open/close |
| `mathpunctspace` | math | Controls the space after punctuation in math expressions |
|                |                     |                                                              |
| `layouts`      | layout show         | Shows layout of document elements.                           |
| `extramarks`   | marks               | More marks other than `\markleft` and `\markboth`            |
| `centeredline` | paragraph alignment | Extends latex2e macro`\centerline`                           |
|                |                     |                                                              |
| `fcolumn`        | tabular             | Adds column type `F/f` for aligning text and cur­rency amounts, and`\sumline` command. |
| `boldline`       | tabular             | Variable-width h/v-lines in tabular. Contained in `shipunov` bundle |
| `hlist`          | tabular/list        | Provides  `hlist` environment in which `\hitem` starts a horizontal and columned item. <br />Doc is in French only. |
|  |  |  |
| `graphbox` | graphics | Adds options to `\includegraphicx` |
| `pgfgantt` | graphics/pgf | Draws Gantt charts |
|                |                     |                                                              |
| `lisp-on-tex`  | programming         | Implements static scoping, dynamic typing, and eager evaluation LISP interpreter written in TeX macros |
| `texapi` | programming | Format-independent `etoolbox` |
| `silence`      | compilation/log     | Filters errors and warnings produced by standard macros.     |
| `hypdoc`       | PDF feature         | Adds more links of references.                               |
| `navigator` | PDF feature | format-independent `hyperref` |
| `lwarp`        | convert format      | Convert LaTeX output (PDF) to HTML.                         |


## Supporting Tools

* [latexindent](https://ctan.org/pkg/latexindent) - a `Perl` script that indents `.tex` (and other) files according to an indentation scheme that the user can modify to suit their taste.
* [TLCockpit](https://ctan.org/pkg/tlcockpit) - A GUI frontend to TeX Live Manager (`tlmgr`), requiring JavaFX
* [ClutTeX](https://ctan.org/pkg/cluttex) - a program to automatically process LaTeX document without clutter working directory with `.aux`, `.log`, etc. files
