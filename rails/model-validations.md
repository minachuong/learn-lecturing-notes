At this point, our index and create actions are handle really happy case scenarios. 
What happens if the database decides that it doesn’t want to create a cat for some reason?
or What if our front end developer decides to make a request that doesn’t have all the data we need to properly create a cat? 

Well, we need to code for those scenarios and signify to whatever is making a request that something went wrong.

TRELLO: move API Validations card to DOING
CREATE BRANCH: api-validations

Task: As a developer, I can add the appropriate model specs that will ensure an incomplete cat throws an error
Task: As a developer, I can add the appropriate model validations to ensure the user submits a name, an age, and what the cat enjoys

So remember that we have some level of protections on data that can enter our database.
Right now cat.rb is empty, no model validations

In spec/models/cat_spec.rb
ADD
it 'should have a valid name' do
end

So to ensure that we’re specifically always requiring that a name is provided, let’s try to create a cat without a name
cat = Cat.create(age: 4, enjoys: 'Walks on the beach')
expect(cat.errors[:name]).to include "can't be blank"

FAIL:  expected [] to include "can't be blank"

ADD to cat.rb
validates :name, presence: true
PASS

ADD to cat_spec.rb
# it 'should have a valid age' do
# cat = Cat.create(name: ‘Daria', enjoys: ’pushing things off tables')
# expect(cat.errors[:age]).to include "can't be blank"
# end


# it 'should have a valid enjoys' do
# cat = Cat.create(name: ‘Daria', age: 4)
# expect(cat.errors[:enjoys]).to include "can't be blank"
# end
FAIL

ADD to cat.rb
# :age, :enjoys,

TRELLO mark off tasks

TASK: As a developer, I can add the appropriate request spec that will look for a 422 error if the validations are not met
TASK: As a developer, I can add the appropriate request validations to ensure the API is sending useful information to the frontend developer

So why do we have this task? Because we want to be able to signal that something is wrong, otherwise we’ll just encounter an error and things stop working.

We want to ensure that the requests that are made have the data the Cat model needs in order to create a cat and that if some piece of data is missing, there’s a proper response for it.
In this case, the user story is indicating it should return a status of 422 if any of the validations fail.
What is 422?
GOOGLE

ADD to cats_requests_spec
it 'cannot create a new cat without a name’ do
    #arrange
    cat_params = {
        cat: {
            age: 8,
            enjoys: 'Sleeping on your face'
        }
    }

    #act
    post ‘/cats’, params: cat_params

    #assert
    error_response = JSON.parse(response.body)
    
    expect(error_response[’name’]).to include “can’t be blank”
    expect(response).to have_http_status(422)
end
 FAIL: expected nil to inlaced “can’t be blank”
BYEBUG

We know that the cat instance here is invalid because the id is nil, nothing was  actually created in the database
So we need to check on the validity of the cat record to determine what kind of response we want to build.
So if the record is invalid, we should probably return the model validation errors as the response so that whoever is making the request has some idea of what went wrong

ADD cats_controller
if cat.valid?
  render json: cat
else
  render json: cat.errors, status: 422
PASS

Now we need to the same for age and enjoys
ADD
it 'cannot create a new cat without an age' do
    cat_params = {
        cat: {
            name: 'Neil',
            enjoys: 'Watching the goldfish bowl'

        }
    }

    post '/cats', params: cat_params

    cat = JSON.parse(response.body)
    expect(cat['age']).to include "can't be blank"
    expect(response).to have_http_status(422)
end

it 'cannot create a new cat without an enjoys' do
    cat_params = {
        cat: {
            name: 'Neil',
            age: 4
        }
    }

    post '/cats', params: cat_params

    cat = JSON.parse(response.body)
    expect(cat['enjoys']).to include "can't be blank"
    expect(response).to have_http_status(422)
end

TRELLO check tasks
TASK: As a developer, I can add a validation to assure that the enjoys value is at least 10 characters long

ADD at cat_spec.rb
it 'should have an enjoys description with at least 10 characters' do
    cat = Cat.create(name: 'Felix', age: 7, enjoys: 'Walks')
    expect(cat.errors[:enjoys]).to include "is too short (minimum is 10 characters)"
end
FAIL: expected [] to include ...

ADD to cat.rb
validates :enjoys, length: { minimum: 10 }

Do we need to add a test in cats_requests_spec?
Nope because the create method is really only concerned with covering logic concerning the response. We didn’t make any changes in the controller, just the model.


Check off TASKS 
CREATE PR

