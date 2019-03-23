# LaTeX Cheatsheet: `xparse`

## Syntax

```latex
\NewDocumentCommand <function> {<arg specs>} {<code>}

\NewDocumentEnvironment {<environment>} {<arg specs>}
  {<start code>} {<end code>}

<arg specs> := <arg spec> <arg specs> | ε
<arg spec>  := <processors> <modifiers> <spec>
```

## Variants of Commands

| Series    | Commands                  | Environments                  | Expandable Commands                 |
| --------- | ------------------------- | ----------------------------- | ----------------------------------- |
| `New`     | `\NewDocumentCommand`     | `\NewDocumentEnvironment`     | `\NewExpandableDocumentCommand`     |
| `Renew`   | `\RenewDocumentCommand`   | `\RenewDocumentEnvironment`   | `\RenewExpandableDocumentCommand`   |
| `Provide` | `\ProvideDocumentCommand` | `\ProvideDocumentEnvironment` | `\ProvideExpandableDocumentCommand` |
| `Declare` | `\DeclareDocumentCommand` | `\DeclareDocumentEnvironment` | `\DeclareExpandableDocumentCommand` |

## Differences of different naming prefix

| Prefix    | If `<function>`defined | If not defined |
| --------- | ---------------------- | -------------- |
| `New`     | ERROR                  | ✓              |
| `Renew`   | ✓                      | ERROR          |
| `Provide` | ignore `<code>`        | ✓              |
| `Declare` | ✓                      | ✓              |

## Argument Specification

### Mandatory Arguments

| Types             | Meaning                                                      | Variant with `{<default>}` |
| ----------------- | ------------------------------------------------------------ | -------------------------- |
| `m`               | standard mandatory argument                                  |                            |
| `r<char1><char2>` | "required" delimited argument, using `<char1>` and `<char2>` as delimiters | `R`                        |
|                   |                                                              |                            |
| `v`               | "verbatim" argument, similar to `\verb`                      |                            |
| `b`               | body of environment argument                                 |                            |

### Optional Arguments

| Types             | Meaning                     | Variant with `{<default>}` | Tester                               |
| ----------------- | --------------------------- | -------------------------- | ------------------------------------ |
| `o`               | standard optional argument  | `O`                        | `\IfValue(TF)`<br />`\IfNoValue(TF)` |
| `d<char1><char2>` | delimited optional argument | `D`                        | `\IfValue(TF)`<br />`\IfNoValue(TF)` |
|                   |                             |                            |                                      |
| `s`               | optional star argument      |                            | `\IfBoolean(TF)`                     |
| `t<char>`         | optional `<char>` argument  |                            | `\IfBoolean(TF)`                     |

## Modifiers of Argument Specifiers

| Modifiers | Meaning                                            | Examples           |
| --------- | -------------------------------------------------- | ------------------ |
| `+`       | mark next argument as long                         | `+m`               |
| `>`       | declare argument processors, composited from right | `>{<processor>} m` |
| `!`       | ignore space-prefixed optional argument            | `!o`               |

## Argument Processors

| Processors                             | Meaning                                                      |
| -------------------------------------- | ------------------------------------------------------------ |
| `\ReverseBoolean`                      | reverse logic of specifiers `s` and `t`                      |
| `\SplitArgument{<number>}{<token(s)>}` | split argument into `<number>` + 1 parts by `<tokens(s)>`    |
| `\SplitList{<token(s)>}`               | split argument into variable parts by `<tokens(s)>`          |
| `\TrimSpaces`                          |                                                              |
| define new                             | - accept one argument<br />- return processed argument as variable `\ProcessedArgument` |

Examples
```latex
% \SplitArgument: split argument into 3 parts by `;`
> { \SplitArgument { 2 } { ; } } m

{a;b}     => {a}{b}{-NoValue-}
{a;b;c}   => {a}{b}{c}
{a;b;c;d} => ERROR (too many `;`s are present)

% \SplitList: split argument by `;`
> { \SplitList { ; } } m

{a;b}     => {a}{b}
{a;b;c}   => {a}{b}{c}
{a;b;c;d} => {a}{b}{c}{d}

% \ProcessList: helper function
\NewDocumentCommand \foo
  { > { \SplitList { ; } } m }
  { \ProcessList {#1} { \MappingFunction } }
```

## Not Included

Argument specifiers: `e/E`, `l`, `u`, `g/G`