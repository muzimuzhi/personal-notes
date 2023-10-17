# Website Usage

## GitHub

Searching issues and pull requests ([full doc][github search issues])

```bash
mentions:USERNAME  # note the third person singular "s"
commenter:USERNAME
```

[github search issues]: https://docs.github.com/en/github/searching-for-information-on-github/searching-issues-and-pull-requests

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

