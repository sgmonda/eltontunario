source "https://rubygems.org"

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
gem "jekyll", "~> 3.9.0"

# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "github-pages", group: :jekyll_plugins

# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.0", :install_if => Gem.win_platform?

# kramdown v2 ships without the gfm parser by default. If you're using
# kramdown v1, comment out this line.
gem "kramdown-parser-gfm"

# Necesarios en Ruby 3.4+
gem "csv", "~> 3.3"
gem "logger", "~> 1.6"

# Jekyll 3.x en Ruby 3 necesita webrick para `jekyll serve`
gem "webrick", "~> 1.8"

# Ruby 3.4+ ya no trae estos por defecto
gem "base64", "~> 0.2"

# stdlib que ya no es default en Ruby 3.4+
gem "bigdecimal", "~> 3.1"

gem "liquid", ">= 4.0.4", "< 5.0"
