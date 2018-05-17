---
templateKey: blog-post
title: What's Coming in Rails 5
date: '2015-07-22T10:37:00-07:00'
tags:
  - action cable
  - actioncable
  - rails
  - rails 5
  - rails api
  - rails5
  - ruby
  - ruby 2.2.2
  - ruby on rails
  - turbolinks
  - turbolinks 3
---
<h1>Rails 5</h1>
As has become tradition, a new full version release of Rails is coming just two years after the release of Rails 4. This new version progresses the platform by bumping the underlying minimum Ruby version and adds a slew of neat features to keep Rails feeling fresh and new.
<h2>Ruby 2.2.2</h2>
<p style="padding-left: 30px;">Ruby 2.2.2 is required in Rails 5 because Rails 5 will take advantage of the new Symbol Garbage Collection found in Ruby 2.2. There is also rumor of Rails 5 using the Incremental Garbage Collection found in Ruby 2.2. They have decided to use Ruby 2.2.2 since Ruby 2.2 had a major security vulnerability that is patched in 2.2.2.</p>
<p style="padding-left: 30px;"><a href="https://github.com/rails/rails/pull/19257" target="_blank">https://github.com/rails/rails/pull/19257</a> <strong>Ruby 2.2.1 PR</strong></p>
<p style="padding-left: 30px;"><a href="https://github.com/rails/rails/commit/32f7491808d2c4e097ed7b3ee875e4d1cea8c442">https://github.com/rails/rails/commit/32f7491808d2c4e097ed7b3ee875e4d1cea8c442</a> <strong>Ruby 2.2.2 Commit</strong></p>

<h2>Rails API</h2>
<p style="padding-left: 30px;">Many Rails developers these days are finding themselves using Javascript Frameworks more and more. Whether DHH likes that or not it's a fact of life. Before Rails 5 developers turned to the ruby-api gem which helps create a minimalist Rails application specifically for use as an API. This functionality is now going to be wrapped up and packaged with Rails 5 so no need for another gem. Just use the command `rails new &lt;application name&gt; --api` and Rails will create your new API app all on its own!</p>
<p style="padding-left: 30px;">Here are a couple of tutorials for you <a href="http://wyeworks.com/blog/2015/6/11/how-to-build-a-rails-5-api-only-and-backbone-application/" target="_blank">Backbone</a> and <a href="http://wyeworks.com/blog/2015/6/30/how-to-build-a-rails-5-api-only-and-ember-application/" target="_blank">Ember</a> users.</p>
<p style="padding-left: 30px;"><a href="https://github.com/rails/rails/pull/19832" target="_blank">https://github.com/rails/rails/pull/19832</a></p>

<h2>Turbolinks 3</h2>
<p style="padding-left: 30px;">Turbolinks has been a part of Rails since Rails 4 but is getting a major update to hopefully make developers happier about using it. Turbolinks has been <a href="http://staal.io/blog/2013/01/18/dangers-of-turbolinks/" target="_blank">criticized for having major usability problems</a> but the concept of only loading portions of the DOM that change is a sound idea. Many Javascript Frameworks take advantage of this idea specifically React.js. Turbolinks will fetch the body content of your page without worrying about rerendering the CSS and Javascript. You can opt-in to specify which parts of the page should be changed if you'd like as well. They also added a progress bar by default to help the user see things are happening behind the scenes, but with the increased speed you hopefully won't need that.</p>
<p style="padding-left: 30px;"><a href="https://github.com/rails/turbolinks" target="_blank">https://github.com/rails/turbolinks</a></p>

<h2>Action Cable</h2>
<p style="padding-left: 30px;">Action Cable is the feature I am most excited about. Simpler web sockets for Rails. Anytime anyone says web sockets to me I cringe a little just because of how complicated they can be to set up. Many have tried to make the problem easier and Action Cable is Rails' way of giving it a try.</p>
<p style="padding-left: 30px;"><a href="https://github.com/rails/actioncable" target="_blank">https://github.com/rails/actioncable</a></p>

<h2>Rake or Rails</h2>
<p style="padding-left: 30px;">The beginner's dillema, do I use `rake db:migrate` or `rails db:migrate`, is it `rake test` or `rails test`? Doesn't matter anymore, it's all `rails`. From Rails 5 on the `rails` command can be used to run `rake` commands. Simple change but a nice one.</p>
<p style="padding-left: 30px;"><a href="https://github.com/rails/rails/issues/18878" target="_blank">https://github.com/rails/rails/issues/18878</a></p>

<h2>Integration Tests</h2>
<p style="padding-left: 30px;">Rails 5 is beginning a push to deprecate Controller tests all together in favor of Integration tests. As part of that they are deprecating `assigns()` and `assert_template` in controller tests. Aaron Patterson has <a href="https://www.youtube.com/watch?v=B3gYklsN9uc&amp;list=PLE7tQUdRKcybf82pLlMnPZjAMMMV5DJsK&amp;index=6" target="_blank">a great keynote from Railsconf</a> where he outlines the speed improvements made to the Rails testing environment and why Integration tests will be the way to go.</p>
<p style="padding-left: 30px;"><a href="https://github.com/rails/rails/pull/19058" target="_blank">https://github.com/rails/rails/pull/19058</a></p>

<h2>Update</h2>
I gave this presentation to the SLC.rb user group July 28th, 2015 and here are the slides from that presentation in case anyone is interested.

<a href="http://eric.sau.pe/wp-content/uploads/2015/07/Rails-5.pdf">Rails 5</a>

A lot of other under the hood improvements are expected to be made but I think I covered a lot of the major upcoming features. Let me know if you have any questions or which Rails feature you are most excited about by leaving a comment below.
