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

## macOS

Homebrew

* install `smlnj` 110.84 on macOS 10.13 through homebrew
  * `brew install --force-bottle smlnj`
  * [explanation](https://github.com/Homebrew/homebrew-core/issues/32722#issuecomment-427260927): Xcode 10 on macOS 10.13 does not support 32-bit builds anymore, and smlnj does not (yet) support 64-bit builds. So an option `--force-bottle` is used to force installing binaries built.