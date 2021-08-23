# frozen_string_literal: true

source 'https://rubygems.org'

git_source(:github) { |repo_name| "https://github.com/#{repo_name}" }

ruby '2.7.3' # This matches the current dependency version of GitHub Pages.

group :jekyll do
  # gem "jekyll", "~> 3.9.0"
  gem 'minima', '~> 2.5'
  gem 'webrick', '~> 1.7'
end

group :jekyll_plugins do
  gem 'github-pages', '~> 217'
  gem 'jekyll-feed', '~> 0.15.1'
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem 'tzinfo', '~> 1.2'
  gem 'tzinfo-data'
end

# Performance-booster for watching directories on Windows
gem 'wdm', '~> 0.1.1', platforms: %i[mingw x64_mingw mswin]

group :test do
  gem 'rspec', require: false
  gem 'rubocop', '~> 1.19.0', require: false
  gem 'rubocop-rspec', '~> 2.4.0', require: false
  gem 'simplecov', require: false
  gem 'simplecov-console', require: false
end
