# frozen_string_literal: true

source 'https://rubygems.org'

git_source(:github) { |repo_name| "https://github.com/#{repo_name}" }

# Where relevant, Ruby and gem version are specified to match the current
# dependency version of GitHub Pages. These can be updated as necessary by
# reference to https://pages.github.com/versions/.

ruby '3.3.4'

group :jekyll do
  gem 'github-pages', group: :jekyll_plugins
  gem 'minima', '~> 2.5.1'
end

group :jekyll_plugins do
  gem 'jekyll-feed'
  gem 'jekyll-sitemap'
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
install_if -> { RUBY_PLATFORM =~ /mingw|mswin|java/ } do
  gem 'tzinfo', '~> 1.2'
  gem 'tzinfo-data'
end

# Performance-booster for watching directories on Windows
gem 'wdm', '~> 0.1.0', install_if: Gem.win_platform?

group :test do
  gem 'rspec', '>= 3.13.0', require: false
  gem 'simplecov', '>= 0.22.0', require: false
  gem 'simplecov-console', '>= 0.9.3', require: false
end

group :linters do
  gem 'rubocop', '>= 1.75.1', require: false
  gem 'rubocop-rspec', '>= 3.5.0', require: false
end
