source "https://rubygems.org"

# Pin Jekyll to the version shipped in the jekyll/jekyll:latest image so
# `bundle install` reuses the preinstalled gems instead of resolving to an
# ancient combination (jekyll-archives is otherwise unbounded and drags in
# jekyll 2.x → pygments.rb → posix-spawn, which fails to compile).
gem "jekyll", "~> 4.4"
gem "webrick", "~> 1.9"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-sitemap"
  gem "jekyll-seo-tag"
  # Use classic jekyll-paginate (v1): it matches the legacy `paginate:` config
  # already in _config.yml and is compatible with Jekyll 4. (jekyll-paginate-v2
  # 3.x dropped legacy-config support, and its 2.x line only supports Jekyll 3.)
  gem "jekyll-paginate"
  gem "jekyll-archives"
end

gem "wdm", ">= 0.1.0" if Gem.win_platform?
