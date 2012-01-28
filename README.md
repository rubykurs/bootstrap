# Rails 3.2 Bootsrap with LESS twitter bootstrap, simpleform and heroku-ready.



## How to reproduce this app bootstrap.

    $ rails new myapp
    $ cd myapp
    $ rm public/index.html

    @ Gemfile
    -gem 'sqlite3'
    +group :development, :test do
    +  gem 'sqlite3'
    +end
    group :assets do
    -  gem 'sass-rails',   '~> 3.2.3'
    +  gem 'less-rails',   '~> 2.1.0'

    +gem 'simple_form', :git => 'git://github.com/plataformatec/simple_form.git', :ref => "9f7a7bcbb9323e91cf8b9261bf2b31c2a174b342"
    +gem 'twitter-bootstrap-rails'
    +group :production do
    +  gem 'pg'
    +  gem 'yui-compressor'
    +end

    @ application.rb
    + config.app_generators.stylesheet_engine :less
    + config.assets.initialize_on_precompile = false
    @ production.rb
    +  config.assets.css_compressor = :yui

    $ bundle
    $ rails generate
    $ rails generate bootstrap:install
    $ rails generate simple_form:install

    @ simple_form.rb
     replace with this gist: https://gist.github.com/1695634

