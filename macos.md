# Notes relating macOS

## macOS Shortcuts

| Key                    | Function                                   |
| ---------------------- | ------------------------------------------ |
| cmd + ` (grave accent) | switch between windows of a same program |

## Program Installation

`smlnj`

* install `smlnj` 110.84 on macOS 10.13 through homebrew
  * `brew install --force-bottle smlnj`
  * [explanation](https://github.com/Homebrew/homebrew-core/issues/32722#issuecomment-427260927): Xcode 10 on macOS 10.13 does not support 32-bit builds anymore, and smlnj does not (yet) support 64-bit builds. So an option `--force-bottle` is used to force installing binaries built.
