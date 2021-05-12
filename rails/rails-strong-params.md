Strong params (short for Strong Parameters)
README NOTE:
Strong params is a feature that provides the first level of data security.

Recall that the controller is primarily the first point of contact (after a URL is matched to a route, that routes to the correct method). 
So while we do have model validations that ensures the right kind of data is going into the database, we want to filter out data we don’t need coming from an incoming request. 

When you have an application deployed to the internet, most of the time, requests will be made as expected but sometimes there are malicious or curious folks (hackers) who want to see how much they can try to penetrate your app.

Where Strong Params comes into play is really to ensure that requests made to our server are made correctly but more importantly that only the columns that we say can be updated are updated--no other attributes can be updated unless we specifically  provide code to permit them.

You saw in Sarah’s full stack lecture that we’re at a point where we can create “endpoints” on our applications.
Endpoints being URLs where someone can make a request and expect a response.
We can dictate how requests should be made and if they’re made in a way that is harmful or doesn’t have all the information we need in order to process the question, we can return a status code or message to indicate that. (TODO: CHECK ON THIS)

README NOTE:
Endpoint - URLs where someone can make a request and expect a response from the server

All that said, 
README NOTE: 
Strong Params - is the indication of which attributes in our params are required for the request and which ones are permitted.

Okay, for implementing strong params, I’ll need to have a controller because that’s where params are surfaced.
I went ahead and added a controller for Dog and CRUD methods (index, new, create, show, update, destroy)
Review methods on dog_controller.rb
Views associated with index, show, edit, delete, new.
We don’t have a view for create because again, that’s the action that gets trigger when a user fills in information on the form on our new.html.erb view file and submits the form. 

And because we’re super concerned with things being secure, we’ll be using a Ruby keyword ‘private’ within our controller class to allow us to define methods that cannot be externally accessed.

README NOTE:
private - Ruby keyword to indicate logic that is not accessible outside of the class/instance

Here is the actual implementation of strong params:
Add to dog_controller.rb
private

def dog_params
  params.require(:dog).permit(:name, :breed, :coat_color)
end
By indicating these attributes as permitted, we’re putting these terms on an “allow list”. You’ll hear the term whitelist but big companies are making a move away from using racist and outdated terminology. Chromium, Google, documentation actually says they’re using “block list” and “allow list” instead of “blacklist” and “whitelist”.  Same reason why GitHub is migrating from using master to main branch.

Update #show
def create
  @dog = create(dog_params)
end

verify that nothing there changes on the webpage.
But we can look at how it’s referencing dog_params using some debugger tools:
byebug -a gem installed in the rails app that allows you to debug your app

Add byebug within #show
def create
byebug
  @dog = Dog.create(dog_params)
end

Show in terminal
$ dog_params >> {dog: {name: “”, breed: “”, coat_color: “"}}
$ c (to continue)


Let’s try to see happens when we try to pass provide an attribute that is not permitted.

Add an input field for another attribute in new.html.erb
Make the create request. 
Show the terminal - there should be messaging for 
Unpermitted parameter: :attribute

It will still create the resource if it’s valid, but that data for the unpermitted attribute doesn’t make it past the Controller

To reiterate why strong params is super important, let’s say someone it trying to be clever and they decide to put SQL into one of our create fields!
“DROP TABLE resource"
