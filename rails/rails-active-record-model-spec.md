Yay, so now that we’re actually adding some logic to our classes/models, 
what do we need? TESTS! 
How are going to test? FAIL FIRST! 
What are we testing? BEHAVIOR!

Per usual, there is some set up involved. So far we’ve just been testing Ruby code outside of a Rails project. Now that we’re testing within a Rails project, we can use something called “bundle” to keep track of all the different versions of gems associated with our project.

README NOTE: To add Rspec to a Rails project:
$ bundle add rspec-rails >> Gemfile: similar to package.json, you’ll see the rspec-rails dependency gets added to the file.

README NOTE: To generate boiler plate code for Rspec
$ rails generate rspec:install  >> spec/ directory where specs will live
$ rails s >> just to check it works

Now whenever I generate a model, a spec file gets generated for that model.

$ rails g model Veterinarian name:string >> generates the migration file, model file, and the spec for the model! 
>> Let’s look at the model for Veterinarian
$ rails db:migrate

README NOTES: run spec files
$ rspec spec 

brew services stop postgresql
brew services restart postgresql

Cool so now that we spec is running our spec files let’s get to doing some TDD. 

For Veterinarians, we told the database that we wanted Veterinarians to have a name
But we haven’t provided any validations on it. 

So let’s right a test that will assert that a Veterinarian cannot be created with a name
it 'must have a name' do
dolittle = Veterinarian.create name: ' '
expect(dolittle.errors[:name]).to_not be_empty
end

Failure!! No error was generated

Let’s add a validation
validates :name, presence: true

What if we want a custom message for the error?
add to spec file
expect(dolittle.errors[:name].first).to eq 'Please provide a name'

FAILURE because messages are different

add to model file
presence: { message: "Please provide a name" }

PASS
