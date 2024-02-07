# ![Controlled Forms in React - Managing Controlled Forms](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to use a form to control state in a React component.

## Adding an Input Form to the `App` component

In order to start our journey towards using controlled forms in React we will want to add a header, form, and input in our `App.jsx` component. 

1. The header will be where the text from our form fields are rendered on the page.
2. The form will contain a single text input.  We will modify the form and input as our needs change.

Add the following to `App.jsx`:

```javascript
const App = () => {

  return (
    <>
      <h2 id='full-name'>The full name will appear in this header.</h2>
      <form>
        <label>
          First Name:
          <input />
        </label>
      </form>
    </>
  );
}

export default App
```

**Note:** By default an input in a form is a text input.  We are wrapping a label around our input for visual clarity.

One we have the form rendered we can type values into it.  This is a great first step but our component has no way of keeping track of the values that we enter.  Our next step will begin to address this issue.

## Add the `useState` hook to our component.

In order to begin to track the value that is being input, we need to establish state.  We will first import the `useState` hook and declare a variable called "firstName".

`App.jsx`:
```javascript
import { useState } from ‘react’; // importing the useState hook

const App = () => {

  const [firstName, updateFirstName] = useState(''); // declaring state with an empty string
  
  return (
  .
  .
  .

```

## Connect state and handling to our input field.

Once we have the state in place, we can add a "value" prop to our input form and set the value of that prop to firstName and connect the update hook via the "onChange" prop.


First, we will define a function to help us change state from our input field:

`App.jxs`:
```javascript

const handleFirstNameChange = (e) => {
  updateFirstName(e.target.value);
}

```

Next, inside of our input field we will add a "value" prop to grab our value from state and an "onChange" prop to call our newly defined function any time a change is tracked in the field.  It will look like this:


```javascript
<label>
  First Name:
  <input
    value={ firstName }
    onChange={ handleFirstNameChange }
  />
</label>
```
Confirm that the changes you have implemented work by typing a value into the input field.  With your dev tools open to the React Components tab ( and highlighting the App component ) you should be able to see the value you typed in reflected in the state of the component!

We now have the ability to track track the value of our input field via state.  Let’s repeat that process for a last name as well

## Repeat the Process for Last Name

Let’s repeat the process and track state for the last name.  In order to do this we will need to:

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
        <label>
          First Name:
          <input 
            value={ firstName }
            onChange={ handleFirstNameChange }
          />
        </label>
        <label>
          First Name:
          <input 
            value={ lastName }
            onChange={ handleLastNameChange }
          />
        </label>
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
2. e.target.name is taking the value of the name prop in our input form
3. e.target.value is acting the same as our previous functions and placing the values we type with said keys.

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