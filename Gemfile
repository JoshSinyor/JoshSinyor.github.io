# frozen_string_literal: true

source 'https://rubygems.org'

git_source(:github) { |repo_name| "https://github.com/#{repo_name}" }

# Where relevant, Ruby and gem version are specified to match the current dependency version of GitHub Pages. These can be updated as necessary by reference to https://pages.github.com/versions/.

ruby '2.7.3'

group :jekyll do
  gem 'github-pages', group: :jekyll_plugins
  gem 'minima', '~> 2.5'
end

group :jekyll_plugins do
  gem 'jekyll-feed'
  gem 'jekyll-sitemap'
end

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
# gem "kramdown-parser-gfm"

group :test do
  gem 'rspec', '>= 3.10.0', require: false
  gem 'rubocop', '>= 1.19.1', require: false
  gem 'rubocop-rspec', '>= 2.4.0', require: false
  gem 'simplecov', '>= 0.21.2', require: false
  gem 'simplecov-console', '>= 0.9.1', require: false
end
