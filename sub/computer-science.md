# Computer Sciences
Some notes on computer science, mostly online sources.

## Programming Languages

### Racket

Load racket package, when `#lang sicp` is used

* load a whole package
    ```racket
    (#%require <racket package name>)

    ;; e.g.
    (#%require racket/base)   ; load racket base
    (#%require sicp-pict)     ; load sicp picture package
    ```
* load selected identifiers from package
    ```racket
    (#%require (only <racket package name>
                     <identifiers, space seperated>))

    ;; e.g.
    ; load `provide` and `all-defined-out` from racket base,
    ; and load these two only
    (#%require (racket/base provide all-defined-out))
    ```

Load sub file, when `#lang sicp` is used

1. export identifiers in the included sub file
    ```racket
    ;; sub file
    ; export selected identifiers
    (provide <identifiers, space seperated>)

    ; export all defined identifiers in the current file
    (provide (all-defined-out))

    ;; e.g. 1
    (#%require (only racket/base provide))

    (provide square double)
    (define (square x) (* x x))
    (define (double x) (+ x x))

    ;; e.g. 2
    (#%require (only racket/base provide all-defined-out))
    (provide (all-defined-out))

    (define (square x) (* x x))
    (define (double x) (+ x x))
    (define (halve x)
      (if (even? x)
          (/ x 2)
          (error "ERROR: invalid input, not even")))
    ```
1. load sub file in the main file
    ```racket
    ;; main file
    (#%require "<path to sub file>")

    ;; e.g.
    ; load file with relative path
    (#%require "exercise_1.43.rkt")
    (#%require "../chapter02/exercise_2.20.rkt")
    ```

