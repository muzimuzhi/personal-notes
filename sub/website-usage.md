# Website Usage

## GitHub

Searching issues and pull requests ([full doc][github search issues])

```bash
mentions:USERNAME  # note the third person singular "s"
commenter:USERNAME
```

[github search issues]: https://docs.github.com/en/github/searching-for-information-on-github/searching-issues-and-pull-requests

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

