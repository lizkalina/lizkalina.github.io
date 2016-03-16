---
layout: post
title: "Authentication with Devise"
date: 2016-03-15 21:36:08 -0400
comments: true
categories: ["Flatiron&nbsp;School","authentication","devise"]
---

After building out basic authentication in class, I was curious about how to implement some of the widely used authentication plug-ins like [Devise](https://github.com/plataformatec/devise) and [Authologic](https://github.com/binarylogic/authlogic), which the RailsGuides mentioned. A ton of developers are using these plug-ins. The Devise Github page has been starred ~15,000 and forked ~3,500 times! To get a better grasp of implementing one of these solutions, I decided to walk myself through the installation and configuration of Devise using the documentation and an example applications. 

# In a nutshell
According to the Devise documentation:
>Devise is a flexible authentication solution for Rails based on Warden. It:

>* Is Rack based;
>* Is a complete MVC solution based on Rails engines;
>* Allows you to have multiple models signed in at the same time;
>* Is based on a modularity concept: use only what you really need.

The Devise gem is built on top of [Warden](https://github.com/hassox/warden)—a Rack application that runs as a separate and standalone module. Warden usually executes its role, including cookie handling and modified app behavior (hooks) for users who aren't logged in, before your Rails application is invoked. Once your application is running, Devise supplies helper methods, controller classes, views, models, routes, and configuration for authentication. 

###Models can inherit up to 10 Devise modules! 

When you run the Devise generator, a model class and migration files are created. You can modify the configuration options (aka modules) on the model classes to determine what security features your application includes.

* *Database Authenticatable:* checks that correct password entered, and encrypts before saving
* *Confirmable:* sends confirmation emails to verify account
* *Recoverable:* resets the user password and sends reset instructions
* *Registerable:* handles registration process, also allows users to edit and destroy their account
* *Rememberable:* remembers the user from a saved cookie, implemented with Remember Me?
* *Trackable:* tracks sign in count, timestamps and IP addresses for users
* *Validatable:* provides validations of email and password
* *Lockable:* locks an account after a specified number of failed sign-in attempts
* *Timeoutable:* expires sessions that have not been active in a specified period of time
* *Omniauthable:* adds OmniAuth support

###Example: User model
```ruby
class User < ActiveRecord::Base
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable

  attr_accessible :email, :password, :password_confirmation, :remember_me, :username
end
```
###Example: User migration
```ruby
class DeviseCreateUsers < ActiveRecord::Migration
  def change
    create_table(:users) do |t|
      # Database authenticatable
      t.string :email,              :null => false, :default => ""
      t.string :encrypted_password, :null => false, :default => ""

      # Recoverable
      t.string   :reset_password_token
      t.datetime :reset_password_sent_at

      # Rememberable
      t.datetime :remember_created_at

      # Trackable
      t.integer  :sign_in_count, :default => 0
      t.datetime :current_sign_in_at
      t.datetime :last_sign_in_at
      t.string   :current_sign_in_ip
      t.string   :last_sign_in_ip

      t.timestamps
    end

    add_index :users, :email,                :unique => true
    add_index :users, :reset_password_token, :unique => true
end
```


---
<br>
#Setting up Devise

###Step 1: Add Devise to your Gemfile

```ruby
gem 'devise' # Use Devise for user authentication
```

###Step 2: Bundle & run the setup generator
```bash
bundle install # Fetch and install the gems
rails generate devise:install #Creates config file, etc.
```
Devise has a well commented configuration file located at config/initializers/devise.rb. This is where site-wide settings are specified. It's important to look over this file before creating new models or adding Devise to your existing models.

###Step 3: Create models (or add Devise to existing models)
```bash
rails generate devise MODEL #Creates model class, routes, etc.
```

Replace MODEL (could be User, Admin or anything you would like) with the class name used for the application’s users. This will create a model (if one does not exist) and configure it with default Devise modules. The generator also configures your config/routes.rb file to point to the Devise controller.

###Step 3: Migrate models
```bash
rake db:migrate # Create model table(s)
```

###Optional: Quickly create default views 
```bash
rails generate devise:views users
```

This command creates the directory /app/views/users with all the devise views, including login form, registration form, etc. These views will help you get started, but after some time you'll probably want to change them.

---
<br>
#Devise helper methods

A few commonly used methods include:

* current_user
* user_signed_in?
* sign_in(@user)
* sign_out(@user)
* user_session
* authenticate_user!

Possible use case: Let's call the authenticate_user! class method (via the controller) to ensure that a user can only access certain controller actions based on their whether or not they're logged in.

###Ensure that every controller action requires a user to be logged in...
```ruby
class ApplicationController < ActionController::Base

  before_filter :authenticate_user!

end
```
###...except for the login and register controller actions.
```ruby
class HomeController < ApplicationController

  skip_before_filter :authenticate_user!

  def login
  end

  def register
  end

end
```

## Conclusion (kind of)

The RailsGuides mention that "75% of attacks are at the web application layer, and that out of 300 audited sites, 97% are vulnerable to attack." That's huge! The attacks can come from a variety of routes including account hijacking, bypassing of access control, presenting fraudulent content or modifying sensitive data. There are a bunch of plug-ins available, but there is no one-size fits all approach to security and your protocol should be built around the needs of your application. Once you're up and running, it's important to keep up-to-date on new authentication developments and perform regular security checks to guarantee your site is still protected.

###Helpful Resources
* [RailsGuides: Security](http://guides.rubyonrails.org/security.html)
* [Devise Documentation](http://www.rubydoc.info/github/plataformatec/devise/)

{% img center https://media.giphy.com/media/Z34IiLkiwSbKw/giphy.gif 300 300 %}




