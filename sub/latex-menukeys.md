# LaTeX Cheatsheet: `menukeys`

## Syntax

### Basic use
```abnf
basic-usage       ::= basic-macro [ "[" input-separator "]" ] menu-sequence

basic-macro       ::= "\\menu" | "\\directory" | "\\keys"
input-separator   ::= "/" | "=" | "+" | "," | ";" | ":" | "-" | ">" | "<" | backslash-symbol
backslash-symbol  ::= "bslash" | "backslash" | "directory" | "location"
menu-sequence     ::= <text separated by input-separator>
```
_Note_
1. If `input-separator` is omitted, `,` is used as default.

Example
```latex
\menu          {a,b,c}
\menu [+]      {a+b+c}
\menu [bslash] {a\b\c}
```

### Define or change menu macros
```abnf
macro-declare     ::= new-macro ["[" input-separator "]"] predefined-style

new-macro         ::= "\\newmenumacro" | "\\renewmenumacro" | "\\providemenumacro"
predefined-style  ::= ["rounded" | "angular"] "menus"
                    | (["shadowed"] ("rounded" | "angular") | "typewriter")  "keys"
                    | ["hyphenate"] "paths" ["with" ["black"] "folder"]
```
_Note_
1. If `input-separator` is omitted, `,` is used as default.
1. full list of `predefined-style`:
    | menus   | keys | paths |  |
    | ------: | ---: | ----- | ----- |
    | `menus` | `roundedkeys` | `paths` | `hyphenatepaths` |
    | `roundedmenus` | `shadowedroundedkeys` | `pathswithfolder` | `hyphenatepathswithfolder` |
    | `anguularmenus` | `angularkeys` | `pathswithblackfolder` | `hyphenatepathswithblackfolder` |
    |  | `shadowedangularkeys` | | |
    |  | `typewriterkeys` | | |

`basic-macro`s are defined by
```latex
\newmenumacro{\menu}      [>] {menus}
\newmenumacro{\directory} [/] {paths}
\newmenumacro{\keys}      [+] {roundedkeys}
```

### Define or change menu style
```abnf
style-declare     ::= new-style-simple <>
new-style
```
