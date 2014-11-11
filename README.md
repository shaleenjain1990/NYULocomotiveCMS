Locomotive CMS
================

Install ImageMagick
-------------------
Wagon requires ImageMagick, so if you don't already have this, you will need to install it.
```
sudo apt-get install imagemagick
```
Install Wagon
-------------
Locomotive CMS requires gem Wagon
```
gem install locomotivecms_wagon
```
Once it finishes, check that wagon installed correctly by running the wagon version command.
```
wagon version
#1.5.0
```
Install Mongodb
---------------
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
sudo apt-get update
sudo apt-get install mongodb-10gen
```

To start mongodb
```
sudo start mongodb
```
To stop mongodb
```
sudo stop mingodb
```

Installing Rails 3.2.19
-----------------------
If you have a newer version of rails or older version of rails.
You need to update to Rails 3.2.19 because Locomotive CMS only works with Rails 3.2.19

To uninstall any previous version of rails and install required version
```
gem uninstall rails
gem uninstall railties
gem install rails -v 3.2.19
```
Creating Rails App
------------------
To create a new rails app acme_cms. Replacr acme_cms with any name you want to.
```
rails new acme_cms --skip-active-record --skip-test-unit --skip-javascript --skip-bundle
```
No change current working directory to the newly created rails app
```
cd acme_cms
```
Open your Gemfile and replace everything with the following:
```
source 'https://rubygems.org'

gem 'rails', '3.2.19'

gem 'locomotive_cms', '~> 2.5.6', :require => 'locomotive/engine'

# Gems used only for assets and not required
# in production environments by default.
group :assets do
  gem 'compass',        '~> 0.12.7'
  gem 'compass-rails',  '~> 2.0.0'
  gem 'sass-rails',     '~> 3.2.6'
  gem 'coffee-rails',   '~> 3.2.2'
  gem 'uglifier',       '~> 2.5.1'
  gem 'therubyracer', :platforms => :ruby
end

# Use unicorn as the app server
gem 'unicorn'
```
Run bundle install to install all dependencies
```
bundle install
```

Setup Locomotive CMS
----------------------
In your rails app root run the following command to setup Locomotive CMS
```
bundle exec rails g locomotive:install
```
This command has installed several configuration files and given us instructions on how to proceed. Since this is just a development Engine, we can ignore most of these instructions.

To avoid any potential conflicts with other LocomotiveCMS installations, let's change the name of our mongoid databases in config/mongoid.yml, as shown in the example below.
```
development:
  sessions:
    default:
      database: acme_cms_development
      hosts:
        - localhost:27017
  options:
    identity_map_enabled: true

test:
  sessions:
    default:
      database: acme_cms_test
      hosts:
        - localhost:27017
  options:
    identity_map_enabled: true

production:
  sessions:
    default:
      database: acme_cms_production
      # heroku
      # uri: <%= ENV['MONGOHQ_URL'] %>
      hosts:
        - localhost:27017
  options:
    identity_map_enabled: true
```

Finally, we are ready to start LocomotiveCMS. Make sure mongod is still running and run the command below.
```
bundle exec unicorn_rails
```
If above command gives a load error, comment out this line in file config/initializers/devise.rb:
```
Devise.setup do |config|
#config.secret_key = "key"
```
