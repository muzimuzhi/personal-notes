# Software Usage

## Git

General info

- `git` documentation https://git-scm.com/docs/git
  - all commands, divided into high level and low level ones
    https://git-scm.com/docs/git#_git_commands
  - `git reset`, `git restore` and `git revert`
    https://git-scm.com/docs/git#_reset_restore_and_revert
    https://stackoverflow.com/a/58003889
  - Identifier terminology, `<object>`, `<tree-ish>`, `<commit-ish>`, etc.
    https://git-scm.com/docs/git#_identifier_terminology
    `<commit-ish>` vs `<tree-ish>`: https://stackoverflow.com/q/23303549/
- https://git-scm.com/docs/gitglossary
- `git <command>` documentation: `https://git-scm.com/docs/git-<command>`
- Pro Git book (living)
  https://git-scm.com/book/en/v2
  https://github.com/progit/progit2
- [Release notes](https://github.com/git/git/tree/master/Documentation/RelNotes)
- Highlights from Git releases
  - https://github.blog/2019-08-16-highlights-from-git-2-23
    - new commands `git switch` and `git restore`

Configuration

- new syntaxes and options since v2.46.0
  for example, `git config KEY VALUE` => `git config set KEY VALUE`
  https://git-scm.com/docs/git-config#_deprecated_modes
  https://github.blog/open-source/git/highlights-from-git-2-46/
- Syntax of config files (`.git/config`, `$HOME/.gitconfig`)
  https://git-scm.com/docs/git-config#_syntax
- Print pathnames in Unicode, other than octal UTF-8 ([ref](https://stackoverflow.com/a/22828826))
  ```bash
  git config set [--global] core.quotepath off
  ```
- Diff non-UTF8 files:
  ```bash
  # Suppose the .tex files are in GBK encoding
  $ cat .git/config
  [diff "gbk"]
          textconv = "iconv -f gbk -t utf-8"

  $ cat .gitattributes
  *.tex diff=gbk
  ```
- Show whitespace changes in `git diff` ([doc](https://git-scm.com/docs/git-config#Documentation/git-config.txt-diffwsErrorHighlight))
  ```bash
  $ git config set [--global] diff.wsErrorHighlight all
  ```
- Pretty one-line `git log`
  based on https://ma.ttias.be/pretty-git-log-in-one-line/
  ```bash
  # set pretty format and alias
  git config set --global pretty.logline "%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(cyan)<%an>%Creset"
  git config set --global alias.logline "log --graph --pretty=format:logline --abbrev-commit"

  # use git alias
  $ git logline
  ```

Show status

- Show individual files in untracked directories ([ref](https://git-scm.com/docs/git-status#Documentation/git-status.txt--ultmodegt))
  ```
  git status [-u | --untracked-files]
  ```

Clone and fetch

- Shallow clone: `git clone --depth=<num> [--no-single-branch]`
  `--depth` implies `--single-branch`, thus the `fetch = +refs/heads/DEFAULT_BRANCH:refs/remotes/origin/DEFAULT_BRANCH` line in `.git/config`, which makes new branches pushed to remote not auto-tracked locally
  - To overwrite `--single-branch`, run `git remote set-branches REMOTE BRANCH` or directly edit `.git/config`
  - To query all `remote.*.fetch` settings, `git -P config get --all --show-names --regexp 'remote\..*\.fetch'`
- Convert a shallow clone to full clone ([ref](https://stackoverflow.com/a/17937889))
  ```bash
  git fetch --unshallow
  ```
- Fetch a specific commit
  ```
  git fetch --depth=1 origin <commit>
  ```
- Fetch a specific tag
  ```
  git fetch --no-tags origin tag <tag>
  ```
  https://stackoverflow.com/a/54635270
  short form `-n` https://git-scm.com/docs/git-fetch#Documentation/git-fetch.txt--n
  see also Git config [`remote.<name>.tagOpt`](https://git-scm.com/docs/git-config#Documentation/git-config.txt-remoteltnamegttagOpt).

Commit changes

- Update commit author (and email)
  https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---authorltauthorgt
  ```bash
  git commit --amend --author="Author <author@example.com>"
  ```
- Batch sign-off
  [`--signoff`][git-rebase-signoff], no short form
  `git rebase --signoff <commit>`
- Reuse last commit message (after a failed attempt)
  ```shell
  # -e/--edit -F/--file
  git commit [--edit] --file .git/COMMIT_EDITMSG
  ```

Workflow

- Keep branch clean with [`--fixup`][git-commit-fixup] and [`--autosquash`][git-rebase-autosquash] ([article](https://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html))
  ```bash
  # `--fix` automatically marks your commit as a fix of a previous commit
  # The resulted commit message will be "fixup! <msg of referred commit>"
  git commit --fixup <commit>
  # `--autosquash` automatically organizes merging of  fixup commits and associated normal commits
  # see also boolean config `rebase.autoSquash`
  git rebase [-i] --autosquash <commit>
  ```

[git-rebase-signoff]: https://git-scm.com/docs/git-rebase#Documentation/git-rebase.txt---signoff
[git-commit-fixup]: https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---fixupamendrewordltcommitgt
[git-rebase-autosquash]: https://git-scm.com/docs/git-rebase#Documentation/git-rebase.txt---autosquash
[git-fetch-refspec]: https://git-scm.com/docs/git-fetch#Documentation/git-fetch.txt-ltrefspecgt

Branching and Merging

- Create a local branch that tracks a remote one ([doc][git-fetch-refspec])
  ```bash
  git fetch <remote> <branch>:<local branch>
  ```
- Delete a remote-tracking branch (the corresponding local branch is unchanged, [ref](https://stackoverflow.com/a/3046478))
  ```bash
  git branch --delete --remotes <remote>/<branch>
  ```
- Track new remote branch after a shallow clone ([ref](https://stackoverflow.com/a/27393574))
  ```bash
  # shallow clone
  git clone --depth=<num> <repository> [<directory>]

  # change the list of branches tracked
  git remote set-branches <remote> '*'
  # or
  git remote set-branches --add <remote> <new_branch>

  # add track to new remote branch
  git fetch <remote> <new_branch>
  ```
  wait for test: `git fetch --update-shallow <remote> <branch>`
- Include a commit summary in merge commit
  ```bash
  git merge --no-ff --log <branch>
  ```

Switch to a branch or commit-ish

```bash
git switch BRANCH
git switch -c/--create NEW_BRANCH [START_POINT]
git switch ---detach START_POINT
```

Back to clean working directory

```bash
git stash list
# -a/--all also takes ignored files into consideration
git stash [push] [-u | --include-untracked] [(-m | --message) <message>]
git stash pop
git stash show [--stat] [--patch] [-u | --include-untracked] [stash@{0}]
git stash drop [stash@{0}]
```

Restore files from index or some commit

```bash
# restore working tree (-W/--worktree) from HEAD
git restore [--] <pathspec>...
# restore index from HEAD
git restore --staged [--] <pathspec>...

# -S/--staged and -W/--worktree can be used in together
# -p/--patch: select hunks interactively
# -s/--source=<tree>: restore from <tree>
```

Tags

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

- Push single tag ([Q&A](https://stackoverflow.com/a/23212493))
  ```bash
  # to resolve tag/branch name clashes, use "refs/tags/<tag>"
  git push <remote> <tag>
  ```
  Differences between annotated (`-m <message>`) and unannotated tags: [this Q&A](https://stackoverflow.com/q/11514075)

Change remote

- Delete remote branch or tag
  ```bash
  git push --delete <remote> <branch/tag>
  ```

Show log

- List commits that changed a specific file ([ref](https://stackoverflow.com/a/8808453))
  ```bash
  git log --follow -- filename
  ```
- Show first commit ([ref](https://stackoverflow.com/a/5188990))
  ```bash
  git log --reverse
  ```

Large File Storage (LFS)

- homepage: https://git-lfs.github.com/
- docs: https://github.com/git-lfs/git-lfs/tree/main/docs/man
- NOTE: GitHub Pages doesn't support Git LFS. ([source](https://github.com/git-lfs/git-lfs/issues/1342#issuecomment-229965973))

```bash
# install and init
brew install git-lfs
git lfs install

# track/untrack files by extension
git lfs track "*.gif"
git lfs untrack "*.gif"
# Equivalent to add/remove line
#   *.gif filter=lfs diff=lfs merge=lfs -text
# to/from `.gitattributes`

# fetch and checkout lfs files
git lfs fetch
git lfs checkout

# retrack files (after untrack them from lfs)
# QA: https://stackoverflow.com/q/35011366/
git add --renormalize .
```

Misc

- Force or cancel pager
  https://git-scm.com/docs/git#Documentation/git.txt--p
  https://git-scm.com/docs/git#Documentation/git.txt--P
  ```bash
  git [-p | --paginate] <command>
  git [-P | --no-pager] <command>
  ```
- Clean up unlinked commits ([ref](https://stackoverflow.com/a/11759044))
  ```bash
  git reflog expire --expire=now --all
  git gc --prune=now
  ```
- Shorten local clone, just to save disk space ([ref](https://stackoverflow.com/a/37105443))
  ```bash
  git fetch --depth=1
  git reflog expire --expire-unreachable=now --all
  git gc --aggressive --prune=all
  ```
- Diff between arbitrary files:
  ```bash
  git diff --no-index <file a> <file b>
  ```
- List all (local) references (heads, remotes, stash, and tags)
  https://git-scm.com/docs/git-for-each-ref
  https://git-scm.com/docs/git-show-ref
  ```bash
  git [--paginate] for-each-ref [--format="%(refname)"]
  # or
  git [--paginate] show-ref --head --dereference
  ```
- List all commits, including unreachable ones
  https://git-scm.com/docs/git-rev-list
  ```bash
  git --paginate rev-list --all --no-commit-header --pretty=oneline
  ```
- Count the commits on current branch ([ref](https://stackoverflow.com/a/11657647))
  ```bash
  git rev-list --count HEAD
  ```
- Describe a commit even without reachable tags
  ```bash
  git describe --tags --always
  # the first reachable tag is "v3.1.2"
  # v3.1.2-21-gc6cd52d
  # no tag is reachable
  # gc6cd52d
  ```
  https://git-scm.com/docs/git-describe#Documentation/git-describe.txt---always
  saw in https://github.com/tuna/thuthesis/commit/bd0481059cf97bf9fa89638ab5cd5bc2abd27fa9, for the request raised in https://github.com/tuna/thuthesis/pull/979


<!-- ## gh -->
## GitHub CLI

- `gh api`
  https://cli.github.com/manual/gh_api
  - adding parameters (via `-f/--raw-field` or `-F/--field`) changes the default HTTP method from `GET` to `POST`
    > The default HTTP request method is `GET` normally and `POST` if any parameters were added. Override the method with `-X/--method`.
    - so `gh api /repos/muzimuzhi/hello-github-actions/issues/26/comments -f body='Send by GitHub CLI'` works but `gh api /repos/denoland/vscode_deno/commits -f per_page=2` (without `-X GET`) failed with error `gh: Not Found (HTTP 404)`
    - feature request to only switch to `POST` when body parameters were added is made in
      https://github.com/cli/cli/issues/6877#issuecomment-2406540679
- debug
  `PAGER= GH_DEBUG=1`
  https://cli.github.com/manual/gh_help_environment
  - `GH_DEBUG=api` also logs HTTP traffic


## just

- just a command runner
  https://github.com/casey/just (README contains single-page doc)
  https://just.systems/ (multi-page doc)
- cheatsheet https://cheatography.com/linux-china/cheat-sheets/justfile/
- notice
  - each recipe line is executed by a new shell
    https://github.com/casey/just?tab=readme-ov-file#setting-variables-in-a-recipe
    https://github.com/casey/just?tab=readme-ov-file#changing-the-working-directory-in-a-recipe
  - `if ... { ... } else { ... }` must has an `else` branch
- limitations
  - not enough shell-portable utilities
  - no recipe variable
  - no recursive call (run `just` in a recipe and inherit current settings and variables)


## pre-commit

- https://pre-commit.com/ (only 1st-level ToC bookmarks, pity)
  https://github.com/pre-commit/pre-commit
- usage
  ```shell
  # install
  $ uv tool install pre-commit

  # add .pre-commit-config.yaml (".yml" is NOT supported)
  # ...

  # install the ".git/hooks/pre-commit" script
  $ pre-commit install

  # run against all files (init run)
  $ pre-commit run --all-files [<hook-id>]

  # run against staged files
  # the "pre-commit" hook runs "pre-commit run"
  $ pre-commit run [--files FILE...] [<hook-id>]
  ```
- config
  - starter `.pre-commit-config.yaml`
    https://pre-commit.com/#2-add-a-pre-commit-configuration
    ```yaml
    repos:
    - repo: https://github.com/pre-commit/pre-commit-hooks
      rev: v5.0.0
      hooks:
      - id: check-case-conflict
      - id: check-illegal-windows-names
      - id: check-merge-conflict
      - id: check-toml
      - id: check-yaml
      - id: end-of-file-fixer
      - id: trailing-whitespace
    ```
  - run arbitrary commands
    ```yaml
    repos:
    - repo: local
      hooks:
      - id: just-lint-all
        name: run just lint-all recipe
        language: system
        entry: just lint-all
        pass_filenames: false
    ```
  - `pre-commit autoupdate [--freeze]` update repo versions
    https://pre-commit.com/#pre-commit-autoupdate
- caches
  - `pre-commit gc` clean unused cached repos
    https://pre-commit.com/#pre-commit-gc
  - location `~/.cache/pre-commit`
    https://pre-commit.com/#managing-ci-caches
- `git commit --no-verify` bypass the `pre-commit` hooks (and several more)
  https://git-scm.com/docs/githooks#_pre_commit
- practices
  https://github.com/muzimuzhi/latex-zutil/blob/main/.pre-commit-config.yaml
  https://github.com/crate-ci/typos/blob/master/.pre-commit-hooks.yaml


## typos

- Low false-positive source code spell checker
  https://github.com/crate-ci/typos
  https://github.com/crate-ci/typos/blob/master/docs/reference.md
- usage
  ```shell
  # run
  typos
  # auto fix (-w/--write-chagnes)
  typos -w
  ```
- config
  https://github.com/crate-ci/typos/blob/master/docs/reference.md
  - config file
    - `[._]?typos.toml` in non-Python projects
    - `typos -c/--config <CUSTOM_CONFIG>`
  - config reference
    - Rust `regex` pattern
    - examples
      - https://github.com/latex3/latex3/blob/develop/.github/typos.toml
      - [`_typos.toml` in this repo](./../_typos.toml)
- GitHub Actions integration
  https://github.com/crate-ci/typos/blob/master/docs/github-action.md
  ```yaml
  jobs:
    spelling:
      name: Spell check with Typos
      runs-on: ubuntu-latest
      # one workaround https://github.com/crate-ci/typos/pull/1192
      # already used in https://github.com/muzimuzhi/latex-zutil
      - name: Install wget
        if: runner.os == 'Windows'
        run: choco install wget --no-progress
      # needs "wget", which is missing from GH Windows runners
      - name: Spell Check Repo
        uses: crate-ci/typos@v1.33.1
  ```
