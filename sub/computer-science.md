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
                     <identifiers, space separated>))

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
    (provide <identifiers, space separated>)

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

* Specifications and Implementations
  * CommonMark
    * spec https://spec.commonmark.org/
    * spec repo https://github.com/commonmark/commonmark-spec/
    * online demo https://spec.commonmark.org/dingus/
    * library https://github.com/commonmark/cmark
  * GitHub Flavored Markdown (GFM)
    * spec https://github.github.com/gfm/
    * spec source https://github.com/github/cmark-gfm/blob/master/test/spec.txt
      (got from https://talk.commonmark.org/t/location-of-gfm-spec/4250 and https://github.com/github/cmark-gfm/issues/93)
    * library https://github.com/github/cmark-gfm
    * some extra (DOM-based?) GitHub extensions are not included
      * autolink extension skips `ftp://` uris https://github.com/github/cmark-gfm/issues/177
      * [alerts][gfm-alert] not included https://github.com/github/cmark-gfm/issues/350
  * Relation between CommonMark and GFM
    * GFM spec and library are both based on CommonMark's
      https://github.blog/2017-03-14-a-formal-spec-for-github-markdown/
    * 0.29-gfm (2019-04-06) is based on CommonMark v0.29 (2019-04-06)
    * in Oct 2023, the latest CommonMark version is v0.30 (2021-06-19)
    * will GFM spec sync with that of CommonMark?
      https://github.com/github/cmark-gfm/issues/259 (closed by issue author on 2023-10-03 with disappointment)
  * GitLab Flavored Markdown (GLFM)
    https://docs.gitlab.com/ee/development/gitlab_flavored_markdown/
  * "Markdown Variants" registry established by IANA (Internet Assigned Numbers Authority)
    https://www.iana.org/assignments/markdown-variants/markdown-variants.xhtml
    * mentioned in RFC7763 (informational), The text/markdown Media Type
      https://www.rfc-editor.org/rfc/rfc7763
      https://datatracker.ietf.org/doc/html/rfc7763
    * saw in https://sspai.com/prime/story/free-markdown-editor-2023

* babelmark3, online comparison among Markdown implementations
  https://babelmark.github.io/
  * last updated Oct 2018, main author remains active on GitHub
  * saw in https://talk.commonmark.org/t/is-first-underdefined-in-the-commonmark-spec/1276/3

* Adoption
  * As of June 20, 2020, all Stack Overflow sites are on CommonMark.
    * [We're switching to CommonMark](https://meta.stackexchange.com/q/348746)
    * [Allowed HTML tags and attributes](https://meta.stackexchange.com/a/135909)

[gfm-alert]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts

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
