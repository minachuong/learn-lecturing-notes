# Inheritance

[Link to slides](https://docs.google.com/presentation/d/1F1MUYt5gDKM3rgwo7TOlSb9ExttgG_PZ6PU_zZxqzKI/edit?usp=sharing)

Object Oriented Programming (OOP) - It is a way of programming that allows developers to describe real word things through the use of objects. 

We’ve already started doing that with the use of classes. 

One of the main OOP concepts is Inheritance.
What problem is Inheritance solving?
 >> The problem is have duplicate code between classes! 
 >> show class with similar methods
 Again it’s that reuse of code! Develop things to avoiding creating duplicate code!

Inheritance is much like how it’s used in real life. 
Biologically, I inherited genes from my parents, so I have many of the same traits as them (two eyes, nose, mouth, black straight hair, brown eyes, split toe nail on my little toe)
In programming, inheritance is the process of one class inheriting properties and methods from another (parent) class.

The way to think about the relationship between two classes created through inheritance is thinking about an “IS A” relationship. For instance, a Banana IS A Fruit. The Banana class is the child class and the Fruit is the parent class. Bananas are a subset of Fruits. A Fruit IS A Food.     

In code what does that look like
In JS the keyword extends is used to signify the inheritance

 child class       IS A       parent class

```javascript
class Fruit extends Food {
    constructor() {
        super();
        this.hasSeeds = true;
    }
}
```

The moment we indicate inheritance through “extends”, the constructor will require the super() method to be called. In fact, it will throw a ReferenceError: Must call super constructor in derived class before accessing ’this’

so the first line within our constructor block must be super() if it’s inheriting from a class. 

why do we have to call the super() method? 
super() is a  function that calls the constructor of the parent class. Recall the constructor method gets called when we create an instance. 
so let’s check out what’s going on when we create an instance of Fruit
>> demonstrate inception
                 
>> console log of Food and Fruit (without its own properties)
>> add this.hasSeeds = true; 

//Inherited Class methods
Just like how properties are inherited, methods are inherited too
declare grow and mold methods and logGrowState() on Food
then call those methods on Fruit

Single Inheritance (Fruit is a Food) … Multi level inheritance (Banana is a Fruit, Fruit is a Food)

// If a parent class requires an argument in the constructor, all subsequent child classes will need provide that argument
>> add a required parameter for Fruit to indicate type
>> initialize the banana without passing 

