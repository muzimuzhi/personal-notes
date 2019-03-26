# LaTeX Cheatsheet: `geometry`

## Commands

```latex
\usepacakge[<user options>]{geometry}
\geometry{<user options>}

% clear <user options> but paper size
\newgeometry{<options>}
% restore the page layout set in preamble
\restoregeometry

\savegeometry{<layout name>}
\loadgeometry{<layout name>}
```

## Options (selected and sufficient)

```latex
\geometry{
%% 1. set paper size, using 1 of the 2
%  paper          = <paper name>, % "[abc][0-6]paper" and others
  papersize       = {<paper width>, <paper height>},
%% 2. set layout size, normally the same as paper
%  layout         = <paper name>,
%  layoutsize     = {<layout width>, <layout height>},
%% 3. set body size, by default body includes text body only
%  ignoreall,       % set by default, ignore head, foot, and margin par
%% 4. set margin size
  hmargin         = {<left/inner>, <right/outer>},
  vmargin         = {<top>, <bottom>},
  headheight      = <dimen>,
  headsep         = <dimen>,
  footskip        = <dimen>,
  marginparsep    = <dimen>,
  marginparwidth  = <dimen>,
  columnsep       = <dimen>,
%  reversemarginpar,
%  twoside,
%% 5. others
  pass,
  reset,
  showframe,
  showcrop
}
```

## Typical Application

```latex
% pass default options, show frame only
\usepackage[pass, showframe]{geometry}
```
