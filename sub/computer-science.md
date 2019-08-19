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

* step 1. export identifiers in the included sub file
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

* step 2. load sub file in the main file
```racket
;; main file
(#%require "<path to sub file>")

;; e.g.
; load file with relative path
(#%require "exercise_1.43.rkt")
(#%require "../chapter02/exercise_2.20.rkt")
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

### Python

 decorator
  * Glossary - [decorator][python-decorator]

 | Reference                                          | First joined version                 | Python Enhancement Proposals |
 | -------------------------------------------------- | ------------------------------------ | ---------------------------- |
 | [Function decorator][function-decorator-reference] | [2.4.1][function-decorator-whatsnew] | [PEP 318][pep-318]           |
 | [Class decorator][class-decorator-reference]       | [2.6][class-decorator-whatsnew]      | [PEP 3129][pep-3129]         |


[python-decorator]: https://docs.python.org/3/glossary.html#term-decorator
[function-decorator-reference]: https://docs.python.org/3/reference/compound_stmts.html#function-definitions
[class-decorator-reference]: https://docs.python.org/3/reference/compound_stmts.html#class-definitions
[function-decorator-whatsnew]: https://docs.python.org/3/whatsnew/2.4.html#pep-318-decorators-for-functions-and-methods
[class-decorator-whatsnew]: https://docs.python.org/3/whatsnew/2.6.html#pep-3129-class-decorators
[pep-318]: https://www.python.org/dev/peps/pep-0318/
[pep-3129]: https://www.python.org/dev/peps/pep-3129/

## General Books

### Introduction to Computation and Programming Using Python, Second Edition

* Subtitle: With Application to Understanding Data
* [Page on MIT Press](https://mitpress.mit.edu/books/introduction-computation-and-programming-using-python-second-edition), containing errata
* Page of book author [John Guttag](https://people.csail.mit.edu/guttag/)

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

Diff between arbitrary files:
```bash
git diff --no-index <old-file> <new-file>
```

Delete remote tracking branch (delete the local tracking only, [ref](https://gist.github.com/magnusbae/10182865))
```bash
$ git branch --delete --remotes <remote_name>/<branch_name>
```

Delete remote branch (delete the remote branch while pushing)
```bash
$ git push --delete <remote_name> <branch_name>
```

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
 - [Column count of mysql.user is wrong. Expected 42, found 44.](https://stackoverflow.com/a/45434694/8590320) - Answer on StackOverflow
 - [Can't find mysql.infoschema after update from 5.7](https://stackoverflow.com/a/51155179/8590320) - Answer on StackOverflow
 - [4.4.5 mysql_upgrade — Check and Upgrade MySQL Tables](https://dev.mysql.com/doc/refman/8.0/en/mysql-upgrade.html) - MySql Ref Manual
 