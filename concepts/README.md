# ![Controlled Forms in React - Concepts](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to explain the difference between the real DOM and virtual DOM, and discuss the benefits of controlled form inputs in React.

## A quick refresher on state

Remember state? Let's recap:

An app's state is its invisible memory, remembering what you're doing, information you've entered, and how it looks. 

A react app uses state to determine what the component looks like when it's first rendered. As soon as that state changes, the component is re-rendered to reflect those changes to state.  

## React's virtual DOM

React's virtual DOM is what makes it so performant. But what is it, and how does it differ from the real DOM?

### Real DOM vs. virtual DOM

While the real DOM is the actual representation of the HTML structure of our app in the browser, the virtual DOM is an in-memory, lightweight representation of the real DOM that React creates and maintains. It's essentially a JavaScript object that mirrors the real DOM structure. 

The real DOM doesn't have any built-in methods for managing application state, meaning that developers have to handle state manually. This can quickly become complex and lead to inconsistencies. 

The virutal DOM, on the other hand, is managed by React and is therefore inherently linked to the app's state. This means state management is simplified and there's consistency between the view and data. 

### A helpful analogy 

Imagine that the UI of our React app is a large skyscraper. The real DOM is the building itself, and represents the elements and structure we see displayed on the screen. As you can imagine, any changes we make to the actual building would be time cosuming and cumbersome. 

React's virtual DOM, however, is like a real-time holographic model of our skyscraper. Since it's just a holograph, it's easy for us to manipulate and update quickly. We can make frequent changes to our holograph to show us what we want to see without actually changing the building itself. When changes *do* need to be made to our building, the virtual DOM allows React to efficiently make those updates only where necessary. 

## Controlled form inputs

In standard HTML, when a user types into a form input field, the browser internally updates the value of the input element. This value resides within the browser's memory and isn't directly accessible by our React component. This causes a disconnect between the UI and our app's state:

- Our app can't react to changes: If our React component needs to respond to the input value (by displaying a greeting or performing validation, for example), it has no way of knowing the latest value since it's not stored in state. 

- There's a one-way flow: The information is only flowing from the user to the browser, and our react component is getting nothing. 

To solve this problem, we can use [**controlled inputs**](https://react.dev/reference/react-dom/components/input#controlling-an-input-with-a-state-variable) to shift conrol from the browser to our React component. We can achieve this like so:

1. Storing the input value in our React component's state with `useState`:

Import `useState`

```jsx
import { useState } from 'react';
```

2. Binding the input's `value` prop to the state variable:

```jsx
import { useState } from 'react';


const NameInput() => {

const [nameInput, setNameInput] = useState('');

// ... rest of our component
}
```

- `nameInput` is the variable that will hold the value of the input field
- `useState('')` sets the initial value of the state variable to an empty string

3. Attaching an `onChange` event handler to the input. This captures the new value whenever a user types and updates state using `setState`.

```jsx
import { useState } from 'react';


const NameInput() => {

const [nameInput, setNameInput] = useState('');

const handleChange = (event) => {
  setNameInput(event.target.value);
};

  return (
    <>
      <input
        type="text" 
        value={nameInput} // Bind state to value prop
        onChange={handleChange}  // Update state on change
      />
    </>

  )

}
```

- `value={nameInput}` ensures the input displays the current value from the state
- `handleChange` is the event handler that captures changes in the input and updates the state using `setInputValue`


