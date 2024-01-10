# Perfana Docs

## Local env
1. install ruby 2.7.8; use a ruby virtual environment such as ``chruby`` or ``rbenv`` and a ruby installer such as ``ruby-install`` or ``ruby-build``
   Note: don't use the Mac system version of Ruby in ``/usr/bin/ruby`` or `/usr/local/bin/ruby`
2. In the root of the project: ``gem install bundler:2.4.22``
3. Next: `bundler install`. Just like `npm install` this will install all the project's dependencies such as *jekyll*.  

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

