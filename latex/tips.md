# Tips About Using LaTeX

## Which version of format am I using?

* The format version appears in the first line of every `.log` file (note the date **2018.5.3** following "preloaded format="):
   ```
   This is XeTeX, Version 3.14159265-2.6-0.99999 (TeX Live 2018) (preloaded format=xelatex 2018.5.3)  2 OCT 2018 11:39
   ```
* Put `\fmtversion` in your `.tex` file, and it will out put the format version into the `.pdf` file, in a format of `yyyy/mm/dd` (e.g. `2018/04/01`).
* Since the format version is restored in command `fmtversion`, you can get it from the terminal, with
    ```bash
    $ latexdef fmtversion

    \fmtversion:
    macro:->2018-04-01
    ```
