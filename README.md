# Rails 3.2 Bootsrap with LESS twitter bootstrap, simple_form and guides, all heroku-ready.

*Getting started*

    $ git clone git://github.com/rubykurs/bootstrap.git
    $ git remote -v
    $ git remote rename origin bootstrap-origin
    $ git remote add origin git@github.com:myaccount/myapp.git #REPLACE account

*What does this bootstrap have?*

 + twitter_bootstrap gem
 + simple_form + setup for twitter bootstrap
 + config to use LESS and make assets precompile work at heroku
 + guide for next steps (below)
 + guide to build this bootstrap from scratch

*What does it not have?*

 + RSpec
 + Cucumber
 + fabricator
 + HAML or other template language (use ERB)
 + no custom migrations, controllers, models or views. This is your job
 + devise - no authentication or authorization (check out branch 'auth' for implementation)

This bootstrap is made by rubykurs.no for a rails class in Norway. Given very short time, we'd like the focus should be on ruby and rails. Real testing and BDD is extremely important, but it all takes time. The guide will surely get outdated soon, any help is appreciated (fork it!)


**Million-dollar tip, use pry instead of irb:**

*Makes sure you run PRY with rails console*

    @ ~/.irbrc
    require 'rubygems' unless defined? Gem # rubygems is only needed in 1.8
    require 'pry'
    Pry.start
    exit

## Next steps.

*Generate frontpage*

The bootstrap-app is also a stripped app. Nothing to show. Generate a **frontpage**

    $ rails generate controller home index
    @ routes.rb
    - get "home/index"
    + root :to => 'home#index'

delete generated helpers and tests, they are empty useless files.

*Use Twitter Bootstrap example for frontpage*

Change the

 * app/views/home/index.html.erb
 * app/views/layouts/application.html.erb
 * app/assets/stylesheets/application.css
 * adding app/views/common/_flashes.html.erb

as follows: https://gist.github.com/1698584

See the http://twitter.github.com/bootstrap/ for more examples on usages

BRANCH **homepage** at github.com/rubykurs/bootstrap has it implemented.

*Using simple form*

    rails generate scaffold report title:string description:text email:string
    rake db:migrate

Compare and change app/veiws/reports/_form.html.erb to
https://gist.github.com/1698732


**Authentication**

*Option 1: Really basic Basic Authentication*

    @reports_controller.rb
     before_filter :require_admin, :except => ['new']
    private
     def require_admin
       authenticate_or_request_with_http_basic do |id, password|
         [id, password] == ["ruby", "rocks"]
       end
     end

*Option 2: Roll your own* http://railscasts.com/episodes/250-authentication-from-scratch

*Option 3: Use popular framework like devise*

    @ Gemfile
    + gem 'devise'
    $ bundle
    $ rails generate #see devise
    $ rails generate devise:install
    # check the instructions. in the above steps, 2., 3. and 4. have already been done. Do 1.

In this example, we create admin user. Later you could create User as well, if your app needs user accounts.

    $ rails g devise admin
    $ rake db:migrate

Enable logout buttons to frontpage: See this gist: https://gist.github.com/1698995



## Deploy to heroku

Make sure root :to points to *something* (see STEPS above) so you can verify the deploy

    $ heroku create myapp --stack cedar
    $ git push heroku master
    @ http://myapp.herokuapp.com  #should see rails error


## How to reproduce this app-bootstrap.

Look, no magic, just tedious tasks:

    $ rails new myapp  #rails 3.2 and ruby 1.9.3 in this tutorial
    $ cd myapp
    $ rm public/index.html
    $ git init && git add . && git commit -am "init"

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
    @ app/assets/stylesheets/application.rb
      MOVE *= require twitter/bootstrap above *=require_self. Else any changes in application.css will be overriden.
    @ app/assets/stylesheets/bootstrap.css.less
    - @import "twitter/bootstrap";  REMOVE - it reloads and overrides your changes.
    $ rails generate simple_form:install

    @ simple_form.rb
     replace with this gist: https://gist.github.com/1695634

    $ git commit -am "bootstrapped"
    $ git remote add origin git@github.com:myaccount/myapp.git #REPLACE
    $ git push -u origin master

Then follow the **next steps** sections above for home page and devise
