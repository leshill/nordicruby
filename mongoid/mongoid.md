!SLIDE

# Mongoid

!SLIDE bullets

# Why Mongoid?

* Rails 3 Compatible
* Atomic Operations by Default
* Handles Large Data Sets Easily
* Badass Criteria API

!SLIDE bullets

# Rails 3 Compatibility

* Gemfile
* Generate Configuration
* Modify application.rb
* Generate Models

!SLIDE

# Modify Your Gemfile

    @@@ ruby
    gem "mongoid", "2.0.0.beta6"
    gem "bson_ext", "1.0.1"

!SLIDE

# Generate Your Configuration

    @@@ ruby
    rails generate mongoid:config

!SLIDE

# Modify Your application.rb

    @@@ ruby
    config.generators do |g|
      g.orm :mongoid
    end

!SLIDE

# Generate Models

    @@@ ruby
    rails generate model person dob:Date name:String --timestamps
