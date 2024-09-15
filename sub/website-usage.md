# Website Usage

## GitHub

### meta

- features
  https://github.com/features/
- changelog
  https://github.blog/changelog
- public roadmap
  https://github.com/orgs/github/projects/4247

### URL Patterns

Main site
- https://github.com/dashboard-feed old feed

Repository specific
```
# PAT ::= "commits" | "tree"
https://github.com/OWNER/REPO/PAT/REF_OR_COMMIT/path/to/dir

# PAT ::= "blob" | "blame" | "commits" | "edit" | "raw"
https://github.com/OWNER/REPO/PAT/REF_OR_COMMIT/path/to/file

# "commit"
https://github.com/OWNER/REPO/commit/SHA (shortened SHA works too)
https://github.com/OWNER/REPO/commit/SHA.diff
https://github.com/OWNER/REPO/commit/SHA.patch

# "labels"
https://github.com/OWNER/REPO/labels
https::/github.com/OWNER/REPO/labels/LABEL_NAME

# "issues"
https://github.com/OWNER/REPO/issues
https://github.com/OWNER/REPO/issues/AUTHOR     # aka "is:open is:issue author:AUTHOR"
                                                # special author "@me" supported
https://github.com/OWNER/REPO/issues/created_by/AUTHOR
https://github.com/OWNER/REPO/issues/ISSUE_NUMBER

https://github.com/OWNER/REPO/issues/new        # blank issue
https://github.com/OWNER/REPO/issues/new/choose # choose an issue template
https://github.com/pgf-tikz/pgf/issues/new?assignees=&labels=&projects=&template=ISSUE_TEMPLATE.{md,yml}
                                                # create an issue from a URL query
                                                # https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue#creating-an-issue-from-a-url-query

# "pulls" and "pull"
https://github.com/OWNER/REPO/pulls
https://github.com/OWNER/REPO/pulls/AUTHOR    # aka "is:open is:pr author:AUTHOR"
https://github.com/OWNER/REPO/pull/PR_NUMBER

# "compare"
# see https://docs.github.com/en/pull-requests/committing-changes-to-your-project/viewing-and-comparing-commits
https://github.com/OWNER/REPO/compare/SHA_BASE..SHA_HEAD
https://github.com/OWNER/REPO/compare/BASE_BRANCH...REPO_B:HEAD_BRANCH
https://github.com/OWNER/REPO/compare/BASE_BRANCH...REPO_B:HEAD_BRANCH?expand=1 # open a pr
```
where
```
REF_OR_COMMIT ::= REFERENCE | COMMIT
REFERENCE     ::= BRANCH | TAG
```
- `raw` pattern first saw in 
  https://github.com/CTeX-org/ctex-kit/issues/686#issuecomment-1827190097

<!-- ### GFM -->
### GitHub Flavored Markdown

