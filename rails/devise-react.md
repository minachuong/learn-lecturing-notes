So far we just have a Rails app with Devise on it, but we know we want some of that React so let’s install React using the ‘react-rails’ gem.

$ bundle add react-rails
$ rails webpacker:install
$ rails webpacker:install:react
$ rails generate react:install
$ rails generate react:component App
$ rails generate controller Home

To render something on our root path ‘/‘ 
we need to add a view and drop a react component in it.
Yesterday,
CREATE file app/views/home/index.html.erb
<%= react_component “App" %>

ADD App.js with content
<h1>HELLO! DO YOU SEE ME!</h1>

ADD to routes.rb
root to: 'home#index'
Recall, react_component is a view helper where we’re indicating the component name, in this case App

Because our app is concerned with a user being logged in we can pass some props into our App component.
We’ll generally need a handle on the current_user and whether they’re logged in

react_component also takes a hash as a second argument where it transforms it to react props

ADD app/views/home/index.html.erb
<%= react_component "App", {
  logged_in: user_signed_in?,
  current_user: current_user,
  new_user_route: new_user_registration_path,
  sign_in_route: new_user_session_path,
  sign_out_route: destroy_user_session_path
} %>

Routes and Constraints
we don’t want anyone to hit the api with requests that are asking for html.
The front end will handle the html views and pages so it should only be making requests to the backend for json objects.
So we can supply a rule in our routes to just redirect all requests for html payloads to our App component
ADD to routes.rb above home#index
get '*path', to: 'home#index', constraints: ->(request){ request.format.html? }


Good practice
Add assets, pages, components directories in app/javascript/components

Adding Log in and log out buttons
By default, Devise uses the http DELETE method for a sign_out request
$ rails routes
But we don’t want to have to build a DELETE request and so instead, we’ll reconfigure it to be a GET
Edit routes.rb to be
config.sign_out_via = :get

Now for adding the actual buttons
The main idea is that if a user is logged in, they should see a log out button. Likewise, if they are logged out, they should see a log in button
First, we want to destructure the props passed into App component so that we have a handle on it in javascript.

ADD to App.js
