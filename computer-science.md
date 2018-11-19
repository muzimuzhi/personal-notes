# Computer Sciences
Some notes on computer science, mostly online sources.

## Programming Languages

### Racket

Load racket package
```racket
(#%require <racket package name>)

; e.g.
(#%require racket/base)   ; load racket base
(#%require sicp-pict)     ; load sicp picture package
```

Load sub file
```racket
;; main file
(#%require "<path to sub file>")

; e.g.
(#%require "exercise_1.43.rkt")
; load file at the current directory

(#%require "../chapter02/exercise_2.20.rkt")
; load file using relative path

;; sub file
(#%require racket/base)
(provide <function names, space seperated>)

; e.g.
(#%require racket/base)

(provide square)
(define (square x) (* x x))
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


## General Books

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

### Category Theory for Programmers, by Bartosz Milewski

* [HTML version](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/), [PDF version](https://github.com/hmemcpy/milewski-ctfp-pdf), and a link to a series of [lecture videos](https://www.youtube.com/playlist?list=PLbgaMIhjbmEnaH_LTkxLI7FMa2HsnawM_) can be found on the main page of this unpublished 31-chapter book.
* A partial [simplified Chinese translation](https://segmentfault.com/a/1190000003882331), covering the whole 10 chapters of the first part, is contributed by Garfileo.

## Tools

### git

Clean up unlinked commits ([ref](https://stackoverflow.com/a/11759044/8590320)):
```bash
$ git reflog expire --expire=now --all
$ git gc --prune=now
```

Diff non-UTF8 files:
```bash
# Suppose the .tex files are GBK encoding
$ cat .git/config
[diff "gbk"]
        textconv = "iconv -f gbk -t utf-8"

$ cat .gitattributes
*.tex diff=gbk
```