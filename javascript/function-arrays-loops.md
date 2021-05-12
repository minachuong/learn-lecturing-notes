In this lecture we’re going to be putting together pieces of code you’ve learned. 
First! Quick pop quiz:

What is a function?
<<A programming structure that: holds set of instructions in a package.  When a function is invoked or called upon, only then will be set of instructions be executed. Functions can have inputs (through use of parameters) and outputs. Bonus: What is the benefit in using a function? Making code reusable!>>

What is a loop?
<< A programming structure that: The process of repeating the execution of a set of instructions. Bonus: What makes a loop stop execution?>>

What is an array?
<<A collection of data that has an order>>
<<An array is also a data structure>>
A data structure is organization of data. There are many types of data structures. Arrays are fundamental because it organizes data in an orderly way.

Think of these three kinds of things as different pieces of Legos.
So it’s like when do I use a narrow long strip? When do I use a round one? When do I use a thick cube? 
As you’re programming you’ll pretty much be needing to ask yourself the same kind of things:

For example, I have a block of code that gets the number of characters in a name, and multiplies it by 10 and logs the final value.
I want those instructions to be performed at different times through my program. 
That means I want that code to be reusable —so how do I make code reusable? I put it in a function.


Okay, so let’s just start jamming those Legos together and see what we get.

PSEUDOCODE!

Here’s our first goal:
// Create a function, given a list of names, that returns a list of name tag text.

We’re given a list of names and we need to generate the text printed on name tags that includes the name.
So for example, for a name like “Churro", the printed text should look like, “Hi! My name is Churro!”

So here, we’re asking to create a function and anytime we’re thinking about creating a function, we need to decide what the inputs and outputs are.

// Input: 
When we’re thinking about inputs, we need to decide what the data type of the input(s) are.
So because I’m being given a list of names, what kind of structure can best help us organize those names?
>> ARRAY
// Input: array
What is the data type of the values this array will hold?
// Input: array of strings 

So I’m feeding my function, my little machine, a list of names.
I’m also looking for a list of things to come out of the machine.
// Output: array of strings 
What is the data type of the output? ARRAY
What is the data type of the values within this array? strings

Okay so at this point, we know what’s going into the function, and what’s coming out of the function.
Now we have to build the instructions for how we’ll transform the list of names into a list of name tag greetings.

So again, it’s easier to build functions when you can visualize the data you’re working with.
So what is an example of an input?
// Input: [“Churro”, “Mina”, “Patrick"]
What do we expect the output to look like?
// Output: [“Hi! My name is Churro!”, “Hi! My name is Mina!”, “Hi! My name is Patrick!”]

With pseudocoding, we’ll just type out everything we need to do in English then translate it into code. 
I’ll also leave some space between my pseudocode so that I can write the code right beneath it. 
For note-takers out there, I also suggest leaving some space between these lines.
// declare a function that takes one parameter


// initialize a variable to store the list of name tag greetings


// for each name


// create a greeting with the name


// store the greeting into the variable


// return the list of greetings



What’s a good function name?
We want to describe what the function does.
What describes the data inputted into the function?
// create a function that takes one parameter
const makeNameTagGreetings = (names) => {

. . .
}

What are we building?
// initialize a variable to store the list of name tag greetings
let nameTagGreetings = [];

How do we access each name in the list repeatedly?
We use a for-loop.
// for each name
for (let i=0; i<names.length; i++) {
    // names[i]
}


How do we create that greeting? “Hi! My name is Mina!”
string interpolation!
for (let i=0; i<names.length; i++) {
    // names[i]
    // create a greeting with the name
    `Hi! My name is ${names[i]}!`
}


How do we store the newly created greeting into our list container?
.push() - built-in method
for (let i=0; i<names.length; i++) {
    // names[i]
    // create a greeting with the name
    // `Hi! My name is ${names[i]}!`
    // store the greeting into the variable
    nameTagGreetings.push(`Hi! My name is ${names[i]}!`)
}

When the loop is done, it will have create a greeting for each name in the given list.
  // return the list of greetings
  return nameTagGreetings
}

So let’s run through this a little bit to show how the data/inputs are being used within the function.
<<DO CODE EVALUATIONS THROUGHOUT THE FUNCTION>>

Now let’s invoke the function!
console.log(makeNameTagGreetings([“Churro”, “Mina”, “Patrick"]))

Let’s try another list of inputs
var bravo = [“Diego”, “Jonathan”, “Hex”, “Kevin”, “Shazeen”, “Vivean”, “Chris”, “Raymond”, “Erik”, “Guillermo”, “Marcus”]
console.log(makeNameTagGreetings(bravo))



Let’s do another example of using functions, arrays, and loops.

Prompt: Given a list of numbers, create a function that returns another list of numbers, each multiplied by 1000.

// Input: array of numbers
// Output: array of numbers

// create a function that takes a list of numbers
// initialize a variable to store the new list of numbers
// for each number
// multiply the number by 1000
// store into the new list of numbers
// return the new list of numbers
const timesOneThousand = (originalNumbers) => {
    let newNumbers = [];
    for (let i=0; i>originalNumbers.length; i++) {
        newNumbers.push(originalNumbers[i] * 1000);
    }   
    return newNumbers;
}


Prompt: Given a list of numbers, create a function that returns only the odd numbers




OVERVIEW:
* Before writing any code, we decided on the inputs and outputs of a function and determined what their data types would be.
* Before writing any code, we wrote the pseudocode! This is our game plan and strategy. No strategy or gameplan often leads to madness.
* After writing out pseudocode, we translate the pseudocode into Javascript code.


















