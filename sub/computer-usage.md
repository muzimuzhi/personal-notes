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
  * used by LaTeX package `minted`
  * notable issue:
    * [#1529 unicode not working for python](https://bitbucket.org/birkenfeld/pygments-main/issues/1529/unicode-not-working-for-python)
  * structure
    * List of default token types and their short names: https://github.com/pygments/pygments/blob/master/pygments/token.py
* [linguist](https://github.com/github/linguist) - blob languages detection used by GitHub
  * [known languages](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml) to GitHub
  * [grammars used by GitHub](https://github.com/github/linguist/blob/master/vendor/README.md) to do syntax highlighting
  * [test page](https://github-lightshow.herokuapp.com/?utf8=✓&scope=from-url&grammar_format=auto&grammar_url=https%3A%2F%2Fraw.githubusercontent.com%2FAlhadis%2Flanguage-grammars%2Fmaster%2Fgrammars%2Fabnf.cson&grammar_text=&code_source=from-url&code_url=https%3A%2F%2Fraw.githubusercontent.com%2FTadiT7%2Fxiaomi_violet_dump%2F5edc11ebb2c4b5d9a4bfd5ebc335ee7e47f69f56%2Fsystem%2Fsystem%2Fusr%2Fsrec%2Fen-US%2Fcontacts.abnf&code=)


## Utility Websites

* Archive Extractor<br />
  [extract.me](https://extract.me/)

## Atom (editor)

Manually install an Atom package ([ref](https://github.com/atom/apm/issues/355#issuecomment-99210591)):
```
git clone <package-repo>
cd package-name
apm install
apm link .
```

[Script Package] Change font size of output window ([ref](https://github.com/rgbkrk/atom-script/issues/1191))

 - Open config file `~/.atom/packages/script/styles/script.less`
 - add property `font-size: 14px;`.
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
 * https://stackoverflow.com/a/11002313

## MathJax

 * [Convert configuration from `MathJax` 2.x to 3.x][mathjax config converter]

[mathjax config converter]: https://mathjax.github.io/MathJax-demos-web/convert-configuration/convert-configuration.html

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
 * It is first [introduced in March 2015][tg-blog hashtag], and can be used to [bring order and structure into group chats][tg-blog reply].

[tg-blog hashtag]: https://telegram.org/blog/replies-mentions-hashtags
[tg-blog reply]: https://telegram.org/blog/replies-mentions

## Convert `DjVu` to `PDF`

| Item          | Content                                                      |
| ------------- | ------------------------------------------------------------ |
| Preparation   | install [DjVuLibre][djvulibre], see `brew info djvulibre`    |
| Basic Usage   | `ddjvu -format=pdf [options] input.djvu output.pdf`          |
| Useful option | `-<n> -subsample=<n>`, downsize `n` times; default 1, max 12 |
| Doc           | [`ddjvu`'s man page][ddjvu doc]                              |

[djvulibre]: http://djvu.sourceforge.net/index.html
[ddjvu doc]: http://djvu.sourceforge.net/doc/man/ddjvu.html

## Read and Write Outline of `DjVu`

| Item          | Content                                              |
| ------------- | ---------------------------------------------------- |
| Read Outline  | `djvused input.djvu -e 'print-outline'`              |
| Write Outline | `djvused -e 'set-outline outline.txt' -s input.djvu` |
| Doc           | [`djvused`'s man page][djvused doc]                  |

[djvused doc]: http://djvu.sourceforge.net/doc/man/djvused.html

Format of an outline

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
# info
brew info poppler
brew list poppler

# utilities
pdfinfo     # show pdf info
pdffonts    # list used fonts in a pdf
pdfdetach   # extract embedded files from pdf
```

## Image Processing

### Remove transparency from a PNG image ([ref](http://www.imagemagick.org/Usage/masking/#alpha_remove), also see [this Q&A](https://stackoverflow.com/q/2322750))
```bash
# use program ImageMagick
convert input.png -background white -alpha remove output.png
```
