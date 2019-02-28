# Personal Notes
These are some notes in the result of my daily web searching and time wasting.

* Logic
  * [Stanford Introduction to Logic](http://logic.stanford.edu/intrologic/homepage/index.html), an online course on symbolic logic
  * [Teach Yourself Logic](https://www.logicmatters.net/tyl/), a study guide provided by Peter Smith on his [Logic Matters blog](https://www.logicmatters.net/)
* Math
  * [Math books](/math-books.md)
  * [Math Notes](/math-notes.md)
* [Computer Science](/computer-science.md)
* LaTeX
  * Guideline to docs and other resources
    * recommended LaTeX packages (TODO)
    * valuable documents on CTAN (TODO)
    * [other online resources](/latex/online-resources.md)
  * [Tips](/latex/tips.md)
  * [List](/latex/interesting-packages.md) of interesting packages I'm not familiar with
  * [Something may help](/latex/may-help.md) in the future
* Typography
  * Examples
    * [The MagPi](https://www.raspberrypi.org/magpi/), the official Raspberry (monthly) Pi magazine, started from May, 2012 and provides free downloads.
* [macOS](./macos.md)

---

Markdown

  * disable auto-linking: insert `<span></span>`<BR>
    ref: https://gist.github.com/alexpeattie/4729247
  * [GitHub Flavored Markdown Specification](https://github.github.com/gfm/)

Tiny Useful Softwares

* Show live battery information of Mac and iOS device<BR>
  [coconut Battery](https://www.coconut-flavour.com/coconutbattery/)<BR>
  `brew cask info coconutbattery`
* Control volume slider each app<BR>
  [Background Music](https://github.com/kyleneideck/BackgroundMusic)<BR>
  `brew cask info background-music`

Utility Websites

* Archive Extractor<BR>
  [extract.me](https://extract.me/)

Convert `DjVu` to `PDF`
* Preparation
  * `brew info djvulibre`, or
  * [DjVuLibre](http://djvu.sourceforge.net/index.html) on SourceForge
* Usage<BR>
  `ddjvu -format=pdf input.djvu output.pdf`
  * resolution controlling option<BR>
    `-<num> -subsample=<num>, default 1`<BR>
    The dimensions of the full output image will be `<num>` times smaller than the DjVu image size. The legal values for argument `<num>` range from 1 to 12.
* [Online doc](http://djvu.sourceforge.net/doc/man/ddjvu.html) 
