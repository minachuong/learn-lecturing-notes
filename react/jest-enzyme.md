Testing:
Quick survey: Why do testing?
-Builds confidence in your code: make changes without breaking existing behavior and coverage for different use cases. 
-Builds a living documentation for your product/application. It should set expectations for behaviors of the app.
-Reduces having to do manual/functional regression. Functional regression: I was working on adding functionality to a button component that’s used at least 20 times throughout an app. I checked that the button still works correct in a couple of places, but it might be maddening to have to check all 20+ places and perform all the hover, click functionality to make sure it work as expected in all places where the button is used. 
-Sometimes used as a measure of completion for a task
-Testing is like a net to catch bugs


Component Unit Testing
Each component has their own test file that will contain expectations for how the component should behave.
Keyword: Behave…  we’ll be writing tests that are concerned with behavior and not implementation.

When you create a React app using 
yarn create react-app <app-name>
it will create the app with React Testing Library and Jest as a dependency out of the box.
show dependency in package.json: '@testing-library/react/‘

Jest (maintained by people at Facebook): Testing framework composed of libraries: Test runner & Assertion library & Mocking library:
-test runner: ‘yarn test’ does the actual running of test code, provides an interface for you to see what tests are passing/failing
-assertion library: provides language for writing tests: describe(), it(), test(), expect(), toBe(); matcher functions
-mocking library: provides language creating and using mocks; what is mocking? It’s a technique in testing usually used to prevent things like making real api calls to external frameworks from happening. 

README: Create React Apps use Testing Library: a testing library that can be used on a variety of different Javascript projects. We’re using the React version since we have a React app. 

Examine App.js component. Start the app. View the landing page.
SHOW: package.json 
Note:
* @testing-library/jest-dom  
* @testing-library/react
* @testing-library/user-event

Note: scripts: “test”:
yarn test

Let’s go ahead and take a look at the test written for us.
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
    // render() from the @testing-library/react
    // providing it an argument that is a component call
    // this loads the screen variable so that you can query elements 
    // arrange
    render(<App />);

    // act 
    const linkElement = screen.getByText(/learn react/i);
    // user interaction here is that the user is looking for an element with some text “learn react"
    // and later on, it’ll be something like when the user clicks a button, scrolls, etc

    // assert 
    // want to set the expectation that we should see something when the app loads
    expect(linkElement).toBeInTheDocument();
});

console.log the screen to see what is rendered
// before linkElement
console.log(screen.debug())


But this process of using render() doesn’t exactly scale well especially when there are a lot of nested components. We don’t want to have to render all components if we’re only concerned with testing one component. 

README: Enzyme is another testing library that does some of the things that React Testing Library does but it follows a different philosophy on how to test but also it does “shallow rendering"

README: What is shallow rendering?
Renders only the first layer of a component. 
So when there are nested components (we don’t need to see or assert on all the internals of child components, we just want to make sure that certain child components are there)


Installation
yarn add -D enzyme react-test-renderer enzyme-adapter-react-16
yarn test

Boilerplate code

// Imports React into our test file.
import React from 'react' 


// Imports Enzyme testing and deconstructs Shallow into our test file. 
import Enzyme, { shallow } from 'enzyme' 


// Imports Adapter utilizing the latest react version into our test file so we can run a testing render on any component we may need.
import Adapter from 'enzyme-adapter-react-16' 


 // Imports in the component we are going to be testing. 
import App from '../App'


//Allows us to utilize the adapter we import in earlier, allowing us to call and render a component. 
Enzyme.configure({ adapter: new Adapter() })




Rewrite the test with Enzyme.





=======================================




Enzyme: shallow(): allows us to render an object in memory instead of in the DOM (make it faster)
Wraps the rendered component in a wrapper which gives us functions to examine rendered component
wrapper allows us to search through the elements rendered using css selectors .find()
.text(): returns the text node (innerhtml) of node

What are mocks? 
They are pretend components that represent actual components. Let’s say you wrote a component that makes an api call to NASA to get some images like everyone did yesterday whether you created that in App.js or were able to migrate that to a child component. 

.simulate(): simulate dom events 




Behavior driven development

in the context of testing:
stubs (fake data) vs mocks (fake components that carry behavior)
spies?

jest.fn() ?

example of testing implementation (asserting on state) vs testing behavior (asserting on dom)

