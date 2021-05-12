Yesterday, you might have encountered an issue where you ran the rails generate command with the wrong data type or with weird syntax and it created more columns than expected. And even if you didn’t and you ran typed the command in all perfectly… one day you’ll encounter the scenario of generating a model with the incorrect column name spelling or model name misspelling.  

Then it’s like, oh do I just have to delete all the files that the generate command created and manually remove all the code generated too? That’s not an awesome developer experience because there’s a lot of manually changing things that were automatically generated.

Thankfully, we don’t have to do that and the thing that allows us to correct these things are migrations. And not only is it about correcting these things, migrations are literally how we make any change we want to do the database. Want to add a column to a table? Generate a migration. Delete a column? Generate a migration. Change the name of a column? Change the data type for a column? Migration!

But recall! There are two steps to making changes: 1) Generating the migration script/file and 2) actually running the migration

The conventions and casing here become even more important!!
Just like how yesterday we had to indicate the column_name:data_type and how that was weird. 
Remember convention over configuration?
Well for generating a migration, we have to follow the PascalCase or snake_case convention

So let’s say I want to add coat color to the Dog table we created yesterday.

rails g migration AddCoatColorToDog
rails g migration add_coat_color_to_dog

rails g migration add_column_name_to_table
rails g migration AddColumnNameToTable

It created a new file with a method of “change”
Inside this method, we’ll provide it more specific information
add_column :dogs, :coat_color, :string

Breaking this down:
add_column is an Active Record method to which we’re providing arguments. 
The first argument is the table (reference as a symbol pluralized)
The second argument is the name of the new column provided as a symbol
The third argument is the data type for the column, again as a symbol

add_column is also considered a “migration definition”…there are a ton other change definitions (things that Active Record supports in being able to communicate to the database). 

Definitely look at Active Record DOCUMENTATION to see your options!

Yesterday, we had a migration file for create the table of Dog
So let’s look at that:
Note: create_table is a change definition

Let’s save the migration file.

So now we’ve completed Step 1: creating the migration script.
Step 2: tell the db to migrate.

rails db:migrate

Let’s look at our schema file to make sure it registered the change.
Then verify in Rails console.
pp Dog.all  << Note: the column is there but is nil
Again, if we really wanted to, we could check pgAdmin.

Alright, let’s say we want to add ear shape and birthday
But you know what we already have age but that’s sorta been weird because Myla was 7 months and we have to do this rounding and it’s not accurate. We can get precise age with a birthday. 
So that means not only are we adding a new birthdate column, we’re removing the age column as well.

So let’s break this up.
Let’s first add a column for just ear shape and then we’ll handle the  age and birthday thing in another migration.

Step 1: generating the migration script
rails g migration AddEarShapeToDog ear_shap:string  << MISSPELLING
And I got super trigger happy and just ran that migration
Rails c 
pp Dog.all
ear_shap  << that’s just terrible
I can literally edit my migration file, and edit the schema file!! drop by database and pretend like this horrible misspelling never happened (I’ve done this…you’ll live)
But let’s correct this in a way that don’t require us to manually edit the schema file.

rails g migration RenameEarShapColumnName
Here the name of the migration isn’t super important because we’re not providing it a column:data_type arguments and so it’s not generating any code base off that.

I wonder if there’s a migration definition for remaining a column though….
rename_column(table_name, column_name, new_column_name)

rename_column(:dogs, :ear_shap, :ear_shape)
rails c << yay! ear_shape!

Alright now back to this issue of age and birthdate.
So we can take the path of renaming the age column to birthdate and changing the data type from integer to datetime…
But that’ll be a weird situation because the database has to reconcile what to do with the data that’s already in the database. 
Our local data is just play data so there isn’t much damage done. But whatever you do in your development environment for database schema should be the same in your production environment.

But let’s just say for now it’s safe to just burn the age column down and add the birthdate column.
Let’s feed two birds with one scone.

First, let’s generate the script
rails g migration ChangeDogAgeColumns

Now let’s look at the documentation to see what can use.
Changing Tables ….Changing Columns….
We’re changing many columns on a table so I’ll go with Changing Tables to see what options for migration definitions we have

change_table :dogs do |t|
  t.remove :age
  t.datetime :birthdate
end

DateTime.now

rails c to verify


