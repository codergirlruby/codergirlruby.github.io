---
layout: post
title: "Deploying Rails App to Heroku"
date: 2014-09-29 12:00:51 -0700
comments: true
categories: 
---
I wanted to deploy my rails blog on heroku but since heroku does not suppot sqlite3 I had to migrate to postgres
for my database.

it was pretty easy to do

I swapped out sqlite3 to pg in my gemfile and I also installed pg on my system to be able to run pg locally

on arch, installing is a breeze

{% codeblock %}
pacman -S postgresql
{% endcodeblock %}

I then created a db as well as a user for my app.
create role myapp with createdb login password 'password1'
then i added the info to my db.yml which should look like this:

{% codeblock %}
development:
  adapter: postgresql
  encoding: unicode
  database: myapp_development
  pool: 5
  username: myapp
  password: password1

test:
  adapter: postgresql
  encoding: unicode
  database: myapp_test
  pool: 5
  username: myapp
  password: password1
{% endcodeblock %}

after setting my app up with pg everything worked fine until I deployed to heroku and then I ran into a problem with my apps routes not showing.

but this did the trick


heroku run rake db:migrate
heroku restart
