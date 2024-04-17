# ![Controlled Forms in React - Managing Controlled Forms](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create and manage a controlled form in React.

## Add an input form to the `App` component

Now let's implement a controlled form in our application. A controlled form in React is a form where all the form elements within it (inputs, checkboxes, select dropdowns, etc.) are controlled by React state.

To get started, replace the existing code in `App.jsx` with this code:

```jsx
// src/App.jsx

// import the useState() hook
import { useState } from 'react';

const App = () => {
  // add placeholder text for the title
  // we'll use the form to update this state by the end of this lesson
  const [title, setTitle] = useState('The full name will appear here.');

  return (
    <>
      <h2>{title}</h2>
      <form>
        <label htmlFor="firstName">First Name: </label>
        <input id="firstName" name="firstName" />
      </form>
    </>
  );
};

export default App;
```

The header will be where the text from our form fields is rendered on the page. The form will contain a single text input. We'll modify the form and input as our needs change, but this is a good starting place.

## Add new state to track the input

Once we have the form rendered, we can type values into it. Note that our component has no way of keeping track of the values we enter, so our next step will address this issue.

To begin tracking the value, we need to establish state for the form input. Declare a new state variable called `firstName`.

```jsx
// src/App.jsx

const App = () => {
  const [title, setTitle] = useState('The full name will appear here.');
  // declaring state with an empty string as the initial value
  const [firstName, setFirstName] = useState('');

  return (
    <>
      <h2>{title}</h2>
      <form>
        <label htmlFor="firstName">First Name: </label>
        <input id="firstName" name="firstName" />
      </form>
    </>
  );
};
```

## Connect state to the input field

With state in place, add a `value` prop to the input and set its value to `firstName`:

```jsx
    <>
      <h2>{title}</h2>
      <form>
        <label htmlFor="firstName">First Name: </label>
        <input
          id="firstName"
          value={firstName}
        />
      </form>
    </>
```

> 🚨 Notice this breaks our input momentarily, as the value is now controlled by state instead of what the user types in.

## Update state with a handler function

Next, we will define a function called `handleFirstNameChange()` to help us change the state controlling our input. Add it to the App component.

```jsx
// src/App.jsx

const App = () => {
  const [title, setTitle] = useState('The full name will appear here.');
  const [firstName, setFirstName] = useState('');

  // when invoked, update the firstName state to equal the new value 
  const handleFirstNameChange = (event) => {
    setFirstName(event.target.value);
  };

  return (
    <>
      <h2>{title}</h2>
      <form>
        <label htmlFor="firstName">First Name: </label>
        <input
          id="firstName"
          name="firstName"
          value={firstName}
          onChange={handleFirstNameChange}
        />
      </form>
    </>
  );
};
```

Confirm that your implemented changes work by typing a value into the input field. With your dev tools open to the React Components tab (and highlighting the App component), you should see the value you typed in reflected in the component's state!

## 🎓 You Do

Now that we can track the value of our input field via state, let's repeat that process for a last name. To do this, we will need to:

1. Create state for the last name. Call it `lastName`.
2. Connect this state to a new input field.
3. Create a function to handle the state change for that input field. Call it `handleLastNameChange`.
4. Test it out and make sure that you can track the elements of state for first name and last name.

Test that you can update your state like you did with the first input field. If you can, you are ready to move forward.
