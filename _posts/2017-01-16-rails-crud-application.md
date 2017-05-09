---
date: 2017-01-16
title: CRUD Applications
categories:
  - Rails
description: Jekyll template for an event.
type: Document
---

 **C**reate <br>
 **R**ead <br>
 **U**pdate <br>
 **D**elete <br>


CRUD is the basic foundation to any dynamic web application. Regarless of how complex the application is, where the application is getting its data from, or what kind of data is flowing through the application, any web based application is a CRUD (Create Read Update Delete) application. 

Crud applications can be as complex or simple as one wants, however, when you hear of CRUD applications in the context of web development, it typically means a relatively simple application.

Creating basic CRUD applications are very straightforward thanks to Rails and Active Record. 

This tutorial assumes that you have ruby and rails installed on your machine, and have a basic understanding of what a webapp is. To learn more about the programming language Ruby, or the Ruby on Rails framework, please In this tutorial, we will be creating a CRUD application with SQLite (https://www.sqlite.org/), however, there a just a few extra steps to creating a production application with MySQL or Postgres. (for differences between databases, refer here: )

The first step is to create the application itself. We will start by opening up terminal, running the rails setup command

## Setting up the Appliction

```
rails new [Name of your app] 
```

where [Name of your app] is the name you would like to give your application and database for instance, I'll run 

```
rails new Testapp
```

you should see a long output like the one pictured below


In the output, we see a long list of "gems". The Ruby on Rails framework is really just a compilation of packages, or "gems". Gems are essentially bits of code, written by a community of developer, that help us create applications quickly. 

Every web application framework has some sort of package or gem-like system. Could you imagine if everyone had to create a user login system, or file upload system, or database connection system from scratch? That would be a lot of time waisted on problems that other people had already solved. Gems allow developers to share snippets of code common to most web applications, are destribute them for everyone to use. If you look at the list of gems that were downloaded to our package, we can see that there is a sqlite gem for making connection to or database, theres's a sass gem to handle our stylesheets, and "turbolinks" gem to make our application run smoother and faster. You can open up the GEMFILE to see what other gems have been included in our initial setup

~~~ bash
.
├── _config.yml
├── _data
|   └── members.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.md
|   └── on-simplicity-in-technology.md
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.md
|   └── 2009-04-26-barcamp-boston-4-roundup.md
├── _sass
|   ├── _base.scss
|   └── _layout.scss
├── _site
├── .jekyll-metadata
└── index.html # can also be an 'index.md' with valid YAML Frontmatter
~~~

## Bundling our Gems

Next we'll want to run 'bundle install' to install these gems into our application. The 'bundle' command take all of our gems, and keeps a record of them. 'Install' downloads them and installs them into our application. 


```
bundle install
```
You can remember the bundle command because gems come in bundles? idk. 

## Creating the database

Next, well create the database. To do so, we'll run 
```
rake db:create
```

you can think of "rake" here, as an equivalent to "make" for Rails. 

## Running the application

We now have a database that is waiting to take in information from our rails application. 

Lets check out our application to make sure everything is working correctly. We'll run 'rails server' to stand it up.

```
rails server
```

Go to Chrome or whatever web browser you use and type 'localhost:3000'

You should see your application at this address.

## Scaffolding A CRUD Application

Next, lets have a look at how we can create our **CRUD** application. For this application, lets create a blog. For our Blog, we'll want to be able to create an entry (create), show it on a page (update), change it if it need corrections (update) and delete it (delete). 

Rails makes it extremely easy to create a CRUD application. Lets say our blog has two parts, Title - the title of the blog, and Body- the blog entry. 

We'll run

```
rails generate scaffold blog title:string body:string
```

Here we're telling rails to generate a scaffold (a general structure for our app) for a blog model where the title is a string and the body is a string. 

What is a model? A model is a representation of concept in our app that maps to a table in our database. 

## Migrating What We've made

Our next step is to migrate the database. What is a migration? Migrations keep track of the state of our database. For the type of database that we're working with (Relational Database) its very important to keep track of the state of the database and how the tables interact with eachother. Relational Databases deserve a tutorial of thie own, but for now we can this of a migration as a record of the tables that we have created.

Let's run our migration with

```
rake db:migrate
```

If we were to run this command, and look at our database, we would see that we would have a table call Blog with two columns, one for the title of each blog post and one for the body of each blog post

| title                 | body                                                                             |   |   |   |
|-----------------------|----------------------------------------------------------------------------------|---|---|---|
| Today was a great day | Today was awesome, because I had ice cream and played with my dog and didn't cry |   |   |   |
| Today sucks           | My professor assigned 300 pages of Foucault                    |   |   |   |
|                       |                                                                                  |   |   |   |  

Obviously it would start out empty, but now its time to fill it with blog posts!

Let's run 

```
rails server 
```

again to see our app, and this time lets point our application to "localhost:3000/blog"

You should see a page similar to this:

