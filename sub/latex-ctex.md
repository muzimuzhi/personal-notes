# LaTeX Cheatsheet: `ctex` bundle

# Loading

```latex
% as document class
\documentclass[<class options>]{<ctex class>}
<ctex class> := ctexart | ctexrep | ctexbook | ctexbeamer

% as package
\usepackage[<package options>]{ctex} % without section config features
\usepackage[<package options>, heading]{ctex} % full features

% Configuration after loading
\ctexset{<config options>}
```



## Configuration

### Class/package only options

| Groups         | Keys                           | Value List/format     | Default value                                                | Notes                                                        |
| -------------- | ------------------------------ | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| encoding       | GBK\|UTF8                      | *no value*            |                                                              |                                                              |
| font mapping   | zhmap                          | `true\|false\|zhmCJK` | `true`                                                       | (pdf)latex only                                              |
| font size      | zihao                          | `-4\|5\|false`        | `5`<br />`false` if `beamer`                                 | `-4 = 12bp`<br />`5 = 10.5bp`                                |
|                | 10pt\|11pt\|12pt               |                       |                                                              |                                                              |
|                | linespread                     | `<num>`               | `1.3` if `scheme=chinese`<br />`1.0` if `scheme=plain` or `beamer` |                                                              |
| heading style  | heading                        | `true\|false`        | `false`                                                      | package option only                                          |
|                | sub3section\|<br />sub4section | *no value*            |                                                              | see sec. 5.2                                                 |
| document style | scheme                         | `chinese\|plain`     | `chinese`                                                    | affect font size, line spread, heading naming and (if `heading=true`) heading style |



### Heading Style Options (used in `\ctexset` only)

|                         | Keys              | Value List/format        | Default value      | Notes                                                        |
| ----------------------- | ----------------- | ------------------------ | ------------------ | ------------------------------------------------------------ |
|                         | `numbering`       | `true\|false`           | `true`             | working with counter `secnumdepth`                           |
| name with counter       | `name`            | `{<prefix>,<suffix>}`    | see table 5 of doc |                                                              |
|                         | `number`          | `<counter printing cmd>` | see table 6        |                                                              |
| format                  | `format(+)`       | `<code>`                 | see table 7        | affect full heading                                          |
|                         | `nameformat(+)`   | `<code>`                 | see table 8        | affect name with counter only                                |
|                         | `numberformat(+)` | `<code>`                 |                    | affect number only                                           |
|                         | `titleformat(+)`  | `<code>`                 | see table 10       | affect title content only                                    |
|                         |                   |                          |                    |                                                              |
| vertical skip           | `beforeskip`      | `<glue>`                 | see table 14       | vertical rubber space before heading                         |
|                         | `afterskip`       | `<glue>`                 |                    | vertical rubber space after heading                          |
|                         | `fixskip`         | `true\|false`           | `false`            | suppress extra space other than `before/after-skip`          |
| horizontal skip         | `indent`          | `<glue>`                 |                    |                                                              |
|                         | `hang`            | `true\|false`           | `true`             | `\section` and lower only                                    |
|                         |                   |                          |                    |                                                              |
| insertion               | `aftername(+)`    | `<code>`                 | see table 9        |                                                              |
|                         | `aftertitle(+)`   | `<code>`                 | see table 11       |                                                              |
|                         | `runin`           | `true\|false`           | see table 12       | if text followed begins a new paragraph                      |
|                         | `afterindent`     | `true\|false`            | see table 13       | if text followed has paragraph indent                        |
|                         | `break(+)`        | `<code>`                 | see table 18       | inserted between heading and following text                  |
| `toc`, `lof`, and `lot` | `tocline` | `<code>` | see table 19 | format of items in `toc`, see the doc |
|  | `lofskip`         | `<glue>`                 | `10pt`             | vertical rubber space between items of different chapters in `lof` |
|                         | `lotskip`         | `<glue>`                 | `10pt`             | ... in `lot`                                                   |
| others                  | `pagestyle`       | `<page style>`           | `plain`            | `\chapter` and `\part` only                                  |
| appendix | `appendix/numbering` | see option `numbering` |                    | affect level 1 heading after `\appendix` |
|  | `appendix/name` | see option `name` | | the same as above |
|  | `appendix/number` | see option `number` | | the same as above |

### Output or apply order
```tex
Example:
With section/name={pre,after}, section/number={\Roman{section}},
\CTEXthesection is defined to "pre\Roman{section}after"

.../break(+)
.../beforeskip      = <glue>
.../format(+)       = <overall format, maybe-one-arg>
.../nameformat(+)   = <name format, maybe-one-arg>
.../name            = {<pre name>, <after name>} | {<pre name>}
.../numbering       = true|false
.../numberformat(+) = <number format, maybe-one-arg>
.../number          = <output counter> % change commands \CTEXthe<section>
.../aftername(+)    = <code>
.../titleformat(+)  = <title format, maybe-one-arg>
.../aftertitle(+)   = <code>
.../runin           = true|false
.../afterskip       = <glue>
```
