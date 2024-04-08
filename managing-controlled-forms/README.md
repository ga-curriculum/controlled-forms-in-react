# ![Controlled Forms in React - Managing Controlled Forms](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to use a form to control state in a React component.

## Adding an Input Form to the `App` component

To get started, add the following to `App.jsx`: 

```jsx
import { useState } from ‘react’; // import the useState hook

const App = () => {
  // Add placeholder text for the title - we'll use the form to update this state by the end of this lesson
  const [title, setTitle] = useState('The full name will appear here.')

  return (
    <>
      <h2>{title}</h2>
      <form>
        <label htmlFor="firstName">First Name: </label>
          <input id="firstName" />
      </form>
    </>
  );
}

export default App
```

The header will be where the text from our form fields is rendered on the page. The form will contain a single text input. We'll modify the form and input as our needs change, but this is a good starting place.

## Add the `useState` hook to our component

Once we have the form rendered, we can type values into it. Note that our component has no way of keeping track of the values we enter, so our next step will address this issue.

To begin tracking the value being input, we need to establish state. We will first import the `useState` hook and declare a variable called "firstName".


```jsx
import { useState } from ‘react’; // importing the useState hook

const App = () => {

  const [title, setTitle] = useState('The full name will appear here.')
  const [firstName, updateFirstName] = useState(''); // declaring state with an empty string
  
  return (
    <>
      <h2>{title}</h2>
      <form>
        <label htmlFor="firstName">First Name: </label>
          <input id="firstName" />
      </form>
    </>
  );
}
```

## Connect state and handling to our input field

Once we have state in place, we can add a "value" prop to our input form and set the value of that prop to `firstName`. 

Inside our input field, we'll add a "value" prop to grab our value from state:

```jsx
    <>
      <h2>{title}</h2>
      <form>
        <label htmlFor="firstName">First Name: </label>
          <input 
            id="firstName"
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

  // When invoked, update the firstName state to equal the new value (taken from the event)
  const handleFirstNameChange = (e) => {
    updateFirstName(e.target.value);
  }

  return (
    <>
      <h2 id='full-name'>The full name will appear in this header.</h2>
      <form>
        <label htmlFor="firstName">First Name: </label>
          <input 
            id="firstName"
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

Confirm that your implemented changes work by typing a value into the input field. With your dev tools open to the React Components tab ( and highlighting the App component ), you should see the value you typed in reflected in the component's state!

## Repeat the Process for the Last Name

Now that we can track the value of our input field via state, let’s repeat that process for a last name. To do this, we will need to:

1. Create state for the last name
2. Create a function to handle the state change for that last name
3. Connect this logic to a new input field
4. Test it out and make sure that you can track the elements of state for first name and last name

Once done, your App component should look like this:

```jsx
import { useState } from 'react';

const App = () => {

  const [title, setTitle] = useState('The full name will appear here.')
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
      <h2>{title}</h2>
      <form>
        <label htmlFor="firstName">First Name: </label>
          <input
            id="firstName"
            value={firstName}
            onChange={handleFirstNameChange}
          />
        <label htmlFor="lastName">Last Name: </label>
          <input 
            id="lastName"
            value={lastName}
            onChange={handleLastNameChange}
          />
      </form>
    </>
  );
}

export default App

```
Test that you can update your state like you did with the first input field. If you can, you are ready to move forward.


## Time for a refactor.

You may have noticed that our code has some repetition in terms of functionality. This gives us an opportunity to refactor our state to simply one item to track and create a more reusable function to be applied to multiple fields. First, let’s change our state to the following:

```jsx
const [title, setTitle] = useState('The full name will appear here.')
// Replace firstName and lastName state with the following: 
const [fullName, setFullName] = ({ 
    firstName: '', 
    lastName: '' 
  });
```

We now have an object that can hold state for both inputs. We'll also add a `name` attribute to our inputs, as we'll use this value in the next step: 

```jsx
<form>
  <label htmlFor="firstName">First Name: </label>
    <input
      id="firstName"
      name="firstName"
      value={fullName.firstName}
      onChange={handleFirstNameChange}
    />
  <label htmlFor="lastName">Last Name: </label>
    <input
      id="lastName"
      name="lastName"
      value={fullName.lastName}
      onChange={handleLastNameChange}
    />
</form>
```

With our form inputs refactored, let’s also refactor our handler functions into a single function that can be passed to both inputs:

```jsx
// Remove this function: 
const handleFirstNameChange = (e) => {
    updateFirstName(e.target.value);
}
// Remove this function: 
const handleLastNameChange = (e) => {
    updateLastName(e.target.value);
}