Run code only when current file is not required as a module
 - This is similar to `if __name__ == "__main__":` in python.
 - See doc of [`main` submodule][racket-main-submodule]and [answer](https://stackoverflow.com/a/28591678)

[racket-main-submodule]: https://docs.racket-lang.org/guide/Module_Syntax.html#%28part._main-and-test%29

```racket
#lang sicp

(#%require (only racket/base module+))
(module+ main
  ...)  ; the body accumulates
        ; and `main` is executed at the end of current module
```

### Scheme

* [Code Style](http://community.schemewiki.org/?scheme-style) from Community Scheme Wiki
* [Comment Style](http://community.schemewiki.org/?comment-style) from Community Scheme Wiki
* Choice of using dialect of Scheme used by SICP:
  * [MIT Scheme](https://www.gnu.org/software/mit-scheme/) maintained by GNU, or
  * [SICP collections](https://docs.racket-lang.org/sicp-manual/index.html) as a package of Racket

### C

Textbook

The C Programming Language, 2nd Edition, by Kernighan and Ritchie (K&R2)
* [Solutions](https://clc-wiki.net/wiki/K&R2_solutions) provided by clc-wiki, an offshoot of the comp.lang.c newsgroup

Discussion

* [How to C in 2016](https://matt.sh/howto-c), by Matt
  * [Critique](https://github.com/Keith-S-Thompson/how-to-c-response), by Keith Thompson

### [Python](python.md)

## Other Languages, mostly markup ones

### Markdown

* Specification and Implementation (parsing and rendering library)
  * [CommonMark](https://spec.commonmark.org/) and library [`commonmark/cmark`](https://github.com/commonmark/cmark)
  * [GitHub Flavored Markdown](https://github.github.com/gfm/) (GFM) and library [`github/cmark-gfm`](https://github.com/github/cmark-gfm), both based on CommonMark's
* Special Usages
  * Use `<span></span>` to disable auto-linking ([ref](https://gist.github.com/alexpeattie/4729247))
  * Use `<summary>` element to hide long contents ([MDN doc](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/summary), [usage example on GitHub](https://github.com/T-F-S/tcolorbox/issues/93#issuecomment-574827784)):
    ```html
    <details>
        <summary>short summary</summary>
        loooooooong contents
    </details>
    ```
* Application
  * As of June 20, 2020, all Stack Overflow sites are on CommonMark.
    * [We're switching to CommonMark](https://meta.stackexchange.com/q/348746)
    * [Allowed HTML tags and attributes](https://meta.stackexchange.com/a/135909)

### HTML

  * Default link colors ([ref to HTML Spec](https://html.spec.whatwg.org/multipage/rendering.html#phrasing-content-3), [answer](https://stackoverflow.com/a/4774037))
    ```css
    :link { color: #0000EE; }
    :visited { color: #551A8B; }
    :link:active, :visited:active { color: #FF0000; }
    :link, :visited { text-decoration: underline; cursor: pointer; }
    ```

### JSON (JavaScript Object Notation)

* Specification: [RFC 8259](https://tools.ietf.org/html/rfc8259) (with [errata](https://www.rfc-editor.org/errata_search.php?rfc=8259)) and [ECMA-404](http://www.ecma-international.org/publications/standards/Ecma-404.htm)

### ABNF (Augmented BNF)

* Introduction: [wikipedia page](https://en.wikipedia.org/wiki/Augmented_Backus–Naur_form)
* Specification:
  * [RFC 7405](https://tools.ietf.org/html/rfc7405), which adds syntax for case-sensitive string literals based on [RFC 5234](https://tools.ietf.org/html/rfc5234)
* Lex used by GitHub: [language-grammars](https://github.com/Alhadis/language-grammars/blob/master/grammars/abnf.cson) by Alhadis, not finished yet (19 Aug 27)

### EBNF (Extended BNF)

* Introduction: [wikipedia page](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)
* Specifications:
  * [ISO/IEC 14977:1996](https://standards.iso.org/ittf/PubliclyAvailableStandards/s026153_ISO_IEC_14977_1996(E).zip), not recommended
  * Variant used by [XML Spec. 5th](https://www.w3.org/TR/REC-xml/#sec-notation)
* Lex used by GitHub: [language-grammars](https://github.com/Alhadis/language-grammars/blob/master/grammars/abnf.cson) by Alhadis, not finished yet (19 Aug 27)

## General Books

### Introduction to Computation and Programming Using Python, Second Edition

* Subtitle: With Application to Understanding Data
* [Page on MIT Press](https://mitpress.mit.edu/books/introduction-computation-and-programming-using-python-second-edition), containing errata
* Page of book author [John Guttag](https://people.csail.mit.edu/guttag/)

### How to Design Programs (2e)

* [Online full-text](https://htdp.org), both 1st (with full solutions) and 2nd editions.
* From [slides of talk](https://con.racket-lang.org/2011/matthias-slides.pdf) Matthias Felleisen gave at RacketCon 2011, there is a four-books plan with HtDP and How to Design Components (now called Classes, see [webpage of HtDC](https://felleisen.org/matthias/htdc)) the first two.

### Structure and Interpretation of Computer Programs (SICP)

* Scheme vs Python (discussions about learning SICP with Scheme or with Python)
  * [Explanation](https://people.eecs.berkeley.edu/~bh/proglang.html) given by Brian Harvey, staff of Berkeley course 61A
  * Articles ([1](http://www.posteriorscience.net/?p=206), [2](https://cemerick.com/2009/03/24/why-mit-now-uses-python-instead-of-scheme-for-its-undergraduate-cs-program/)) based on a [talk](https://vimeo.com/151465912#t=59m36s) given by Gerry Sussman, one of the author of book, at 2016 NYC Lisp meet-up
* Open-Sourced Book and solutions
  * [Original HTML version](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book.html) provided by The MIT Press
  * [Nice-looking HTML version](http://sarabander.github.io/sicp/) with modern HTML techniques, provided by [Andres Raba](https://github.com/sarabander/sicp) with [EPUB](https://github.com/sarabander/sicp-epub) and [PDF](https://github.com/sarabander/sicp-pdf) versions as well
  * [Full solution](http://community.schemewiki.org/?SICP-Solutions) with discussions and explanations, provided by Community Scheme Wiki
* Readings
  * Iteration and tail recursive
    * SICP Iteration Exercise: http://wiki.c2.com/?SicpIterationExercise
    * An Interactive Change-Counting Procedure: https://logicgrimoire.wordpress.com/2013/02/16/an-iterative-change-counting-procedure/
    * Attempting to Count Change, Iteratively: https://logicgrimoire.wordpress.com/2013/01/27/attempting-to-count-change-iteratively/

### Algorithms in a Nutshell (2e)

* [Code Repo](https://github.com/heineman/algorithms-nutshell-2ed)
* [Errata](http://shop.oreilly.com/product/0636920032885.do)

### Category Theory for Programmers, by Bartosz Milewski

* [HTML version](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/), [PDF version](https://github.com/hmemcpy/milewski-ctfp-pdf), and a link to a series of [lecture videos](https://www.youtube.com/playlist?list=PLbgaMIhjbmEnaH_LTkxLI7FMa2HsnawM_) can be found on the main page of this unpublished 31-chapter book.
* A partial [simplified Chinese translation](https://segmentfault.com/a/1190000003882331), covering the whole 10 chapters of the first part, is contributed by Garfileo.

### Parsing Techniques - A Practical Guide, by Dick Grune and Ceriel J.H. Jacobs

* [1st edition](https://dickgrune.com/Books/PTAPG_1st_Edition/) (1990)
* [2nd edition](https://dickgrune.com/Books/PTAPG_2nd_Edition/) (2008)
* [Chinese translation project (2e)](http://parsing-techniques.duguying.net/)

### Pattern Recognition and Machine Learning, by Christopher Bishop

* Springer (2006)
* [Book page on MS research](https://www.microsoft.com/en-us/research/publication/pattern-recognition-machine-learning/), PDF available

### Dive into Deep Learning

* Authors: Aston Zhang, Zack C. Lipton, Mu Li, and Alex J. Smola
* Introduction: A free and interactive deep learning book for students, engineers, and researchers.
* Online version: [en](https://d2l.ai/), [zh-cn](https://zh.d2l.ai/)

### Advanced Programming in the UNIX Environment

http://www.apuebook.com/

## Tools

### git

General info

 * Per command documentation: `https://git-scm.com/docs/git-<command>`
 * [Release notes](https://github.com/git/git/tree/master/Documentation/RelNotes)

Configuration

 * Print pathnames in Unicode, other than octal UTF-8 ([ref](https://stackoverflow.com/a/22828826))
    ```bash
    git config [--global] core.quotepath off
    ```
 * Diff non-UTF8 files:
    ```bash
    # Suppose the .tex files are in GBK encoding
    $ cat .git/config
    [diff "gbk"]
            textconv = "iconv -f gbk -t utf-8"

    $ cat .gitattributes
    *.tex diff=gbk
    ```
 * Show whitespace changes in `git diff` ([doc](https://git-scm.com/docs/git-config#Documentation/git-config.txt-diffwsErrorHighlight))
    ```bash
    $ git config [--global] diff.wsErrorHighlight all
    ```
 * Pretty one-line `git log` ([ref](https://ma.ttias.be/pretty-git-log-in-one-line/))
    ```bash
    # set git alias
    $ git config --global alias.logline "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold cyan)<%an>%Creset' --abbrev-commit"

    # use git alias
    $ git logline
    ```

Show status

  * Show individual files in untracked directories ([ref](https://git-scm.com/docs/git-status#Documentation/git-status.txt--ultmodegt))
    ```bash
    # short option: -u
    git status --untracked-files
    ```

Clone and fetch
 * Shallow clone: `git clone --depth=<num>`
 * Convert a shallow clone to full clone ([ref](https://stackoverflow.com/a/17937889))
    ```bash
    git fetch --unshallow
    ```

Commit changes
 * Update commit author (and email): `git commit --amend --author="Author <author@example.com>"` ([official doc](https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---authorltauthorgt))

Workflow
 * Keep your branch clean with [`--fixup`][git-commit-fixup] and [`--autosquash`][git-rebase-autosquash] ([article](https://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html))
    ```bash
    # `--fix` automatically marks your commit as a fix of a previous commit
    # The resulted commit message will be "fixup! <msg of referred commit>"
    git commit --fixup <commit>
    # `--autosquash` automatically organizes merging of  fixup commits and associated normal commits
    git rebase [-i] --autosquash <upstream>
    ```

[git-commit-fixup]: https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---fixupamendrewordltcommitgt
[git-rebase-autosquash]: https://git-scm.com/docs/git-rebase#Documentation/git-rebase.txt---autosquash

Branch tracking

 * Create a local branch that tracks a remote one
    ```bash
    git fetch <remote> <branch>:<local branch>
    ```
 * Delete a remote-tracking branch (the corresponding local branch is unchanged, [ref](https://stackoverflow.com/a/3046478))
    ```bash
    git branch --delete --remotes <remote>/<branch>
    ```
 * Track new remote branch after a shallow clone ([ref](https://stackoverflow.com/a/27393574))
    ```bash
    # shallow clone
    $ git clone --depth=<num> <repository> [<directory>]

    # change the list of branches tracked
    $ git remote set-branches <remote> '*'
    # or
    $ git remote set-branches --add <remote> <new_branch>

    # add track to new remote branch
    $ git fetch <remote> <new_branch>
    ```
    wait for test: `git fetch --update-shallow <remote> <branch>`

Add tags
    ```bash
    # add a lightweight tag
    git tag -a <tag>
    # add an annotated tag
    git tag -a <tag> -m "annot"

    # list only lightweight tags
    # ref: https://stackoverflow.com/a/67687543
    git for-each-ref refs/tags | grep commit

    # list only annotated tags
    git for-each-ref refs/tags | grep -v commit
    ```

Push to remote

 * Push single tag ([Q&A](https://stackoverflow.com/a/23212493))
    ```bash
    # to resolve tag/branch name clashes, use "refs/tags/<tag>"
    git push <remote> <tag>
    ```
    Differences between annotated (`-m <message>`) and unannotated tags: [this Q&A](https://stackoverflow.com/q/11514075)

Change remote
 * Delete remote branch or tag
    ```bash
    git push --delete <remote> <branch/tag>
    ```

Show log
 * List commits that changed a specific file ([ref](https://stackoverflow.com/a/8808453))
    ```bash
    git log --follow -- filename
    ```
 * Show first commit ([ref](https://stackoverflow.com/a/5188990))
    ```bash
    git log --reverse
    ```

Clean up unlinked commits ([ref](https://stackoverflow.com/a/11759044))
```bash
git reflog expire --expire=now --all
git gc --prune=now
```

Shorten local clone, just to save disk space ([ref](https://stackoverflow.com/a/37105443))
```bash
git fetch --depth=1
git reflog expire --expire-unreachable=now --all
git gc --aggressive --prune=all
```

Diff between arbitrary files:
```bash
git diff --no-index <file a> <file b>
```

Count the commits for current branch ([ref](https://stackoverflow.com/a/11657647))
```bash
git rev-list --count HEAD
```

### SSH

* [GitHub SSH Docs](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)
* [GitLab SSH Docs](https://gitlab.com/help/ssh/README.md)

### MySql

When `SHOW DATABASES;` showing
```bash
ERROR 1449 (HY000): The user specified as a definer ('mysql.infoschema'@'localhost') does not exist
```
after updating the `mysql`, run
```mysql
mysql_upgrade --force -uroot -p
```
to upgrade tables.

Related links:
 - [Column count of mysql.user is wrong. Expected 42, found 44.](https://stackoverflow.com/a/45434694) - Answer on StackOverflow
 - [Can't find mysql.infoschema after update from 5.7](https://stackoverflow.com/a/51155179) - Answer on StackOverflow
 - [4.4.5 mysql_upgrade - Check and Upgrade MySQL Tables](https://dev.mysql.com/doc/refman/8.0/en/mysql-upgrade.html) - MySql Ref Manual
