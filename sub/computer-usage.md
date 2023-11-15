# Computer Usage

## Tiny GUI Softwares

* Show live battery information of Mac and iOS device<br />
  [coconut Battery](https://www.coconut-flavour.com/coconutbattery/)<br />
  `brew info coconutbattery`
* Control volume slider each app<br />
  [Background Music](https://github.com/kyleneideck/BackgroundMusic)<br />
  `brew info background-music`

## Libraries

* [pyl](https://github.com/dabeaz/ply) - Python Lex-Yacc
* [pygments](https://github.com/pygments/pygments) - Pythonic syntax highlighter for 300+ languages and text formats
  * used by LaTeX package `minted`
  * structure
    * List of default token types and their short names: https://github.com/pygments/pygments/blob/master/pygments/token.py
* [linguist](https://github.com/github/linguist) - blob languages detection used by GitHub
  * [known languages](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml) to GitHub
  * [grammars used by GitHub](https://github.com/github/linguist/blob/master/vendor/README.md) to do syntax highlighting
  * [test page](https://github-lightshow.herokuapp.com/?utf8=✓&scope=from-url&grammar_format=auto&grammar_url=https%3A%2F%2Fraw.githubusercontent.com%2FAlhadis%2Flanguage-grammars%2Fmaster%2Fgrammars%2Fabnf.cson&grammar_text=&code_source=from-url&code_url=https%3A%2F%2Fraw.githubusercontent.com%2FTadiT7%2Fxiaomi_violet_dump%2F5edc11ebb2c4b5d9a4bfd5ebc335ee7e47f69f56%2Fsystem%2Fsystem%2Fusr%2Fsrec%2Fen-US%2Fcontacts.abnf&code=)
* [cloc](https://github.com/AlDanial/cloc) - count lines of code, in Perl
  * Readme contains full manual
  * Count lines of tex code: `cloc --force-lang=TeX,def --vcs=git`


## Utility Websites

* Archive Extractor<br />
  [extract.me](https://extract.me/)

## Editor

<!-- ### vscode -->
### Visual Studio Code (vscode)

- latex grammar
  https://github.com/jlelong/vscode-latex-basics
  imported by both vscode and its `latex-workshop` extension
  https://github.com/microsoft/vscode/tree/main/extensions/latex
  https://github.com/James-Yu/LaTeX-Workshop/tree/master/syntax

- workspace cache
  stored in `~/Library/Application Support/Code/User/workspaceStorage/<32-length ID>`
  - cleanup cache per workspace
    run command "Workspaces: Cleanup Storage" from extension "Workspace Storage Cleanup"
    https://marketplace.visualstudio.com/items?itemName=mehyaa.workspace-storage-cleanup
    - saw in https://stackoverflow.com/a/75666552

- features
  - Floating editor windows
    since v1.84, insiders only
    https://code.visualstudio.com/updates/v1_84#_floating-editor-windows

### TeXstudio

 - [`CHANGELOG.txt`](https://github.com/texstudio-org/texstudio/blob/master/utilities/manual/CHANGELOG.txt)


### Atom

Manually install an Atom package ([ref](https://github.com/atom/apm/issues/355#issuecomment-99210591)):
```
git clone <package-repo>
cd package-name
apm install
apm link .
```

[Script Package] Change font size of output window ([ref](https://github.com/rgbkrk/atom-script/issues/1191))

- Open config file `~/.atom/packages/script/styles/script.less`
- Add property `font-size: 14px;`.
  ```less
  .script-view {
    // [...]

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

## Decompress files

 - in `.tar.gz`
    ```shell
    tar -zxvf file.tar.gz
    ```

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
| Read Outline  | `djvused input.djvu -e 'print-outline' -u > outline.txt` |
| Write Outline | `djvused -e 'set-outline outline.txt' -s input.djvu` |
| Doc           | [`djvused`'s man page][djvused doc]                  |

[djvused doc]: http://djvu.sourceforge.net/doc/man/djvused.html

Format of an outline

```lisp
(bookmarks
  ("1 First chapter" "#10"
    ("1.1 First section" "#11"
      ("1.1.1 First subsection" "#12"))
    ("1.2 Second section" "#13"))
  ("2 Second chapter" "#14"
    ("2.1 First section" "#16")
    ("2.2 Second section" "#13")))
```

## PDF Utilities

### Use `QPDF` to check and fix PDF file
```bash
# program info
brew info qpdf
# usage
qpdf --check file.pdf
qpdf --qdf input.pdf - | fix-qdf > output.pdf
```

### Use `mutool` to generate human readable and editable PDF
```bash
# aquire program
brew info mupdf
# usage
mutool clean -ad input.pdf output.pdf
```

doc: https://mupdf.readthedocs.io/en/latest/mutool-clean.html


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

### Read and Write Outlines
```bash
pip3 install PyMuPDF
```

```python3
import fitz

doc = fitz.open('test.pdf')
toc = doc.getToC()  # read outlines

# modify toc: List[List]

doc.setToC(toc)     # write outlines
doc.saveIncr()      # saves incrementally
```

Doc: https://pymupdf.readthedocs.io/en/latest/document.html

## Image Processing

### Tools
 - ImageMagick
   - `brew info imagemagick`
   - [CLI options](https://imagemagick.org/script/command-line-options.php)
   - [`-format <expr>`](https://imagemagick.org/script/escape.php)

### Change DPI (density)
```bash
# show dpi
#  - long form `-format '%[units] %[resolution.x],%[resolution.y]\n'`
$ identify -units PixelsPerInch -format '%U %x,%y\n' input.png

# change dpi
$ convert -density 300 -units PixelsPerInch input.png output.png
```

### Remove transparency from a PNG image ([ref](http://www.imagemagick.org/Usage/masking/#alpha_remove), also see [this Q&A](https://stackoverflow.com/q/2322750))
```bash
# use program ImageMagick
convert input.png -background white -alpha remove output.png
```

### Set transparency color for a PNG image
```bash
# make white transparent, allowing a 4% fuzz
# source: https://tex.stackexchange.com/a/611207
convert input.png -fuzz 4% -transparent "#ffffff" output.png
```
Doc for options [`-transparent color`][imagemagick-opt-transparent] and [`-fuzz distance{%}`][imagemagick-opt-fuzz].

[imagemagick-opt-transparent]: https://imagemagick.org/script/command-line-options.php#transparent
[imagemagick-opt-fuzz]: https://imagemagick.org/script/command-line-options.php#fuzz

## Font Selection

### Source Code Pro (before v2.032) can't be styled/colored in Chrome

Solution: Install the latest version by running `brew install iandol/adobe-fonts/font-source-code-pro`

The `homebrew-cask-fonts` switched the source of Source Code Pro from adobe-fonts to google-fonts, while the latter decided to stop pulling from the former since newer versions don't have hinting. But adobe-fonts just removed SVG table from that font to support its use in browser since v2.032 ...

Longer story
 - https://github.com/adobe-fonts/source-code-pro/issues/250
 - https://bugs.chromium.org/p/chromium/issues/detail?id=1100502
 - https://bugzilla.mozilla.org/show_bug.cgi?id=1520157
 - https://github.com/Homebrew/homebrew-cask-fonts/issues/3973

Links
 - https://github.com/adobe-fonts/source-code-pro/releases
 - https://github.com/iandol/homebrew-adobe-fonts

## Gradle

Persist `--console` setting in system-wide `gradle.properties` file

 - system-wide `gradle.properties` file is located in the path stored in `GRADLE_USER_HOME`, which by default is `~/.gradle`
 - add line `org.gradle.console=verbose` to `$GRADLE_USER_HOME/gradle.properties`, after that running `gradle test` is equivalent to `gradle --console=verbose test`

 docs:
  - [Build Environment](https://docs.gradle.org/current/userguide/build_environment.html)
  - [Command-Line Interface / Logging options / Customizing log format](https://docs.gradle.org/current/userguide/command_line_interface.html#sec:command_line_customizing_log_format)
