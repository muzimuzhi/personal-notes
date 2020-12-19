# Notes relating macOS

## macOS Shortcuts

| Key                    | Function                                   |
| ---------------------- | ------------------------------------------ |
| cmd + ` (grave accent) | switch between windows of a same program |

Shortcut List
 * [Mac keyboard shortcuts](https://support.apple.com/en-us/HT201236) - support.apple.com
 * [Mac OS X Finder Keyboard Shortcuts](https://www.dummies.com/computers/macs/macbook/mac-os-x-finder-keyboard-shortcuts/) - dummies.com

## BSD Bash Usage

Display disk usage statistics sorted by human readable numbers ([ref](https://serverfault.com/a/156648))
```bash
# use GNU sort which supports the missing option "-h"
$ du -hd 1 | gsort -h
```

Open man page in new terminal window ([ref](https://scriptingosx.com/2017/04/on-viewing-man-pages/))
```bash
$ open x-man-page://<name>
```

## Homebrew and Homebrew-cask

* [`version` Stanza Details](https://github.com/Homebrew/homebrew-cask/blob/master/doc/cask_language_reference/stanzas/version.md)
  * Example: [Homebrew/homebrew-cask-fonts#2082](https://github.com/Homebrew/homebrew-cask-fonts/issues/2082)

## GNU, instead of BSD CLI Tools

| Name    | Homebrew formula    |
| ------- | ------------------- |
| `ggrep` | `brew info grep`    |
| `gsed`  | `brew info gnu-sed` |
| `gsort` | `brew info coreutils` |

## Where PATHs come from

1. `/etc/profiles`
1. `/usr/libexec/path_helper -s`
1. `/etc/paths` + file lines under `/etc/paths.d`
1. shell profiles (e.g., [zsh startup files](http://zsh.sourceforge.net/Doc/Release/Files.html#Startup_002fShutdown-Files))
see [ref](https://scriptingosx.com/2017/05/where-paths-come-from/)

## Zsh

Remove duplicates in zsh history ([ref](https://qr.ae/pNk9yZ))
```bash
cat -n $HISTFILE | sort -t ';' -uk2 | sort -nk1 | cut -f2- > .zsh_short_history
```
