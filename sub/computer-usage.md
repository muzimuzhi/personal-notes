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

* Preparation
  * `brew info djvulibre`, or
  * [DjVuLibre](http://djvu.sourceforge.net/index.html) on SourceForge
* Usage<BR>
  `ddjvu -format=pdf input.djvu output.pdf`
  * resolution controlling option<BR>
    `-<num> -subsample=<num>, default 1`<BR>
    The dimensions of the full output image will be `<num>` times smaller than the DjVu image size. The legal values for argument `<num>` range from 1 to 12.
* [Online doc](http://djvu.sourceforge.net/doc/man/ddjvu.html) 