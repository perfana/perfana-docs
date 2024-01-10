# Perfana Docs

## Local testing

To run locally, first edit the `_config.yml` file to use `theme:` instead of `remote_theme`:

```yml
#remote_theme: pmarsceill/just-the-docs
theme: "just-the-docs"
```

When done, put back the remote_theme before commit! 
This is to have it working correctly in github docs.

To run locally, use bundle: 

```sh
bundle exec jekyll serve
```

# for mac

Follow these steps to use specific Ruby version

```sh
brew install rbenv
eval "$(rbenv init -)"
rbenv install 2.7.8
rbenv global 2.7.8
gem install jekyll
```

