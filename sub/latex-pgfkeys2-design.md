# Design comments for `pgfkeys2`

## General purpose

* types, one key must belong to one and only one type
  * store in
  * code
  * style
* attributes?
  * arguments
  * code
  * body?
  * accumulated value
  * last value
* handlers that can perform on all types
  * `.default`
  * `.add`, `.prepend`, `.append`
* handlers that can only perform on specific type(s)
* more useful handlers
  * expansion controller, expl3 arg-spec?
  * arithmetic, those in `forest` and `tcolorbox` pkg
* consistent brace stripping

And possibly more
 * split pure style keys and styles that accept arguments
   * `.code` and `.style` accept no arguments
   * `.flatten style`?
   * `.code n args` and `.style n args` accept 0 to 9 mandatory arguments
   * `.code expl3 args`
   * `.code args`
 * quick choices?
 * show or print info about a key, when redefining
 * pre-process the key-val list

## Implementation details
  * full eTeX and `\expanded` usage
  * deliberately avoid `\toks`

  * value filter/validator? https://tex.stackexchange.com/q/565781


## Notes

```tex
\tikz{key1, key2=, key3=val3, key4/.code=...}

\pgfkeyscurrentkey := seen key name, "key4/.code"


```
