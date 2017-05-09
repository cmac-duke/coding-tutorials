---
date: 2017-01-16
title: Deploying to Heroku
categories:
  - Rails
description: Jekyll template for a small business
type: Document
---
Without sounding too much like a salesman for heroku. Heroku is a server as a service that allows us to stand up applications quickly and efficiently. [Heroku] (https://www.heroku.com/) Traditionally, we would have to by time on an empty Liux box, install all of the dependencies necessary for our application to run, move our application over to the box, setup Passenger or some other web server, and stand up our application ourselves. This is a collosal pain. Heroku allows you to stand up a full web application without worrying about the hastle of learning dev-ops.

We can simply take our application and push it to Heroku much like we would Github or any  other git service. Better still, Heroku allows you to hook into Github for "Continuous Integration" (CI).


Here's how to get started:

## Login

```
$ heroku login
Enter your Heroku credentials.
Email: schneems@example.com
Password:
Could not find an existing public key.
Would you like to generate one? [Yn]
Generating new SSH public key.
Uploading ssh public key /Users/adam/.ssh/id_rsa.pub
```


## Create the application

```
$ heroku create
Creating app... done, safe-mesa-35727
https://safe-mesa-35727.herokuapp.com/ | https://git.heroku.com/safe-mesa-35727.git
```

## Push you code

```
git push heroku master
```

## Migrate the data base

```
heroku run rake db:migrate
```


## Visit Your Site

If all went well, you should see your site standing at the address listed aftr you pushed your code. You can also run `heroku open` to visit it directly

```
heroku open
```

Thats it! you web app is live.