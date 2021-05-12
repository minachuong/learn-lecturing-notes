# Loops

[Link to Loops slides](https://docs.google.com/presentation/d/1pv9hkvPYZzM-q-zmTqS86vxDeZMGO_4lgMciGXaD_5o/edit?usp=sharing)

A loop is a fundamental programming structure that allows us to repeat the execution of code 

### Why Loops?
Say you're following a recipe for scrambling eggs. But even if you've made scrambled eggs a hundred times, the outcome likely won't be exactly the same every time. Humans naturally are just not consistent. Computers however are very good at being consistent.

### Anatomy of a For-loop
There are many different types of loops but a for-loop is the most common one you'll encounter in the beginning. 

A for-loop is declared with a `for` reserve word. A reserve word is just something in a language that is reserved for a special meaning so that it gets interpreted in a very specific way. Just like `let`, `const`, `var`, `if`, `else`, etc. 


## Live-coding
```javascript

// basic loop using an incrementor
for(let i=0; i<5; i++){
  console.log(i);
}

// logic without console.log
for(let i=0; i<=3; i++) {
  i + 3;
}

// basic loop using a decrementor
for(let i=10; i>0; i--){
  console.log(i);
}

// infinite loop
for(let i=0; i>0; i++){
  console.log(i);
}

// infinite loop
for(let i=1; i>0; i++){
  console.log(i);
}

// logic that doesn't depend on using index
for(let i=0; i<10; i++) {
  console.log("HELLO!");
}

// scope
let letters = "Churro";

for(let i=0; i<letters.length; i++){
  console.log(letters[i]);
}

// using an array
let falalalas = ["fa", "la", "la", "laaaaa"];

for(let i=0; i<falalalas.length; i++){
  console.log(falalalas[i]);
}

// iterating through an array
let evenNumbers = [0, 2, 4, 6];

for(let i=0; i<evenNumbers.length; i++){
  console.log(evenNumbers[i] * 12);
}

// but does it change the array?
// console.log(evenNumbers);

let someMoreNumbers = [0, 2, 3, 0, 4, 5, 6, 0];

for(let i=0; i<someMoreNumbers.length; i++){
  if (someMoreNumbers[i] !== 0) {
    console.log(someMoreNumbers[i]);
  }
}

```