Yesterday you had a chance to see how to create a React in Rails app and play around a little bit. Break it and fix it.

For the rest of the week, you’ll have the opportunity to work individually to create your own Apartment App. 
So let’s take a look at the Apartment App Challenge.
https://github.com/learn-academy-2021-alpha/Syllabus/blob/master/react_in_rails/apartment_app.md

Since we’ll need time to generate the app I’ll do that first, so I can talk about the good stuff.
$ rails new apartment_app -d postgresql -T
$ cd apartment_app
$ rails db:create
$ bundle add rspec-rails
$ rails generate rspec:install

So for this challenge, we’re task with supporting user accounts.
Anytime we’re working with user accounts, we are dealing with things like authorization and authentication. 
What’s the difference between the two?

Authorization and authentication both are processes concerned with confirming something about a user but the question that gets asked about the user is different.

Authentication is a question of “Who are you? Are you really you?” We have some tools like credentials (username and password) to help us answer that. Surely if no one else knows your password and you’ve provided your password, you must be you.

Once a user has been authenticated, then we go on to authorization. We usually check authentication first because if it’s not a legit user, they’ve got no business in our app and we’ll likely return a 403 status “Access Denied”.

Authorization is a question of “What do you have access to?” Every app has their own way of identifying what kind of users get what kind of permissions. So like upon sign in, the app recognizes this is a “Child” account on Netflix, that user should not have access to Mature content. Or like on Google Docs, a person without a Google Account has read-only access, and you can actually configure who gets edit access.

This is where Devise comes into play.  Devise is a super popular gem that handles Authentication and Authorization. The popular part is important because you want any dependencies you choose to have in your app to be well-maintained and tested well. Like if you were a developer of Devise, you’d make sure what you’re doing is right because if an update goes out with a big bug in it, you’d be messing with a lot of apps. (But also, you’d find out very quickly that the bug exists because the community is there to tell you.) When it comes to security of your app and ensuring your users can log in and log out and for all the security measures to be handled properly, you want something you know has stood the test of time.

So let’s install Devise
$ bundle add devise
$ rails generate devise:install
# this generates boilerplate code so that you can use it
# Note: config/initializers/devise.rb & config/locales/devise.en.yml
# NOTE: it also has some instructions upon installation, the first one is REQUIRED
>> config/environments/development.rb  
ADD:
# defined default url
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

$ rails generate devise User
>> generates files needed for a User
# Devise has a user defined in a special way so that it that it can support certain security features
# Look at the user.rb
# Look at routes.rb
# Look at migration file

$ rails db:migrate

$ rails s
>> localhost:3000
>> localhost:3000/users/sign_in
>> localhost:3000/users/sign_up

$ rails routes #to see other routes that are generated

So now for the other main resource, the Apartment.
Users will have many Apartments so we know we’ll need to create a has_many:belongs_to relationship. 
What is the foreign key and where does it live? 😄
Each Apartment will need a reference to a User.
So the user_id is the foreign key and it will live on the Apartment resource

$ rails g resource Apartment street:string city:string state:string manager:string email:string price:string bedrooms:integer bathrooms:integer pets:string user_id:integer

$ rails db:migrate

ADD to apartment.rb
belongs_to :user

ADD to user.rb
has_many :apartments


RECAP:
We created an app, and support authorization and authentication through use of Devise. We created a resource of Apartment and created a relationship between Users and Apartments where Users have many Apartments

