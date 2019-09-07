# Computer Usage

## Markdown

  * [GitHub Flavored Markdown Specification](https://github.github.com/gfm/)
  * disable auto-linking: insert `<span></span>`<br />
    ref: https://gist.github.com/alexpeattie/4729247
  * Use `<summary>` element to hide long content ([MDN doc](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/summary)):
    ```html
    <details>
      <summary>short summary</summary>
      loooooooong contents
    </details>
    ```  

## Tiny Useful Softwares

* Show live battery information of Mac and iOS device<br />
  [coconut Battery](https://www.coconut-flavour.com/coconutbattery/)<br />
  `brew cask info coconutbattery`
* Control volume slider each app<br />
  [Background Music](https://github.com/kyleneideck/BackgroundMusic)<br />
  `brew cask info background-music`

## Libraries

* [pyl](https://github.com/dabeaz/ply) - Python Lex-Yacc


## Utility Websites

* Archive Extractor<br />
  [extract.me](https://extract.me/)

## Atom (editor)

[Script Package] Change font size of output window ([ref](https://github.com/rgbkrk/atom-script/issues/1191))

 - Location of config file: `~/.atom/packages/script/styles/script.less`

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