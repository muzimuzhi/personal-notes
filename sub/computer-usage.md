# Computer Usage

## Markdown

  * disable auto-linking: insert `<span></span>`<BR>
    ref: https://gist.github.com/alexpeattie/4729247
  * [GitHub Flavored Markdown Specification](https://github.github.com/gfm/)

## Tiny Useful Softwares

* Show live battery information of Mac and iOS device<BR>
  [coconut Battery](https://www.coconut-flavour.com/coconutbattery/)<BR>
  `brew cask info coconutbattery`
* Control volume slider each app<BR>
  [Background Music](https://github.com/kyleneideck/BackgroundMusic)<BR>
  `brew cask info background-music`

## Utility Websites

* Archive Extractor<BR>
  [extract.me](https://extract.me/)

## Convert `DjVu` to `PDF`

| Steps       | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| Preparation | install [DjVuLibre](http://djvu.sourceforge.net/index.html), see `brew info djvulibre` |
| Usage       | `ddjvu -format=pdf [options] input.djvu output.pdf`          |
| Option      | `-<n> -subsample=<n>`, downsize `n` times; default 1, max 12 |
| Doc         | http://djvu.sourceforge.net/doc/man/ddjvu.html               |
