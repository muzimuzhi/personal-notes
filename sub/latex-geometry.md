# LaTeX Cheatsheet: `geometry`

## Commands

```latex
\usepacakge[<user options>]{geometry}
\geometry{<user options>}

% clear <user options> but paper size
\newgeometry{<options>}
% restore the page layout in preamble
\restoregeometry

\savegeometry{<name>}
\loadgeometry{<name>}
```

## Options (selected and sufficient)

| Level  | Key-Value                  |                      |
| ------ | -------------------------- | -------------------- |
| paper  | `paper `                   | `<paper name>`       |
|        | `paperwidth/paperheight `  | `<dimen>`            |
|        | `papersize`                | `{<dimen>, <dimen>}` |
| layout | `layout`                   | `<paper name>`       |
|        | `layoutwidth/layoutheight` | `<dimen>`            |
|        | `layoutsize`               | `{<dimen>, <dimen>}` |
| body   | `inner/outer/top/bottom`   | `<dimen>`            |
|        | `top/bottom`               | `<dimen>`            |
|        | `headheight`               | `<dimen>`            |
|        | `headsep`                  | `<dimen>`            |
|        | `footskip`                 | `<dimen>`            |
|        | `marginparsep`             | `<dimen>`            |
|        | `marginparwidth`           | `<dimen>`            |
| other  | `reversemarginpar`         |                      |
|        | `pass`                     |                      |
|        | `reset`                    |                      |
|        | `showframe`                |                      |
|        | `showcrop`                 |                      |

```
<paper sizes> := aXpaper | bXpaper | cXpaper | <others>
  where 0 <= X <= 6
```

## Typical Application

```latex
% pass default options, show frame only
\usepackage[pass, showframe]{geometry}
```
