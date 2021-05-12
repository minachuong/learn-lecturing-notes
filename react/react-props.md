# React Props

[Link to slides](https://docs.google.com/presentation/d/1MMbt-CEBYFjC_nxaCee3p99RbWPUx-CqrqNZziKk72A/edit?usp=sharing)

Data flow in React

Last week, you learned about how we can create components in React.

The purpose of creating components is for grouping some behaviors together for reuse. Rule of thumb here is anytime you catch yourself having to copy and paste more than twice — it’s time to put that logic into a component.

<npx create-react-app props-example>  
`$ yarn start`

App component is created for us 
delete the filler stuff in App component
But we can create other components. 
create a directory named components in src/

Let’s say it’s a Greeter component that when rendered says "Hi Alpha 2021!”
```javascript
class Greeter extends Component {
    render () {
        return ( <h1>Hi Alpha 2021!</h1>)
    }
}
export default Greeter;
```
Then we can make a component call in our App component to consume/use that component. 

Syntax for a component call
`<Greeter />`
Make sure to import Greeter!

This is pretty much the same as instantiating the component class or creating an instance of that component class.
At this point, the moment we make a component call inside of another component we’re creating parent-child relationships between the components. Here: App component is the parent and Greeter component is the child. The child component is usually referred to as a nested component.

We can call it however many times we want.
```
<Greeter />
<Greeter />
<Greeter />
```

But it’s not that dynamic because the data for each instance is the same. The data I’m referring to is “Hi Alpha 2021!”. It’s been hard-coded into the component. 

So how do we make it more dynamic?
Like what if I wanted it to say “Hi Sarah!” and “Hi Fernando!” and “Hi Steph!”

What is the thing that needs to be dynamic here? The only thing that is really changing here is the name. The string output is really “Hi, name!” 
So here, the responsibility the Greeter component has is really just to render the h1 tag with “Hi, name!” but it needs to get that name from somewhere. That’s where the parent comes it. Because the parent is the one making all these component calls it should know what the child needs. We can have a parent tell the child what the name is through a mechanism called “props” (short for properties).

```javascript
<Greeter name=“Sarah” />
<slide break down of prop>
```
This looks a lot like attributes in HTML with the same syntax for providing the value for that attribute which can pretty much be view in the same light as a key-value pair. Only now, instead of there being pre-defined HTML attributes, we’re deciding what those attributes are and what they’re named.

Within the component we need to access that data through `this.props`. `this` is the scope of the instance and .props is a class property that is an object that React will always provide on the instance of the component so that we can get data from a parent.

Every “attribute”/“prop” now becomes a property/key on this.props
So in the Greeter class, instead of “Alpha 2021” I can use JSX provide curly braces and access the name through this.props.name

<slide breakdown of parent and child side by side>

You’ll notice that only Sarah’s name gets rendered because we’re only passing props on the first Greeter instance.

So let’s 
<add names to the other components>

Verify change.

But this data can be organized in some way right? Like what we really have here is a list of names. What data structure would help organize this? 
An array!
And we can put the list in state and reference it using array index-accessor.
vip: [
"Sarah",
"Fernando",
"Steph",
"Justin",
"Allen",
"Raul",
"Angelo",
"Deven",
"Elyse",
"Guerrero",
"Kevinn",
"Lex",
"Nick"
]

Alpha Party up in here!
—
<Greeter name={this.state.vip[0]} />...
But again there’s still a good amount of redundancy and hard-coding here.
This is where that beautiful high order function called .map() becomes handy.
.map(person => <Greeter name={person} />)

CHECK/SHOW for KEY ERROR MESSAGE

React needs to be able to uniquely identify different instances of a component rendered at the same time so that’s why it complains about a key in the error message. So we’ll just use the index to resolve this.
.map((person, index) => <Greeter name={person} key={index} />

A big thing to note here is the direction of data. It is always flowing from parent to child. Down the component tree.
<slide: Down the component tree>
You can pass any kind of data down. 
strings, numbers, booleans, objects, arrays, arrays of objects

You can also pass functions down! This is a way of the parent telling the child how to behave. 

Let’s dress this up a bit because things are about to get fancy
style={{border: '1px solid black', margin: '20px', padding: '20px'}}


Let’s say there’s a raffle happening at this party
And we want the Greeter component to help the party guest select a raffle number when a button is clicked.

So let’s define a method in our App component that generates a random number. 

We’ll create a prop of raffleAction and pass the method. Notice I’m not including any parentheses. What I’m doing is providing a function pointer or a reference to the function without actually executing it. The reason we do this is because the calling of this method shouldn’t happen until a button is clicked. If I provide parentheses, when the page loads that method will be called right way. 

raffleAction={this.pickRaffleNumber}
 
now the child is going to use that method to get the raffle number and store that raffleNumber to state.

so we need to create a method in the child 
getRaffleNumber = () => {
const newRaffleNumber = this.props.raffleAction();
this.setState({raffleNumber: newRaffleNumber});
}
Note that I’m using arrow function here instead of class method declaration. That’s because I need to access `this` within the function and I can do that with the arrow function. Without using the arrow function, React components compile in a way where the method doesn’t where know its this is. 

now the child is going to need a button
<button onClick={this.getRaffleNumber}>
Pick a raffle number
</button>

Now I could have just put the method in the Greeter component but what if the parent component also wants to use the same method to pick the winning number. Now the behaviors are shared.

console.log
DEBUGGER!

