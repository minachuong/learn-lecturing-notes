Last week, a lot of what we did was spin up an application and primarily use that application as a tunnel to be able to interact with a database. We barely did anything in the actual app. Yes, we might have created some migrations and generated model files. But most of the code we were writing were queries that asked the database to do CRUD actions and even then it was temporary code because all those queries only live the session of the terminal or console.

At this point, the data that we’ve been entering into the data has sorta been the wild west. Minus some data type checking, we could put whatever we want for a name.  We might even be able to get away with doing something like added a Dog with a name that is only spaces.  

<<rails console in pet_app>>
<< query for a dog and store name to be “     ::spaces::      “>>

Is this something we want to allow?
Like 1) it’s not likely a Dog’s name is legally a bunch of spaces and even then, it would be really hard to support this kind of name because welp it’s a bunch of spaces. Is it 1 space? 20 spaces? 
2) But also I think as humans and our social standards of taking care of dogs, Do we really want to support this kind of name? No, every dog deserves a real meaningful name.

We’re developers right? And the thing about being developers is we have the power to dictate what goes into the database…so we must use our powers for good and create must ensure all dogs that get created in our database are created with names that isn’t just whitespace.

It turns out, Rails developers understood the need to ensure that only right kind of data should make it to the database. The database shouldn’t have to deal with bad data because the database's only responsibility should be for CRUD actions, not validating data. Of course we can write sql logic so that it can check for bad data but it slows the database down. This checking of data should really happen in our app because that’s where all the “business logic” should live. The process of ensuring our data is good for the database referred to as model validation.  Rails allows how to declare model validations.

So here we are, 
we have the class of Dog but no logic it in. 
Again because we’re in the realm concerning data that will be stored in our database, we’re placing the validations in our model. (We’re still in the M of the MVC).

README NOTE: Model Validations - process of ensuring data is valid and suitable for the database

$Dog.create>> no arguments and a dog is created with no data… this instance has an actual id

README NOTE: Syntax for providing a model validation in Rails
validates :column_name validator: conditional

README NOTE: validates - a method that takes a varying number of arguments 

<< add code to Dog class >>
validates :name, presence: true

$Dog.create>> can still create a dog instance but look! the id is nil! 
$Dog.all >> Note: no Dog instance was inserted into the database
$Dog.create!>> Yell at it!  It yells back! Validation failed: Name can’t be blank
<<$ last = Dog.last
$ last.name = “         “
$ last.save   >> false — SILENT FAILURE
$ last.save!  >> Name cannot be blank.
$ last.valid? >> false

README NOTE: 
When do model validations run?
Any time a method that tries to perform a CRUD action, the validations will run. 
The difference between just calling the CRUD method and yelling the method is whether Active Record will yell back.

README NOTE: 
.valid? -a method that runs the model validations without attempting CRUD and allows us to check the validity of the instance and returns true or false
if it returns false, it’ll add errors on the instance to an instance variable named "errors"

README NOTE:
.errors - a method that returns the list (array) of errors encountered while running model validations
$ pp last.errors
$ pp last.errors[:name]  >>  ["can't be blank”] 

But just because a name is present doesn’t mean it’s meaningful

I could create a dog so that it’s name is “A” 
$ Dog.create! name:”A"

which okay maybe it’s meaningful but generally most names require two letters
So to strengthen our validation and super restrict single letter names: let’s change the validator from checking on to checking on character length…

<<Rails Dog: validates :name, presence: true, length: { minimum: 2} >>

The length validator requires a Hash where we can indicate length property the name should have. In this case, we’re saying it needs to have at least 2 characters.

$ Dog.create! name: ‘A’  >> Name is too short
$ Dog.create! name: “A “ >> DOH! — This becomes a question of how is the app getting this kind of data and where should the chomping of the data happen (maybe it’s about a UI?)

Dog (name, breed, coat_color, ear_shape, birthdate)

<<Rails Dog  validates :breed, :coat_color, :birthdate, presence: true>>
$ Dog.create! >> all errors
$ dog1 = Dog.create
$ pp dog1.errors

<<Rails Dog    validates :birthdate, uniqueness: true

README NOTE: belong_to associations are default validations

$ rails g model Person name:string
<< person.rb: has_many :dogs
$ rails g migration AddPersonIdToDog person_id:integer
<< dog.rb: belongs_to :person
$ rails db:migrate  >>runs both migrations for creating table and adding column
$ rails c
$ dog=Dog.create
$ pp dog.errors >> Notice: “Person must exist”

$  dog = Dog.create name:"Fluffy", breed: "fluffy", birthdate: DateTime.new(2020,3,12), coat_color: "blue"

$ pp dog.errors
$ pp dog.errors[:person] >> “must exist”

$ mina = Person.create! name: ‘Mina’
$ mina.valid? >> true
$ dog.valid? >> false
$ dog.person = mina
$ dog.valid? >> true
$ dog.save >> true
$ mina.dogs
