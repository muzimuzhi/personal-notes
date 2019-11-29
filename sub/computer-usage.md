# Computer Usage

## Tiny Useful Softwares

* Show live battery information of Mac and iOS device<br />
  [coconut Battery](https://www.coconut-flavour.com/coconutbattery/)<br />
  `brew cask info coconutbattery`
* Control volume slider each app<br />
  [Background Music](https://github.com/kyleneideck/BackgroundMusic)<br />
  `brew cask info background-music`

## Libraries

* [pyl](https://github.com/dabeaz/ply) - Python Lex-Yacc
* [pygments](https://bitbucket.org/birkenfeld/pygments-main/src/default/) - Pythonic syntax highlighter for 300+ languages and text formats
  * used by latex package `minted`
  * noted issue: 
    * [#1529 unicode not working for python](https://bitbucket.org/birkenfeld/pygments-main/issues/1529/unicode-not-working-for-python)
* [linguist](https://github.com/github/linguist) - blob languages detection used by GitHub
  * [known languages](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml) to GitHub
  * [grammars used by GitHub](https://github.com/github/linguist/blob/master/vendor/README.md) to do syntax highlighting
  * [test page](https://github-lightshow.herokuapp.com/?utf8=✓&scope=from-url&grammar_format=auto&grammar_url=https%3A%2F%2Fraw.githubusercontent.com%2FAlhadis%2Flanguage-grammars%2Fmaster%2Fgrammars%2Fabnf.cson&grammar_text=&code_source=from-url&code_url=https%3A%2F%2Fraw.githubusercontent.com%2FTadiT7%2Fxiaomi_violet_dump%2F5edc11ebb2c4b5d9a4bfd5ebc335ee7e47f69f56%2Fsystem%2Fsystem%2Fusr%2Fsrec%2Fen-US%2Fcontacts.abnf&code=)


## Utility Websites

* Archive Extractor<br />
  [extract.me](https://extract.me/)

## Atom (editor)

[Script Package] Change font size of output window ([ref](https://github.com/rgbkrk/atom-script/issues/1191))

 - Open config file `~/.atom/packages/script/styles/script.less` and add `font-size` property.

    ```less
    .script-view {
      .panel-body pre {
        background: @tool-panel-background-color;
        color: @text-color;
      }

      .output {
      }

      .stderr {
        color: @text-color-error;
      }

      .line {
        border-radius: 0px;
        margin: 0px;
        padding: 0px;
        font-size: 14px;  // added here
      }
    }
    ```

## DrRacket (IDE)

Shortcuts
 - Add line comment: `Esc-Ctrl-;`
 - Remove line comment: `Esc-Ctrl-=`

## Ghostscript

Query settings:
 * https://superuser.com/a/440573
 * https://stackoverflow.com/a/11002313/8590320

## Typora (markdown editor)

MathJax config used by Typora for macOS, version 0.9.9.23.4 (2293).
```js
/* /Applications/Typora.app/Contents/Resources/TypeMark/index.html */
<script type="text/x-mathjax-config" aria-hidden="true">
  MathJax.Hub.Config({
    skipStartupTypeset: true,
    jax: ["input/TeX", "output/SVG"],
    extensions: ["tex2jax.js", "toMathML.js"],
    TeX: {
      extensions: ["noUndefined.js", "autoload-all.js", "AMSmath.js", "AMSsymbols.js", "mediawiki-texvc.js"],
      mhchem: { legacy: false }
    },
    /* more */
  });
</script>

<script src="./lib/MathJax/MathJax.js" aria-hidden="true"></script>
```

## Telegram (IM software)

Hashtag
 * Clickable combination of a `#` and a word like `#smile` showing in messages is called _hashtag_.
 * It is first [introduced in March 2015](https://telegram.org/blog/replies-mentions-hashtags), and can be used to [bring order and structure into group chats](https://telegram.org/blog/replies-mentions).

## Convert `DjVu` to `PDF`

| Item       | Content                                                  |
| ----------- | ------------------------------------------------------------ |
| Preparation | install [DjVuLibre](http://djvu.sourceforge.net/index.html), see `brew info djvulibre` |
| Usage       | `ddjvu -format=pdf [options] input.djvu output.pdf`          |
| Option      | `-<n> -subsample=<n>`, downsize `n` times; default 1, max 12 |
| Doc         | http://djvu.sourceforge.net/doc/man/ddjvu.html               |

## Read and Write Outline of `DjVu`

| Item          | Content                                              |
| ------------- | ---------------------------------------------------- |
| Read Outline  | `djvused input.djvu -e 'print-outline'`              |
| Write Outline | `djvused -e 'set-outline outline.txt' -s input.djvu` |
| Doc           | http://djvu.sourceforge.net/doc/man/djvused.html     |

Format of outline

```lisp
(bookmarks
  ("1 first chapter" "#10" 
    ("1.1 first section" "#11" 
      ("1.1.1 first subsection" "#12" ))
    ("1.2 second section" "#13" ))
  ("2 second chapter" "#14" 
    ("2.1 first section" "#16" )
    ("2.2 second section" "#13" ))
)
```

## PDF Utilities

### Use `QPDF` to check and fix `PDF` file
```bash
# program info
brew info qpdf
# usage
qpdf --check file.pdf
qpdf --qdf input.pdf - | fix-qdf > output.pdf
```

### `poppler` utilities
```bash
# program info
brew info poppler
# utility list
pdfinfo     # show general info
pdffonts    # list info of used fonts
pdfdetach   # extract embedded files
```
