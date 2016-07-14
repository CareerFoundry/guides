Before you can upgrade your Rails app, you should make sure to follow the instruction in our BLOG POST to upgrade your Ruby version with rvm to 2.3.1 (when the blog post was written, the most recent version was 2.3.0. Now that's actually already outdated. So make sure to upgrade to 2.3.1)

## Update your Gemfile

Update the version numbers of the following gems in your Gemfile:

```ruby
gem 'rails', '~> 5.0.0'
gem 'sass-rails', '~> 5.0'
gem 'coffee-rails', '~> 4.2'
gem 'will_paginate', '~> 3.1.0' # If you have this already installed
```

Make sure the turbolinks gem does not have a version number.

If you are using the spork-rails gem you have to comment that one out for now. Unfortunately it's not yet supported in Rails 5

```ruby
# gem 'spork-rails'
```

Now run `bundle update`.

You also need to add a few gems:

```ruby
# Use Puma as the app server
gem 'puma', '~> 3.0'

group :development do
# Access an IRB console on exception pages or by using <%= console %> anywhere in the code.
gem 'web-console'
gem 'listen', '~> 3.0.5'
# Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
gem 'spring'
gem 'spring-watcher-listen', '~> 2.0.0'
```

If you're on Windows you might also want to add this gem:

```ruby
# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```

Now go ahead and run `bundle install`.

## Run the update task

Before you run this, make sure to create a copy of your routes file somewhere. It's very likely that its content will be overwritten with the following task.

Rails offers a task to help you update your files. Run `rails app:update` and always type `Y` to overwrite your old files.

If necessary, add back the contents of your routes file.

## Additional updates

Follow the instructions in Rails Guides: http://guides.rubyonrails.org/upgrading_ruby_on_rails.html#upgrading-from-rails-4-2-to-rails-5-0

The main steps you need are:  2.2, 2.3, 2.4

If you're using `protext_from_forgery` anywhere 2.12 is important, too. Otherwise that's probably all you need.

## Update your JavaScript

With Turbolinks 5 (got released together with Rails 5) neither `$(document).ready(function(){ ... })` nor `$(document).on('ready page:load', function(){ ... })` will work anymore. Instead they came up with an even better solution which loads even faster and even on page load:

```javascript
$(document).on('turbolinks:load', function() {
  // JavaScript in here will be loaded when the page is ready
});
```

So don't forget to replace all your JavaScript code accordingly.

This doc is work in progress. At CF we are still testing all the changes and stability of Rails 5 ourselves. Don't make these changes on a production app without talking to your tutor or mentor first!
