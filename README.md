![Ruby Version](https://img.shields.io/badge/ruby-2.7.3-red?logo=ruby)
![License](https://img.shields.io/github/license/JoshSinyor/JoshSinyor.github.io)
![Code Size](https://img.shields.io/github/languages/code-size/JoshSinyor/JoshSinyor.github.io)
![Ruby Style Guide](https://img.shields.io/badge/code_style-rubocop-brightgreen?&logo=data:image/svg%2bxml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAzMiAzMiI+PGRlZnMvPjxwYXRoIGQ9Ik0yNyAxNHYtMWEyIDIgMCAwMC0yLTJIN2EyIDIgMCAwMC0yIDJ2MWEyIDIgMCAwMC0xIDF2MmExIDEgMCAwMDEgMnYxYTIgMiAwIDAwMiAxaDE4YTIgMiAwIDAwMi0xdi0xYTIgMiAwIDAwMS0ydi0yYTIgMiAwIDAwLTEtMXpNMTYgMmExMCAxMCAwIDAwLTEwIDloMjBhMTAgMTAgMCAwMC0xMC05ek0xMyAyNGg2djFsMS0xLTEtMWgtN3YybDEtMXoiLz48cGF0aCBmaWxsPSIjZWMxYzI0IiBkPSJNMjQgMThIOGExIDEgMCAxMTAtM2gxNmExIDEgMCAwMTEgMSAxIDEgMCAwMS0xIDJ6Ii8+PHBhdGggZD0iTTIzIDIydjRhMiAyIDAgMDEtMiAyaC0xYTEgMSAwIDAxMC0xbC0yLTFhMSAxIDAgMDAwLTFoLTRhMSAxIDAgMDAwIDFsLTIgMWExIDEgMCAwMTAgMWgtMWEyIDIgMCAwMS0yLTJ2LTRIN3Y0YTQgNCAwIDAwNCA0aDEwYTQgNCAwIDAwNC00di00eiIvPjwvc3ZnPg==)

# JoshSinyor.github.io

This repository functions as the back-end of my GitHub Pages webpage, [The Punch Card](https://joshsinyor.github.io/).

---

## Table of Contents

- [Installation](#installation)
- [Built With](#built-with)
- [Dependencies](#dependencies)
- [Author(s)](#author-s-)
- [License(s)](#license-s-)
- [Acknowledgements](#acknowledgements)

---

## Installation

This repository is automatically processed by GitHub Pages using Jekyll. If you would like to run a copy of the site locally, fork this repository to your local machine. Verify that you have the variant of Ruby specified in the `Gemfile` installed. In the repository directory, run `gem install bundler`, then `bundle update`. Some of the gems this repository uses are frequently updated, so running `bundle update` frequently is advised.

To start the local hosting server, run `jekyll serve`, and visit the site at http://localhost:4000/.

---

## Built With

This repository is based on the guidelines provided in [How to Build A SQL Blog](https://chadbaldwin.net/2021/03/14/how-to-build-a-sql-blog.html), courtesy of [Chad Baldwin](https://github.com/chadbaldwin).

- [simple-blog-bootstrap](https://github.com/chadbaldwin/simple-blog-bootstrap/), courtesy of [Chad Baldwin](https://github.com/chadbaldwin).

---

## Dependencies

- [Ruby](https://www.ruby-lang.org/), courtesy of [Yukihiro Matsumoto](https://github.com/matz).

GitHub pages uses Jekyll to translate the contents of this repository into a static website rendered in HTML and CSS. In a sense these are the repository's only true dependencies. However, local build and testing of the repository requires the installation of a local version of Jekyll and GitHub-Pages.

- [Jekyll](https://jekyllrb.com/), courtesy of [Ashwin Maroli](https://github.com/ashmaroli), [Frank Taillandier](https://github.com/DirtyF) and [Matt Rogers](https://github.com/mattr-).
- [github-pages](https://github.com/github/pages-gem), courtesy of [Ben Balter](https://github.com/benbalter) and [Parker Moore](https://github.com/parkr).

This program's other dependencies are minimal and relate solely to testing. They were chosen for their ubiquity and self-contained nature, so that they could be specified as `require: false` and required only in the `test` environment to reduce their impact on the program's speed. For clarity, all dependencies are explicitly invoked by the `Gemfile`.

- [RSpec](https://rspec.info/), courtesy of [Jon Rowe](https://github.com/JonRowe), [Benoit Tigeot](https://github.com/benoittgt), [Phil Pirozhkov](https://github.com/pirj), [Xavier Shay](https://github.com/xaviershay) and [Yuji Nakayama](https://github.com/yujinakayama).
- [Rubocop](https://rubocop.org/) and [Rubocop-RSpec](https://github.com/rubocop/rubocop-rspec), both courtesy of [Bozhidar Batsov](https://github.com/bbatsov).
- [SimpleCov](https://github.com/simplecov-ruby/simplecov), courtesy of [Christoph Olszowka](https://github.com/colszowka).
- [SimpleCov-Console](https://github.com/chetan/simplecov-console), courtesy of [Chetan Sarva](https://github.com/chetan).

---

## Author(s)

Authored by [Joshua Sinyor](https://gist.github.com/JoshSinyor).

---

## License(s)

This project is licensed under the [MIT License](LICENSE).

---

## Acknowledgements

- Table of contents generated with [markdown-toc](http://ecotrust-canada.github.io/markdown-toc/).
