Before you can upgrade your Rails app, you should make sure to follow the instruction in our [BLOG POST](http://blog.careerfoundry.com/web-development/ruby-on-rails-5-its-here-find-out-whats-new-and-how-to-upgrade) to upgrade your Ruby version with `rvm` to 2.3.1 (when the blog post was written, the most recent version was 2.3.0. Now that's actually already outdated. So make sure to upgrade to 2.3.1)

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
end
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

If you're using `protect_from_forgery` anywhere 2.12 is important, too. Otherwise that's probably all you need.

## Update your application layout

In the `app/views/layouts/application.html.erb` file of your app you will have these two lines:

```
<%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
<%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
```

You'll have to update them to look like this:

```
<%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
```

## Update your JavaScript

With Turbolinks 5 (got released together with Rails 5) neither `$(document).ready(function(){ ... })` nor `$(document).on('ready page:load', function(){ ... })` will work anymore. Instead they came up with an even better solution which loads even faster and even on page load:

```javascript
$(document).on('turbolinks:load', function() {
  // JavaScript in here will be loaded when the page is ready
});
```

So don't forget to replace all your JavaScript code accordingly.

## Add new files and folders

The previous steps will add all the necessary configurations. If you were to generate a completely new Rails 5 app, though, your project folder would contain a few more folders and files. Those are **not** necessary for your app to work. But for the sake of completeness and if you are taking the CareerFoundry Web Development course you should make sure you have those files and folders in your project since they are necessary to complete some of the later tasks.

Add the following paths to your app. If some folders don't exist, just add them. For example inside the `app` folder of your app you will not have a `channels` folder. So you'll have to add that one, as well as the `applictaion_cable` folder in the `channels` folder. **It's absolutely essential that you do these things exactly as shown below.**

- `/app/assets/config/manifest.js` and add this content:

```javascript
//= link_tree ../images
//= link_directory ../javascripts .js
//= link_directory ../stylesheets .css
```

- `/app/assets/javascripts/channels` (just the empty folder)

- `/app/assets/javascripts/cable.js` and add this content:

```javascript
// Action Cable provides the framework to deal with WebSockets in Rails.
// You can generate new channels where WebSocket features live using the rails generate channel command.
//
//= require action_cable
//= require_self
//= require_tree ./channels

(function() {
  this.App || (this.App = {});

  App.cable = ActionCable.createConsumer();

}).call(this);
```

- `/app/channels/application_cable/channel.rb` and add this content:

```ruby
module ApplicationCable
  class Channel < ActionCable::Channel::Base
  end
end
```

- `/app/channels/application_cable/connection.rb` and add this content:

```ruby
module ApplicationCable
  class Connection < ActionCable::Connection::Base
  end
end
```

- `/app/jobs/application_job.rb` and add this content:

```ruby
class ApplicationJob < ActiveJob::Base
end
```


>This doc is work in progress. At CF we are still testing all the changes and stability of Rails 5 ourselves. Don't make these changes on a production app without talking to your tutor or mentor first!
