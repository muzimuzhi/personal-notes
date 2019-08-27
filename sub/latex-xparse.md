# LaTeX Cheatsheet: `xparse`

## Syntax

```abnf
xparse-declare ::= cmd-declare <cmd name> arg-spec <definition code>
                 | env-declare <env name> arg-spec <begin code> <end code>

cmd-declare    ::= "\\" prefix ["Expandable"] "DocumentCommand"
env-declare    ::= "\\" prefix "DocumentEnvironment"
prefix         ::= "New" | "Renew" | "Provide" | "Declare"

arg-spec       ::= argument*
argument       ::= processor* [modifier] arg-type
```
_Note_: notation introduced by [Python Language Reference](https://docs.python.org/3/reference/introduction.html#notation) is used.

Example
```latex
\ExplSyntaxOn
\NewDocumentCommand \myCmd { s O{default} m }
  {
    \IfBooleanTF {#1} {star} {no-star},
    #1, #2
  }

\NewDocumentEnvironment {myEnv} { +m }
  {begin code} {end code}
\ExpleSyntaxOff

\myCmd{arg2}        % -> "no-star,default,arg2"
\myCmd*{arg2}       % -> "star,default,arg2"
\myCmd[arg1]{arg2}  % -> "no-star,arg1,arg2"

\begin{myEnv}{...}
\end{myEnv}
```

## `<prefix>`: Name Prefix

| Prefix      | If `<cmd-name>/<env-name>` defined | If not defined |
| ----------- | ---------------------- | -------------- |
| `"New"`     | ERROR                  | run            |
| `"Renew"`   | run                    | ERROR          |
| `"Provide"` | ignored                | run            |
| `"Declare"` | run                    | run            |

## `<arg-type>`: Argument Type

### Denoting Mandatory Argument

| Type                       | Meaning                              |
| -------------------------- | ------------------------------------ |
| `m`                        | standard                             |
| `r<char1><char2>`          | delimited by `<char1>` and `<char2>` |
| `R<char1><char2>{default}` | variant of `r`                       |
| `v`                        | delimited like `\verb`               |
| `b`                        | denote the body of environment       |

### Denoting Optional Argument

| Type                       | Meaning                              | Tester           |
| -------------------------- | ------------------------------------ | ---------------- |
| `o`                        | standard                             | `\If(No)ValueTF` |
| `O{default}`               | variant of `o`                       |                  |
| `d<char1><char2>`          | delimited by `<char1>` and `<char2>` | `\If(No)ValueTF` |
| `D<char1><char2>{default}` | variant of `d`                       |                  |
| `s`                        | optional star `*` argument           | `\IfBooleanTF`   |
| `t<char>`                  | optional `<char>` argument           | `\IfBooleanTF`   |

## `<modifier>`: Argument Modifier

| Modifiers | Meaning                                            | Examples           |
| --------- | -------------------------------------------------- | ------------------ |
| `+`       | mark next argument as long                         | `+m`               |
| `!`       | ignore space-prefixed optional argument            | `!o`               |

## `<processor>`: Argument Processor

```abnf
processor ::= ">{" processor-function "}"
```

| Processor function                     | Meaning                                                      |
| -------------------------------------- | ------------------------------------------------------------ |
| `\ReverseBoolean`                      | reverse logic of specifiers `s` or `t`                       |
| `\SplitArgument{<number>}{<token(s)>}` | split argument into `<number>` + 1 parts by `<tokens(s)>`    |
| `\SplitList{<token(s)>}`               | split argument into variable parts by `<tokens(s)>`          |
| `\TrimSpaces`                          |                                                              |
| user defined `\procFunc<n args>`       | - `\procFunc` accepts `n` + 1 argument(s)<br />- return processed argument as variable `\ProcessedArgument` |

Examples
```latex
% \SplitArgument: split argument into fixed parts
> { \SplitArgument { 2 } { ; } } m

{a;b}     => {a}{b}{-NoValue-}
{a;b;c}   => {a}{b}{c}
{a;b;c;d} => ERROR (too many `;`s are present)

% \SplitList: split argument into variable parts
> { \SplitList { ; } } m

{a;b}     => {a}{b}
{a;b;c}   => {a}{b}{c}
{a;b;c;d} => {a}{b}{c}{d}

% \ProcessList: list-processing helper function
> { \foo } m

\NewDocumentCommand \foo { > { \SplitList { ; } } m }
  { \ProcessList {#1} { \map } }
\NewDocumentCommand \map { m }
  { |#1| }

{a;b}     => {|a|}{|b|}
{a;b;c}   => {|a|}{|b|}{|c|}
{a;b;c;d} => {|a|}{|b|}{|c|}{|d|}
```

## Not Included

Argument types
 - denoting optional argument: `e/E`
 - backwards compatibility: mandatory `l` and `u`, optional `g/G`

See documentation of `xparse` package for full introduction.
