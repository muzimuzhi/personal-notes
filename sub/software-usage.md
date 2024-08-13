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

- Print pathnames in Unicode, other than octal UTF-8 ([ref](https://stackoverflow.com/a/22828826))
  ```bash
  git config [--global] core.quotepath off
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
  $ git config [--global] diff.wsErrorHighlight all
  ```
- Pretty one-line `git log`
  based on https://ma.ttias.be/pretty-git-log-in-one-line/
  ```bash
  # set pretty format and alias
  git config --global pretty.logline "%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(cyan)<%an>%Creset"
  git config --global alias.logline "log --graph --pretty=format:logline --abbrev-commit"

  # use git alias
  $ git logline
  ```

Show status

- Show individual files in untracked directories ([ref](https://git-scm.com/docs/git-status#Documentation/git-status.txt--ultmodegt))
  ```
  git status [-u | --untracked-files]
  ```

Clone and fetch

- Shallow clone: `git clone --depth=<num>`
- Convert a shallow clone to full clone ([ref](https://stackoverflow.com/a/17937889))
  ```bash
  git fetch --unshallow
  ```
- Fetch a specific commit
  ```
  git fetch --depth=1 origin <commit>
  ```

Commit changes

- Update commit author (and email)
  https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---authorltauthorgt
  ```bash
  git commit --amend --author="Author <author@example.com>"
  ```
- Batch sign-off
  [`--signoff`][git-rebase-signoff], no short form
  `git rebase --signoff <commit>`

Workflow

- Keep branch clean with [`--fixup`][git-commit-fixup] and [`--autosquash`][git-rebase-autosquash] ([article](https://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html))
  ```bash
  # `--fix` automatically marks your commit as a fix of a previous commit
  # The resulted commit message will be "fixup! <msg of referred commit>"
  git commit --fixup <commit>
  # `--autosquash` automatically organizes merging of  fixup commits and associated normal commits
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
git stash [push] [-u | --include-untracked] [-m | --message <message>]
git stash pop 
git stash show [--stat] [--patch] [-u | --include-untracked] [stash@{0}]
git stash drop [stash@{0}]
```

Restore files from index or some commit

```bash
# restore from HEAD
git restore <pathspec>...
# restore from index
git restore --staged <pathspec>...
# restore from <tree>
git restore --source=<tree> [--] <pathspec>...

# -p/--patch: select hunks interactively
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