- general [Markdown](./computer-science#markdown) notes
- spec https://github.github.com/gfm/
- related Github docs https://docs.github.com/en/get-started/writing-on-github

Links

```md
<!-- full-reference-link -->
[my text][my-label] <!-- link-text == "my text", link-label == "my-label" -->
<!-- shortcut-reference-link -->
[my-label]          <!-- link-text == link-label == "my-label" -->

<!-- link reference definition -->
[my-label]: /my-url
```

- GFM Spec
  - link-reference-definition
    `[link-label]: link-destination "link-title"`
    https://github.github.com/gfm/#link-reference-definitions
      - the concept `link-text`, `link-label`, and `link-title` defined in GFM Spec contain the corresponding delimiters like `[...]` or `"..."`
  - link
    https://github.github.com/gfm/#links
    - components: link-text, link-destination, and link-title
      https://github.github.com/gfm/#link-text
      https://github.github.com/gfm/#link-destination
      https://github.github.com/gfm/#link-title
      - link-title is optional
    - classification
      tip: link-text is always taken from the first `[...]`
      - inline-link
        `[link-text](link-destination "link-title")`
        - link-title can alternatively be delimited by `'...'` and `(...)`
        https://github.github.com/gfm/#inline-link
      - reference-link
        https://github.github.com/gfm/#reference-link
        - full-reference-link
          `[link-text][link-label]`
          https://github.github.com/gfm/#full-reference-link
        - collapsed-reference-link
          `[link-label][]`, equivalent to full-reference-link `[link-label][link-label]`
          https://github.github.com/gfm/#collapsed-reference-link
        - shortcut-reference-link
          `[link-label]`, equivalent to collapsed-reference-link `[link-label][]`
          https://github.github.com/gfm/#shortcut-reference-link

- Confusing description in 0.29-gfm
  - phrase "first link label" in definitions of concepts "collapsed reference link" and "shortcut reference link"
    - > A collapsed reference link consists of a link label that matches a link reference definition elsewhere in the document, followed by the string `[]`. The contents of the **first link label** are parsed as inlines, which are used as the link’s text.
      https://github.github.com/gfm/#collapsed-reference-link
    - > A shortcut reference link consists of a link label that matches a link reference definition elsewhere in the document and is not followed by `[]` or a link label. The contents of the **first link label** are parsed as inlines, which are used as the link’s text.
      https://github.github.com/gfm/#shortcut-reference-link
    - Q: What does "first link label" here mean?
    - Can be traced back to commonmark spec. 
      https://github.com/commonmark/commonmark-spec/issues/746
    - On 2023-10-18, CommonMark has removed the two occurrences of word "first". Waiting for GFM to catch up.

Special Usages

- Using `<span>` tag to disable auto-linking
  https://gist.github.com/alexpeattie/4729247
- Using `<details>` and `<summary>` tags to create collapsed sections
  - shortcut: typing `/details`, a GitHub slash command
    ```html
    <details><summary>Details</summary>
    <p>

    |cursor position

    </p>
    </details> 
    ```
    https://github.blog/changelog/2023-03-15-introducing-the-github-markdown-helpers-public-beta/
    https://docs.github.com/en/issues/tracking-your-work-with-issues/about-slash-commands
  - example
    ```html
    <details>
      <summary>Details about <code>\x</code></summary>

      markdown content normally parsed

    </details>
    ```
    https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/organizing-information-with-collapsed-sections
  - notes
    - `<details>` starts a three-line html block which is treated as raw html, and can only be ended by a blank line. Thus the content of `<summary>` element is not parsed as markdown.
      https://github.github.com/gfm/#html-blocks
    - To enable parsing, `<summary>` open tag must be followed by a blank line. But this will make the `<summary>` content a block element (surrounded by `<p>`). It will look like `▸<newline>Details about ...`.
      https://github.github.com/gfm/#paragraphs
    - A CSS solution is to set `details summary > * { display: inline; }`, but it seems GitHub blocks user-specified re-styling. Not only `<style>` element, but `style` attribute like `<summary style="color: red;">` are blocked.
      https://css-tricks.com/two-issues-styling-the-details-element-and-how-to-solve-them/#aa-inline-all-the-things
      https://github.github.com/gfm/#disallowed-raw-html-extension-
  - MDN docs
    https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details
    https://developer.mozilla.org/en-US/docs/Web/HTML/Element/summary

### Searching on GitHub

Filter issues and pull requests
https://docs.github.com/en/search-github/searching-on-github/searching-issues-and-pull-requests

```bash
author:@me
mentions:USERNAME  # note the third person singular "s"
commenter:USERNAME
head:HEAD_BRANCH
```

All my subscriptions
- https://github.com/notifications/subscriptions

### Pull Requests

Check out a PR locally
- with local branch created
  `git fetch origin pull/<pr-id>/head:<branch-name> && git switch <branch-name>`
- without local branch created
  `git fetch origin pull/<pr-id>/head && git checkout FETCH_HEAD`
  https://stackoverflow.com/a/45967995
- using the GitHub CLI
  `gh pr checkout <pr-id>`
  This may fail in a shallow clone, see https://github.com/cli/cli/issues/4287
- docs (not very helpful)
  - "[Checking out pull requests locally][checkout-pr-branch]"
  - "[Committing changes to a pull request branch created from a fork][commit-to-pr-branch]".

[checkout-pr-branch]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/checking-out-pull-requests-locally
[commit-to-pr-branch]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/committing-changes-to-a-pull-request-branch-created-from-a-fork

### Labels

- labels in a repo
  https://github.com/OWNER/REPO/labels
  or `gh label list --repo OWNER/REPO`
  https://cli.github.com/manual/gh_label_list

### GitHub Pages

Abstractions
- workflow run "pages build and deployment"
  demo in https://github.com/actions/jekyll-build-pages
  - job "build"
    - uses action image `ghcr.io/actions/jekyll-build-pages:vx.y.z` packed by https://github.com/actions/jekyll-build-pages
      depends on `gem "github-pages", "= VERSION"`
      https://github.com/actions/jekyll-build-pages/blob/main/Gemfile
      - gem package`github-pages`
        https://github.com/github/pages-gem
        depends on `"jekyll" => "x.y.z",`
        https://github.com/github/pages-gem/blob/master/lib/github-pages/dependencies.rb
  - job "report-build-status" which needs "build"
  - job "deploy" which need "build"

Dependency versions
  - https://pages.github.com/versions/
  - Till 2023-11-23, Jekyll 3.9.3, rather than 4.x is used.
    > `"jekyll" => "3.9.3",`
    https://github.com/github/pages-gem/blob/v228/lib/github-pages/dependencies.rb

Build site locally
```bash
# install prerequisitions
gem install bundler

# create "Gemfile", if it doesn't exist
echo "gem 'github-pages', group: :jekyll_plugins" > Gemfile
# to use the same version as GitHub Pages
# echo "gem 'github-pages', \"~> VERSION\", group: :jekyll_plugins" > Gemfile

# workaround for "webrick"
bundle add webrick

# install (ruby packages), build, and serve
bundle install
bundle exec jekyll serve
```

Docs and useful links
 - [Building your site locally](https://docs.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll#building-your-site-locally) - GitHub Docs (outdated, Jan 20 2021)
 - [Dependency gem versions](https://pages.github.com/versions/)

About the `webrick` workaround
 - `jekyll` depends on `webrick`, but the latter one is [no longer a bundled gem in Ruby 3.0](https://www.ruby-lang.org/en/news/2020/12/25/ruby-3-0-0-released/).
 - Once `jekyll` releases a new version (most likely [v4.3](https://github.com/jekyll/jekyll/milestone/72?closed=1)) including the [PR resolving this](https://github.com/jekyll/jekyll/pull/8524), and gem `gihub-pages` uses that newer `jekyll`, then the workaround is not needed.

### GitHub Actions

- meta
  - general
    https://github.com/features/#features-automation
    https://github.com/features/actions
  - docs
    https://docs.github.com/actions
  - marketplace of actions
    https://github.com/marketplace?type=actions
  - changelog
    https://github.blog/changelog/label/actions/
  - repos
    https://github.com/actions/runner
    https://github.com/actions/runner-images
    - GitHub-hosted runners, their YAML labels and included softwares

- workflow
  a `.github/workflows/filename.(yml|yaml)` file
  - structure
    - each workflow contains one or more jobs
    - each job either contains one or more steps (`jobs.<job_id>.steps`) or uses another (reusable) workflow (`jobs.<job_id>.uses`)
      see https://github.com/github/vscode-github-actions/issues/291
    - each step either uses an action (`jobs.<job_id>.steps[*].uses`) or runs some code (`jobs.<job_id>.steps[*].run`)
  - actions used by steps
    - `uses: actions/checkout@main`
    - such actions are one of "Docker container action", "JavaScript action", and "composite action"
  - reusable workflows used by jobs
    - `jobs.<job_id>.uses: {owner}/{repo}/.github/workflows/{filename}@{ref}`
    - reusable workflows must have the `on.workflow_call` event
      https://docs.github.com/en/actions/sharing-automations/reusing-workflows
  - composite actions vs reusable workflows
    https://docs.github.com/en/actions/sharing-automations/avoiding-duplication#comparison-of-reusable-workflows-and-composite-actions

- workflow syntax
  https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions
  - glob pattern `*` doesn't match `/`, use `**` instead
    https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet
    - misuse: using `on.push.branches: ["*"]` to accept all branches, but actually branches containing `/` are filtered out
      https://github.com/latex3/latex3/pull/1293
  - `on.(push|pull_request).(paths|paths_ignore)` should match a (relative) path to file, so `paths: dir/**` works but `paths: dir` doesn't
    https://github.com/muzimuzhi/hello-github-actions/commit/c84ebdc31cc0af60d02ed74ceffb3a069ee772e1

- variables
  - default environment variables
    https://docs.github.com/en/actions/learn-github-actions/variables#default-environment-variables
    > Because default environment variables are set by GitHub and not defined in a workflow, they are not accessible through the `env` context.
    ```yaml
    - run |
        echo 'github.workspace === ${{ github.workspace }}' # ok
        echo "GITHUB_WORKSPACE === $GITHUB_WORKSPACE" # ok
        echo 'env.GITHUB_WORKSPACE === ${{ env.GITHUB_WORKSPACE }}' # wrong
    ```
  - use env var as boolean
    ```yaml
    if: fromJSON(env.MY_VAR) # all truthy values are accepted
    if: env.MY_VAR == 'true' # suppose MY_VAR is either 'true' or 'false'
    ```
    - Background: using env var used in conditionals along
      - if non-existent or empty -> falsy value `""` (string) -> `false` (boolean)
      - otherwise -> truthy value (string) -> `true` (boolean)
      - So with `env.MY_ENV_VAR: false`, `if: $MY_ENV_VAR` evaluates to true

- expressions
  - if-else (ternary operator) in expressions
    `${{ x && 'ifTrue' || 'ifFalse' }}`
    > the first value after the `&&` must be truthy
    https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/evaluate-expressions-in-workflows-and-actions#operators
    - first saw in https://github.com/latex3/latex2e/blob/f7ccd3168fd1fee22d6bf574bd96876124b9ef6b/.github/workflows/main.yaml#L122C39-L122C99
      common reference https://github.com/actions/runner/issues/409#issuecomment-752775072

- run logs
  - `working-directory` is never logged, even when debug logging is enabled
    https://docs.github.com/actions/monitoring-and-troubleshooting-workflows/enabling-debug-logging
    - can be set by one or more of `defaults.run.working-directory`, `jobs.<job_id>.defaults.run.working-directory`, and `jobs.<job_id>.steps[*].working-directory`
    - only `shell` and `env` are always logged for all steps. Quite some other step properties are similarly hidden in logs
    - Sep 15, 2024: feedback submitted https://github.com/orgs/community/discussions/138657

- frequently used actions
  - `actions/checkout`
    https://github.com/actions/checkout
    ```yaml
    - name: Checkout
      uses: actions/checkout@v<n>
      with:
        # when possible, default values are listed
        repository: ${{ github.repository }}
        ref: "${{ github.ref }} or the HEAD of default branch"
        path: .               # relative path under ${{ github.workspace }}
        fetch-depth: 1        # 0 indicates "all history for all branches and tags"
        fetch-tags: false
        lfs: false
        submodules: false
        # more sophisticated inputs
        filter: ''            # see `git clone --filter=<filter-spec>`
                              # https://git-scm.com/docs/git-clone#Documentation/git-clone.txt-code--filtercodeemltfilter-specgtem
        sparse-checkout: null # see `git sparse-checkout`
                              # https://git-scm.com/docs/git-sparse-checkout
    ```
    - by default checks out `${{ github.repository }}` to `${{ github.workspace }}`
      - configurable via `repository` and `path` (relative path, sub-dir only) inputs
    - in most cases the `path` directory is erased before checkout
      - for example, when `path` doesn't contain a pre-existing `.git` dir
        https://github.com/actions/checkout/pull/561
      - job log `Deleting the contents of '/home/runner/work/REPO/REPO'`
    - default setting `set-safe-directory: true` doesn't work for containers
      - workaround: `git config set --global safe.directory "*"` (too loose?)
      - https://github.com/actions/runner/issues/2033 "git config safe.directory inside docker containers"
        https://github.com/actions/checkout/issues/766 "fatal: unsafe repository (REPO is owned by someone else) in other workflow steps after running checkout"
        https://github.com/actions/checkout/issues/1169 "/github/home/.gitconfig does not exist for container runs"

- tools
  - vscode extensions (not actively maintained)
    - https://github.com/github/vscode-github-actions
      https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions "GitHub Actions"
      it's linter is not as comprehensive and updated as others'
      - important dependency https://github.com/actions/languageservices
    - https://github.com/redhat-developer/vscode-yaml
      https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml "YAML"
      uses JSON schemas for workflow and action files from https://www.schemastore.org/json/
  - linter
    - https://github.com/rhysd/actionlint
      playground https://rhysd.github.io/actionlint/

### Design System

- https://primer.style/
  https://github.com/primer/design
