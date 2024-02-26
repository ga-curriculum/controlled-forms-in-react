# ![Controlled Forms in React - Managing Controlled Forms](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to use a form to control state in a React component.

## Adding an Input Form to the `App` component

To get started, add the following to `App.jsx`: 

```jsx
const App = () => {

  return (
    <>
      <h2 id='full-name'>The full name will appear in this header.</h2>
      <form>
        <label for="first-name">First Name: </label>
          <input id="first-name" />
      </form>
    </>
  );
}

export default App
```

The header will be where the text from our form fields are rendered on the page. The form will contain a single text input. We'll modify the form and input as our needs change, but this is a good starting place.

## Add the `useState` hook to our component

One we have the form rendered we can type values into it. Note that our component has no way of keeping track of the values that we enter, so our next step will begin to address this issue.

In order to begin to track the value that is being input, we need to establish state. We will first import the `useState` hook and declare a variable called "firstName".

`App.jsx`:
```jsx
import { useState } from ‘react’; // importing the useState hook

const App = () => {

  const [firstName, updateFirstName] = useState(''); // declaring state with an empty string
  
  return (
    <>
      <h2 id='full-name'>The full name will appear in this header.</h2>
      <form>
        <label for="first-name">First Name: </label>
          <input id="first-name" />
      </form>
    </>
  );
}
```

## Connect state and handling to our input field

Once we have state in place, we can add a "value" prop to our input form and set the value of that prop to `firstName`. 

Inside of our input field, we'll add a "value" prop to grab our value from state:

```jsx
    <>
      <h2 id='full-name'>The full name will appear in this header.</h2>
      <form>
        <label for="first-name">First Name: </label>
          <input 
            id="first-name"
            // Add the following: 
            value={firstName} 
          />
      </form>
    </>
```

Next, we will define a function to help us change state from our input field:

```jsx
const App = () => {

  const [firstName, updateFirstName] = useState('');

  // When invoked, update firstName state to equal the new value (taken from the event)
  const handleFirstNameChange = (e) => {
    updateFirstName(e.target.value);
  }

  return (
    <>
      <h2 id='full-name'>The full name will appear in this header.</h2>
      <form>
        <label for="first-name">First Name: </label>
          <input 
            id="first-name"
            value={firstName} 
            // When the input changes, call the event handler
            onChange={handleFirstNameChange}
          />
      </form>
    </>
  );
}

export default App
```

Confirm that the changes you have implemented work by typing a value into the input field. With your dev tools open to the React Components tab ( and highlighting the App component ) you should be able to see the value you typed in reflected in the state of the component!

## Repeat the Process for Last Name

Now that we now have the ability to track the value of our input field via state, let’s repeat that process for a last name as well. In order to do this we will need to:

1. Create state for the last name
2. Create a function to handle the state change for that last name
3. Connect this logic to a new input field
4. Test it out and make sure that you can track to elements of state for first name and last name

Once done your App component should look like this:

```javascript
import { useState } from 'react';

const App = () => {

  const [firstName, updateFirstName] = useState('');
  const [lastName, updateLastName] = useState('');

  const handleFirstNameChange = (e) => {
    updateFirstName(e.target.value);
  }

  const handleLastNameChange = (e) => {
    updateLastName(e.target.value);
  }

  return (
    <>
      <h2 id='full-name'>The full name will appear in this header.</h2>
      <form>
        <label for="first-name">First Name: </label>
          <input
            id="first-name"
            value={firstName}
            onChange={handleFirstNameChange}
          />
        <label for="last-name">Last Name: </label>
          <input 
            id="last-name"
            value={lastName}
            onChange={handleLastNameChange}
          />
      </form>
    </>
  );
}

export default App

```
Test that you can update your state in the same manner that you did with the first input field.  If you can, you are ready to move forward.


## Time for a refactor.

You may have noticed that our code has some repetition in terms of functionality.  This presents us with an opportunity to refactor our state to simply one item to track and also create a more reusable function to be applied to multiple fields.  First, let’s change our state to the following:

```javascript
const [fullName, updateFullName] = ({ firstName: ‘’, lastName: ‘’ });
```

We now have an object that can be used in both of our inputs.  We are also adding a "name" attribute to our inputs.  This is crucial because the value in our name attribute will become the "key" of our objects when we refactor the function that handles the change in the input:

```javascript
<form>
  <label>
    First Name:
    <input
      name='firstName'
      value={ fullName.firstName }
      onChange={ handleFirstNameChange }
    />
  </label>
  <label>
    First Name:
    <input
      name='lastName'
      value={ fullName.lastName }
      onChange={ handleLastNameChange }
    />
  </label>
</form>
```

With our form inputs refactored, let’s refactor our handler functions into one function that can be passed to both inputs:

```javascript
const handleChange = (e) => {
  updateFullName({ ...fullName, [e.target.name]: e.target.value });
}
```

**Note:** A couple of things are happening here:

1. We are using the spread operator to copy any values from the original state into a new object for state.
2. `e.target.name` is taking the value of the name prop in our input form
3. `e.target.value` is acting the same as our previous functions and placing the values we type with said keys.

We can now use this one function in our onChange handlers.


## Add the submit button.

We have the ability to hold these values in state but we don’t do anything.  Our last step will be to add a submit button, connect it to a mock "submit" function, and reset our state

Let’s add a submit button to our form:

```javascript
<input type='submit' value='Submit Your Name' />
```

When we click the button the page will refresh so let’s create a function to attach to a submit handler and prevent that behavior.

```javascript
const handleSubmit = (e) => {
  e.preventDefault();
  console.log('the page does not refresh now');
}
```

Next, we’ll add it to a submit handler in the opening form tag:

```javascript
<form onSubmit={ handleSubmit }>
```

Once in place, navigate to the console tab of your dev tools, click the submit button, and see if you can view the message without a page refresh.  If so, we are ready to modify this function:

```javascript
const handleSubmit = (e) => {
  e.preventDefault();
  const header = document.getElementById('full-name');
  header.innerText = `Your name is: ${fullName.firstName} ${fullName.lastName}`
  updateFullName({ firstName: '', lastName: '' });
}
```

Now, our function grabs our header element, modifies the innerText of it to include the firstName and lastName found in state, and updates the state to empty strings which clears the form as well.

Your full `App.jsx` should look like this:

```javascript
import { useState } from 'react';

const App = () => {

  const [fullName, updateFullName] = useState({ firstName: '', lastName: '' });

  const handleChange = (e) => {
    updateFullName({ ...fullName, [e.target.name]: e.target.value });
  }

  const handleSubmit = (e) => {
    e.preventDefault();
    const header = document.getElementById('full-name');
    header.innerText = `Your name is: ${fullName.firstName} ${fullName.lastName}`;
    updateFullName({ firstName: '', lastName: '' });
  }

  return (
    <>
      <h2 id='full-name'>The full name will appear in this header.</h2>
      <form onSubmit={ handleSubmit }>
        <label>
          First Name:
          <input
            name='firstName'
            value={ fullName.firstName }
            onChange={ handleChange }
          />
        </label>
        <label>
          First Name:
          <input
            name='lastName'
            value={ fullName.lastName }
            onChange={ handleChange }
          />
        </label>
        <input type='submit' value='Submit Your Name'/>
      </form>
    </>
  );
}

export default App
```