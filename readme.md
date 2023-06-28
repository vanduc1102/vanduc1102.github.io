# Development

## Install Ruby

Install [Ruby](https://www.ruby-lang.org/en/) with [rbenv](https://github.com/rbenv/rbenv)

DO NOT use the default ruby of MacOS which located in `/usr/bin/ruby` because it is required [ownership permissions](https://gist.github.com/KetchCyork/ab896f1c8e3e845b8d17d3aef169b871)


```bash
rbenv install 2.7.2
rbenv local 2.7.2
eval "$(rbenv init - zsh)"
gem install bundler
```

## Preparing Ruby local version

```bash
rbenv init
```

Check which ruby you are using with `which ruby`

Expected output:

```bash
$HOME/.rbenv/shims/ruby
```

Else try this command if you are using ZSH:

```bash
eval "$(rbenv init - zsh)"
```

## Run these commands to start development:

```bash
bundle update
bundle exec jekyll serve
```

Build for production

```bash
JEKYLL_ENV=production bundle exec jekyll build
```

## Notice

DO NOT change `categories` of `posts` as Category is included in URL.

Sample: http://localhost:4000/reactjs/2021/08/13/nx-react-typescript-webworker.html
