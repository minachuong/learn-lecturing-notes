We’ve been in the Model section of a Rails app for quite a while. And now it’s time to shift out of that context and into Controllers.

Before we do that though, let’s revisit the topic of how our app works in relation to the internet.

When you access a website, you have a URL.
README NOTE:
URL which is Uniform Resource Locator, the address that is associated to some way of sending data to an application

Let’s break down this URL
https://github.com/learn-academy-2021-alpha
https:// is the protocol
github is the host
.com is the top-level domain  

github.com together is usually referred to as your domain/host

/learn-academy-2021-alpha is a segment of a path

When a URL is accessed, we press enter after providing the URL in the browser, the first point of contact to app is made through a controller.

README NOTE: What is a controller?
It is where actions/methods associated with requests live.
The controller is responsible for handling incoming HTTP requests and providing a response.

What is a resource?
In the HTTP context, a resource is a model.
We can create, read, update, and delete a resource.
We can create, read, update, and delete a Dog. Dog would be resource.  

But we’re not going to get into the CRUD right now.
We just want to focus on the mechanics of connecting a URL to some action in our controller.

README NOTE:
The controller is also orchestrates the interaction between the model and the view.

What does that mean?
The model is responsible for manipulating the data that goes into our database. 
The view is responsible for providing some visual representation of our data which usually involves some HTML. 
The controller takes the data from our models and renders our views with that data in order to generate a response.

Okay, so all that said, we need to generate a controller to see this stuff in action.

continuing with the pet_app… 
rails new amazing_app
cd amazing_app
rails db:create
rails s

$ rails g controller Main
again often the convention controller is PascalCase…we could do rails g controller Dog if we wanted a controller for Dog, but right now I want something separate as an example.

Notice all the things that get generated:
>> app/controllers/main_controller.rb
>> app/views/main
>> app/helpers/main_helper.rb
>> app/assets/stylesheets/main.scss
>> and because i have rspec-rails installed, it also generates a spec file: one for each the view and the controller

main_controller.rb: cool not much logic here (it’s inheriting from an ApplicationController class so that there are some methods we have access to
app/views/main: just a directory, no file
main.scss: nothing in file

In our MainController, let’s provide an action/method of “hello”
def hello
  render html: 'HELLO WORLD!'
end

README NOTE
render - a method that takes a varying number of arguments: the arguments indicate options for how we would like data to be rendered.  

Here, we’re indicating that we’d like to render html.
<html><body>HELLO WORLD<body><html>

https://api.rubyonrails.org/classes/ActionController/Renderer.html#method-i-render

This is great but how does this method of hello actually get triggered? What calls it?  The URL should have some kind of path that triggers this method right?

so let’s check 
localhost:3000/hello

ROUTING ERROR: No route matches [GET] “/hello”

Well this makes sense because we haven’t told the app how to match the URL to a method.

This is where the routes.rb file comes into play.

add to routes.rb
get '/hello' => 'main#hello'

Let’s break this down 
get - HTTP method for reading data
‘/hello’ is the segment or path we’re providing in the URL
‘main#hello’ is the ‘controller#method’

Okay so what did we do there?

README NOTE:
Making the connection:
1. generated the controller: $ rails g controller Main
2. added an action on the controller that uses the render method
def hello
  render html: ‘HELLO WORLD'
end
3. added the route to associate a URL segment with the action in the controller
http method ‘/url segment’ => ‘controller#action'
get ‘/hello’ => ‘main#hello'

Does the URL segment always have to be the same name as the action? No.
change in routes.rb
get ‘/supercalifragilisticexpialidocious' => 'main#hello'
try localhost:3000/supercalifragilisticexpialidocious
then try localhost:3000/hello >> Routing Error


Let’s do that again!
Let’s add another action on MainController:
def reminder
  render html: "Don’t forget to create a branch"
end
Add route
get '/reminder' => 'main#reminder'


When we visit localhost:3000 without any following path or segments, Rails provides us with a default landing page. 

README NOTE:
Landing page is what the user sees when they first visit your site.

But we probably don’t want to have users “land” on the default Rails page.
So to change that we can provide a route to indicate what the landing page should be. 
Let’s say for instance, we want our users to land on the hello world page.

So we can provide a route
Add to route.rb
get ‘/‘ => ‘main#hello’
This slash is literally the slash that appears after localhost:3000/
Refresh and verify.

But Rails has a special way of doing this.
It uses a method called ‘root’
root to: ‘main#hello'
However, whenever you use ‘root’ because it’s expected to be the most common URL that gets visited for this site, it should be the first to be match. So we place it at the top of the routes file.

Otherwise it does a check on each route to see if the segment matches until it gets to the one that does match. Because we want the most common one to match right away, we put it at the top so that it’s the first one to match.

Okay cool but where to the views come into play?
So let’s say we want to provide more complex html aside from the super simple html that we’ve been providing

Inside the views directory, when we generated the controller a new directory ‘main’ was created.  In that, we can create a new view file.
<< create a file in views/main/
reminder.html.erb
 >>

There’s no real file name convention here. The most important part is that we provide the file with the correct extensions .html.erb

README NOTE:
To create a view file, filename.html.erb
erb stands for embedded Ruby. We’re embedding Ruby into HTML.

Let’s just add some text to the view file to see what that does.
Add to reminder.html.erb:
Please remind me to take a branch every 25 minutes.

In order to user this view file, we need it to be rendered by the controller. 
Update main_controller.rb
change 
render html: "Don’t forget to create a branch"
to
render "reminder.html.erb"
render can take a path to the view file.

Visit localhost:3000/reminder to verify.

So we haven’t really embedded any Ruby into a view file. Like it’s just text right now.

Let’s create an instance variable in the controller within our reminder method
Add to main#reminder in main_controller.rb
@number_of_minutes = 15
Update reminder.html.erb
<h1>Please remind me to take a branch every <%=@number_of_minutes%> minutes.</h1>

README NOTE: 
Syntax for embedding Ruby into an erb file:
<%= ruby_code %> 
Instance variables declared in the controller can be embedded and accessed in a view file.

We can link to other routes from within our view files too!
Add to reminder.html.erb
<%= link_to 'Word Vomit', ‘/supercalifragilisticexpialidious' %>

README NOTES:
link_to - a method that takes a string (link display text), another string that is the segment of the URL on a route

Add to reminder.html.erb
<h3>Back to Home Pls</h3>
<%= link_to 'Home', '/' %>


Review because there are lot of moving parts
* routes.rb is where all routes live (like an address book)
* Each route basically has two parts: the URL segment and the method on a controller
* The controller provides the response to the request created by the URL
