source "https://rubygems.org"

# Jekyll 4 directly — no more `github-pages` metagem locked to ancient versions.
# The site is built and deployed by GitHub Actions (see .github/workflows/pages.yml),
# which uses Ruby 3.3 and the same Jekyll version pinned here.
gem "jekyll", "~> 4.3"

# Plugins. These are also listed in _config.yml so Jekyll loads them.
group :jekyll_plugins do
  gem "jekyll-feed",      "~> 0.17"
  gem "jekyll-seo-tag",   "~> 2.8"
  gem "jekyll-sitemap",   "~> 1.4"
  gem "jekyll-paginate",  "~> 1.1"
end

# Stdlib gems unbundled in Ruby 3.4+. Harmless on older Ruby; needed on 3.4 / 4.0+
# so plugins like jekyll-feed don't blow up on `require 'csv'` etc.
gem "csv"
gem "base64"
gem "bigdecimal"
gem "logger"
gem "mutex_m"
gem "ostruct"

# WEBrick was removed from Ruby's stdlib in 3.0. `jekyll serve` needs it.
gem "webrick", "~> 1.8"

# Windows / JRuby compatibility shims.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
