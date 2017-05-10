---
date: 2017-01-16
title: User Sign Up with Devise Gem
categories:
  - Rails
description: Jekyll template for digital agencies
type: Document
---
The Devise Gem for Rails is on of the more popular gems in the system. The [Desive Gem] (https://github.com/plataformatec/devise) allows developers to plug in a sign-in/sign-out system for users of thier site. It is robust and flexible, and developers can customize it and extend it however they please. Lets check out Devise and how we can use it to keep track of our users.


The inital step is to include the gem into our GEMFILE

```
gem 'devise'
```


and bundle install to install it into our application. 

Next, well run devise install to install all of the config files necessary for Devise to work correctly in out application

```
rails generate devise:install
```


Then well use a built in devise generator to create the migrations, models, and controllers to get up and running with devise

```
rails generate devise User
```

Here we're telling devise to create a User model, and create a sign in system for our users. If we wanted to create a sign-in system for our Administrators, we would run `rails generate devise  Admin`

Lets run `rake db:migrate`  to migrate the changes we've just made.

We should now be able to navigate to `localhost:3000/sign-in` to see our new sign in page. 

We can test it with any email/password combo to log in.

We can see that devise has generated a login, log-out, sign-up, and accounts page for us. Great! We have a new login system. 

The next step is not manditory but something that I always do when setting up an application. If we navigate to our views folder, we can see that we haven't yet created views for our sign-up system, and the default views are pretty ugly.

Lets create our views  

```
rails generate devise:views
```


We should now find a devise folder inside of our views folder with the all of the views we need to change.

So we now have a sign up system up and running, but it's not doing much of anything yet. If someone navigates to our app, they can still see each page in our application. We want to keep users who haven't signed up from navigating around our site, or else...why go through all of this trouble. 

Lets lock down some pages of our site. This can be done with a single line of code in our controllers. Pick a page and its associated controller that you would like to keep our of reach of people who haven't signed up for your site. In my application I have projects that people can sign up for, but I only want registered users to be able to sign up. I'll add a `before action` in my Projects Controller with `authenticate_user!` that will ensure that a user is autheticated in my system before sign up.

```
class ProjectController < ApplicationController 

  before_action :authenticate_user!

...
```

