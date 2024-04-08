# ![Controlled Forms in React - Managing Controlled Forms](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create and manage a controlled form in React.

## Add an input form to the `App` component

Now let's implement a real controlled form in our application.

To get started, add the following starter code to `App.jsx`:

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

## Add new state to track the input

Once we have the form rendered, we can type values into it. Note that our component has no way of keeping track of the values we enter, so our next step will address this issue.

To begin tracking the value, we need to establish state for the form input. Declare a new state variable called `firstName`.

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

## Connect state to the input field

Once we have state in place, we can add a "value" prop to our input and set the value of that prop to `firstName`. In standard HTML, `value` is an attribute of the input element. However, when you're working with JSX in React, these attributes are treated as `props`.

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

> 🚨 Notice this breaks our input momentarily, as the value is now controlled by state instead of what the user types in.

## Update state with a handler function

Next, we will define a function to help us change the state controlling our input:

```jsx
const App = () => {
  const [firstName, updateFirstName] = useState('');

  // When invoked, update the firstName state to equal the new value (taken from the event)
  const handleFirstNameChange = (e) => {
    updateFirstName(e.target.value);
  };

  return (
    <>
      <h2 id="full-name">The full name will appear in this header.</h2>
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
};

export default App;
```

Confirm that your implemented changes work by typing a value into the input field. With your dev tools open to the React Components tab ( and highlighting the App component ), you should see the value you typed in reflected in the component's state!

## 🎓 You Do: Create a new input for Last Name

Now that we can track the value of our input field via state, let’s repeat that process for a last name. To do this, we will need to:

1. Create state for the last name
2. Connect this state to a new input field
3. Create a function to handle the state change for that input field
4. Test it out and make sure that you can track the elements of state for first name and last name

Test that you can update your state like you did with the first input field. If you can, you are ready to move forward.

## Time for a refactor

At this point your App component should look like this:

```jsx
import { useState } from 'react';

const App = () => {
  const [title, setTitle] = useState('The full name will appear here.');
  const [firstName, updateFirstName] = useState('');
  const [lastName, updateLastName] = useState('');

  const handleFirstNameChange = (e) => {
    updateFirstName(e.target.value);
  };

  const handleLastNameChange = (e) => {
    updateLastName(e.target.value);
  };

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
        <input id="lastName" value={lastName} onChange={handleLastNameChange} />
      </form>
    </>
  );
};

export default App;
```

You may have noticed that our code has some repetition in terms of functionality. Imagine what the `App` component would look like if our form had just 20 inputs? To streamline our code, we can refactor the way we manage our state. Instead of having separate state items for each input, like `firstName` and `lastName`, we can consolidate them into a single state object. This approach also allows us to create a more versatile function that can be used for multiple input fields.

First, let’s change our state to the following:

```jsx
const [title, setTitle] = useState('The full name will appear here.');
// Replace firstName and lastName state with the following:
const [fullName, setFullName] = {
  firstName: '',
  lastName: '',
};
```

We now have an object that can hold state for both inputs. Update the `value` of each input to reflect our new state variable. We'll also add a `name` attribute to our inputs, we'll need this value in the next step:

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
};
// Remove this function:
const handleLastNameChange = (e) => {
  updateLastName(e.target.value);
};

// Replace them with the following:
const handleChange = (e) => {
  setFullName({ ...fullName, [e.target.name]: e.target.value });
};
```

**Note:** A couple of things are happening here:

1. We are using the spread operator to copy any values from the original state into a new object for state. This way, when updating the `firstName` state, we're not losing `lastName` state, and vice versa!
2. `e.target.name` is taking the value of the name prop in our input form. We're able to dynamically access the key through the use of square bracket notation. If the user types into the `firstName` input, then the `name` of that input is used as the key and the `value` of that input is used as the value.

For a more visual example, typing the name "Sam" into the first name input would result in an event handler that does the following:

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

Let's add our new handler function to our inputs:

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
  <input type="submit" value="Submit Your Name" />
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
  <input type="submit" value="Submit Your Name" />
</form>
```

By default, when you submit a form in a web page, it causes the page to refresh. To prevent this from happening in our React application, we can create a function that stops this default behavior. This function will be linked to the form's submit event. Let's add the following function to our component, just below our handler function:

```jsx
const handleSubmit = (e) => {
  // Prevent default event behavior
  e.preventDefault();
  console.log('the page does not refresh now');
};
```

Next, we’ll connect this function to an `onSubmit` handler in the opening form tag:

```jsx
<form onSubmit={handleSubmit}>
```

Once in place, navigate to the console tab of your dev tools, click the submit button, and see if you can view the message without a page refresh.

## Update state on submit

If the message displays in the console, we are ready to make our final adjustments. Update the submit function to call `setTitle` on submit and clear the input state:

```jsx
const handleSubmit = (e) => {
  e.preventDefault();
  // Set our title state based on the fullName state at the time of submission
  setTitle(`Your name is: ${fullName.firstName} ${fullName.lastName}`);
  // Reset fullName state to clear our form inputs
  setFullName({ firstName: '', lastName: '' });
};
```

Your full `App.jsx` should look like this:

```javascript
import { useState } from 'react';

const App = () => {
  const [title, setTitle] = useState('The full name will appear here.');
  const [fullName, setFullName] = useState({ firstName: '', lastName: '' });

  const handleChange = (e) => {
    setFullName({ ...fullName, [e.target.name]: e.target.value });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    setTitle(`Your name is: ${fullName.firstName} ${fullName.lastName}`);
    setFullName({ firstName: '', lastName: '' });
  };

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
        <input type="submit" value="Submit Your Name" />
      </form>
    </>
  );
};

export default App;
```

Great Job! Test it out in the browser!
