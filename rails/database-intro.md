What is a database?
A database is a tool for storing and organizing information.

Well how is that different from using spreadsheets?
The primary differences between the two are:
How the data is stored and manipulated
Spreadsheet: using spreadsheet software that has a focus on providing a user the ability to perform analysis on data
Database: stored using database software (database management systems - DBMS) has a focus on reading and writing huge amounts of data

Who can access the data
Spreadsheet: pretty much if you can create a spreadsheet, you have access to it
Database: have user/role systems to restrict users from being able to perform certain actions (for example, if there’s an app that stores “production data”, often times, developers are not given access to that data and only specific administrators have privileges around being able to actually interact with data)
How much data can be stored
Spreadsheets: you can probably have 3000 rows in your spreadsheet but it’ll likely take a long time to load 
Databases: are built to support enormous amounts of data and more importantly the ability to read and update that data
Why use a database?
Not all projects require a database. 
You’ve built tic-tac-toe which is more or less a static website. But if you want support the ability for someone to log in with a user account, be able to see all the games they’ve won or loss and be able to view people they’ve played with, there is a lot of data there that needs to persist through time. It sorta just need to live somewhere until it needs to be accessed. 
For most web apps, there’s a need to store data about users, so having a database becomes a crucial part of a full stack app.
Where do databases live?
Database instances live on computers/servers. We’ll be creating database servers on our individual computers. Just like instances of a class, they’re not the same unless you’re referencing the same database/computer.  So I can create a database named Animals on my computer. Sarah can create a database named Animals on her computer. We cannot expect that they will have the same data because they are different instances.

Types of Databases
Relational Database: stores information into tables that consist of rows and columns and relationships are created between different tables; each row represents a record (instance) of that table
Non-relational (NoSQL) Database: stores information in documents  which is a different data structure

Database Schema
Organizational blueprint of the database (what tables there are and what columns exist for each table)

SQL
Structured Query Language, domain-specific language for communicating with relational databases
SQL allows you to ask the database to create, read, update, delete data

Postgres (PostgreSQL) is a flavor of SQL (mySQL, SQL Server, T-SQL) are all just version of SQL that have their specialty. 
Postgres is an open source relational database management system that is often used with Rails projects. The choice between the databases is really comes down to the servers being used deploy the app and what flavor of SQL is supported. The tool we use to host and serve our Rails apps support PostgreSQL databases.


How Programs Use Databases
Let’s say that we have some data about a Person
First Name:     Korben
Last Name:      Dallas
Date of Birth:  6/1/12097
Address:        Apt 3497 1 Main St CityVille, Earth
Email:          korben_dallas@hotmail.com

In Ruby, we can store this information in an object with key value pairs.
person1 = {
  "first_name" => "Korben",
  "last_name" => "Dallas",
  "birthday" => "6/1/1209",
  "address" => "Apt 3497 1 Main St CityVille, Earth",
  "email" => "korben_dallas@gmail.com"
}
This works fine for one instance but how do we ensure that a Person is always going to have some data for each of these keys/properties?
We build a class. That blueprint for the information a Person should always have
class Person
  def initialize(fname, lname, birthday, address, email)
    @fname = fname
    @lname = lname
    @birthday = birthday
    @address = address
    @email = email
  end
end

Person.new("Korben", "Dallas", "6/1/1209", "Apt 3497 1 Main St CityVille, Earth", "korben_dallas@gmail.com")

So now that we have a systematic way of holding the data for a Person, we use an ORM (Object Relational Mapper) which transforms this instance of a Person into SQL so that we can ask the database to store this data. 
When the data makes its way the database, it gets stored into tables with columns that map with the keys in the object. Each row is a record or instance of the table named Person. 

A collection of instances or records is a table or a relation.

When a record is created on a table, the database generates an id for it. Sometimes it follows a sequence 1, 2, 3 sometimes it looks like ‘asdf-234d-2343-sfdf’. GUID/UUID (globally unique identifier) The point is that the id is unique. 
Why? 
Because let’s say you have to two people both with names Leeloo Dallas, same birthdate… but that they’re actually different people. How will the database know they’re different? That unique identifier. This use of a unique identifier to distinguish the different records is a mechanism called Primary Key.


