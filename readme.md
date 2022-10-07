# Development

## Install Ruby

Install [Ruby](https://www.ruby-lang.org/en/) with [rbenv](https://github.com/rbenv/rbenv)

DO NOT use the default ruby of MacOS which located in `/usr/bin/ruby` because it is required [ownership permissions](https://gist.github.com/KetchCyork/ab896f1c8e3e845b8d17d3aef169b871)

## Preparing Ruby local version
```
rbenv init
```

Check which ruby you are using with `which ruby`
## Run these commands to start development:

```
bundle update
bundle exec jekyll serve
```

## Notice

DO NOT change `categories` of `posts` as Category is included in URL.

Sample: http://localhost:4000/reactjs/2021/08/13/nx-react-typescript-webworker.html
