# Website Usage

## GitHub

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

Confusing description in 0.29-gfm

- phrase "first link label" in definitions of concepts "collapsed reference link" and "shortcut reference link"
  - > A collapsed reference link consists of a link label that matches a link reference definition elsewhere in the document, followed by the string `[]`. The contents of the **first link label** are parsed as inlines, which are used as the link’s text.
    https://github.github.com/gfm/#collapsed-reference-link
  - > A shortcut reference link consists of a link label that matches a link reference definition elsewhere in the document and is not followed by `[]` or a link label. The contents of the **first link label** are parsed as inlines, which are used as the link’s text.
    https://github.github.com/gfm/#shortcut-reference-link
  - Q: What does "first link label" here mean?
  - Can be traced back to commonmark spec. 
    https://github.com/commonmark/commonmark-spec/issues/746
  - On 2023-10-18, CommonMark has removed the two occurrences of word "first". Waiting for GFM to catch up.

### GitHub Searching

Filter issues and pull requests ([full doc][github search issues])

```bash
mentions:USERNAME  # note the third person singular "s"
commenter:USERNAME
```

[github search issues]: https://docs.github.com/en/search-github/searching-on-github/searching-issues-and-pull-requests

All my subscriptions
- https://github.com/notifications/subscriptions

### GitHub Pull Requests

Check out a PR locally
- with local branch created
  `git fetch origin pull/<pr-id>/head:<branch-name> && git checkout <branch-name>`
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

### GitHub Pages

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