// Replace them with the following: 
const handleChange = (e) => {
  setFullName({ ...fullName, [e.target.name]: e.target.value });
}
```

**Note:** A couple of things are happening here:

1. We are using the spread operator to copy any values from the original state into a new object for state. This way, when updating the `firstName` state, we're not losing `lastName` state, and vice versa! 
2. `e.target.name` is taking the value of the name prop in our input form. We're able to dynamically access the key through the use of square bracket notation. If the user types into the `firstName` input, then the `name` of that input is used as the key and the `value` of that input is used as the value. 

For a more visual example, typing the name "Sam" into the first name input would result in an event handler that resolves to the following: 
```jsx
// First letter typed ("S")
setFullName({...fullName, firstName: 'S'})
// Second letter typed ("a")
setFullName({...fullName, firstName: 'Sa'})
// Third letter typed ("m")
setFullName({...fullName, firstName: 'Sam'})

// With our state looking like this afterward: 
{
  firstName: 'Sam',
  lastName: ''
}
```

Note that because we are spreading all of our existing state into the new state object, we don't lose the `firstName` property's value even when we call the handler from our last name input: 

```jsx
// First letter typed ("V")
setFullName({...fullName, lastName: 'V'})
// Second letter typed("o")
setFullName({...fullName, lastName: 'o'})

// Updated state: 
{
  firstName: 'Sam',
  lastName: 'Vo'
}
```

As a result of this dynamic behavior, we can use this one function in both of our `onChange` handlers! 

Let's go ahead and change those now: 

```jsx
<form>
  <label htmlFor="firstName">First Name: </label>
    <input
      id="firstName"
      name="firstName"
      value={fullName.firstName}
      // Update to accept handleChange
      onChange={handleChange}
    />
  <label htmlFor="lastName">Last Name: </label>
    <input
      id="lastName"
      name="lastName"
      value={fullName.lastName}
      // Update to accept handleChange
      onChange={handleChange}
    />
    <input type='submit' value='Submit Your Name' />
</form>
```

## Add the submit button

We can hold these values in state, but we can't do anything with them yet. Our last step will be to add a submit button, connect it to a mock "submit" function, and reset our state! 

Let’s add a submit button to our form:

```jsx
<form>
  <label htmlFor="firstName">First Name: </label>
    <input
      id="firstName"
      name="firstName"
      value={fullName.firstName}
      onChange={handleChange}
    />
  <label htmlFor="lastName">Last Name: </label>
    <input
      id="lastName"
      name="lastName"
      value={fullName.lastName}
      onChange={handleChange}
    />
    <input type='submit' value='Submit Your Name' />
</form>
```

When we submit a form, the page will refresh by default, so let’s create a function we can attach to the submit handler that prevents that behavior. Add the following function before the return: 

```jsx
const handleSubmit = (e) => {
  // Prevent default event behavior
  e.preventDefault();
  console.log('the page does not refresh now');
}
```

Next, we’ll add it to an `onSubmit` handler in the opening form tag:

```jsx
<form onSubmit={handleSubmit}>
```

Once in place, navigate to the console tab of your dev tools, click the submit button, and see if you can view the message without a page refresh. If so, we are ready to make our final adjustments: 

```jsx
const handleSubmit = (e) => {
  e.preventDefault();
  // Set our title state based on the fullName state at the time of submission
  setTitle(`Your name is: ${fullName.firstName} ${fullName.lastName}`)
  // Reset fullName state to clear our form inputs
  setFullName({ firstName: '', lastName: '' });
}
```

Your full `App.jsx` should look like this:

```javascript
import { useState } from 'react';

const App = () => {a
  
  const [title, setTitle] = useState('The full name will appear here.')
  const [fullName, setFullName] = useState({ firstName: '', lastName: '' });

  const handleChange = (e) => {
    setFullName({ ...fullName, [e.target.name]: e.target.value });
  }

  const handleSubmit = (e) => {
  e.preventDefault();
  setTitle(`Your name is: ${fullName.firstName} ${fullName.lastName}`)
  setFullName({ firstName: '', lastName: '' });
}

  return (
    <>
      <h2>{title}</h2>
      <form onSubmit={handleSubmit}>
        <label htmlFor="firstName">First Name: </label>
          <input
            id="firstName"
            name="firstName"
            value={fullName.firstName}
            onChange={handleChange}
          />
        <label htmlFor="lastName">Last Name: </label>
          <input
            id="lastName"
            name="lastName"
            value={fullName.lastName}
            onChange={handleChange}
          />
          <input type='submit' value='Submit Your Name' />
      </form>
    </>
  );
}

export default App
```

Test it out in the browser!
