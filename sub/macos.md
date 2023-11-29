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

- `zip` and `unzip`
    ```bash
    # dry run, list files
    unzip -l file.zip

    # unzip specific file
    unzip file.zip path/to/file.txt     # this will create "./path/to/file.txt"

    # unzip specific file and drop directory structures in archive
    unzip -j file.zip path/to/file.txt  # this will create "./file.txt"
    ```

## Homebrew

### `brew` CLI program

https://docs.brew.sh/Manpage

- Display local location of homebrew itself or one of cloned tap
  https://docs.brew.sh/Manpage#--repository---repo-tap-
  `brew --repository [OWNER/REPO]`
  ```bash
  # /usr/local/Homebrew
  cd $(brew --repository)

  # tap OWNER/REPO is located in $(brew --reposot)/Library/Taps/OWNER/homebrew-REPO
  brew --repo homebrew/casks
  brew --repo dart-lang/dart
  ```

- Show (installed) dependents of a formula
  https://docs.brew.sh/Manpage#uses-options-formula-
  `brew uses --recursive --installed <formula>`
  seen in https://stackoverflow.com/a/66142860

- Show dependencies of a formula
  https://docs.brew.sh/Manpage#deps-options-formulacask-
  `brew deps <formula>`

### Writing formulae and casks

- Formula Cookbook
  https://docs.brew.sh/Formula-Cookbook

- Cask Cookbook
  https://docs.brew.sh/Cask-Cookbook

- [`version` Stanza Details](https://github.com/Homebrew/homebrew-cask/blob/master/doc/cask_language_reference/stanzas/version.md)
  - Example: [Homebrew/homebrew-cask-fonts#2082](https://github.com/Homebrew/homebrew-cask-fonts/issues/2082)

## GNU, instead of BSD CLI Tools
* Show dependencies as a tree: `brew deps --tree <formula>`

| Name    | Homebrew formula    |
| ------- | ------------------- |
| `ggrep` | `brew info grep`    |
| `gsed`  | `brew info gnu-sed` |
| `gsort` | `brew info coreutils` |

## Where do PATHs come from

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
