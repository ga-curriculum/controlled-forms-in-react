# ![Controlled Forms in React - Handling Form Submission](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will understand the basics of handling simple form submissions in React.

## Add the submit button

We can hold these values in state but can't do anything with them yet. Our last step will be to add a submit button, connect it to a submit function, and reset our state!

Let's add a submit button to our form:

```jsx
// src/App.jsx

      <form>
        <label htmlFor="firstName">First Name: </label>
        <input
          id="firstName"
          name="firstName"
          value={formData.firstName}
          onChange={handleChange}
        />
        <label htmlFor="lastName">Last Name: </label>
        <input
          id="lastName"
          name="lastName"
          value={formData.lastName}
          onChange={handleChange}
        />
        <button type="submit">Submit your name</button>
      </form>
```

By default, submitting a form on a web page causes a page navigation event in the browser. We never want this to happen in our single-page React app, as it would defeat the point of building a SPA.

To prevent this from happening in our React application, we can create a function that stops this default behavior. This function will be linked to the form's submit event. Let's add the following function to our component just below our handler function:

```jsx
// src/App.jsx

  const handleSubmit = (event) => {
    // Prevent default event behavior
    event.preventDefault();
    console.log('We no longer navigate away from this page');
  };
```

Next, we'll connect this function to an `onSubmit` handler in the opening form tag:

```jsx
// src/App.jsx

<form onSubmit={handleSubmit}>
```

Once in place, navigate to the console tab of your dev tools, click the submit button, and ensure you can view the message.

## Update state on submit

We are ready to make our final adjustments! Update the submit function to call `setTitle` on submit and also reset the input state to the initial values:

```jsx
// src/App.jsx

  const handleSubmit = (event) => {
    event.preventDefault();
    // Set our title state based on the formData state at the time of submission
    setTitle(`Your name is: ${formData.firstName} ${formData.lastName}`);
    // Reset fullName state to clear our form inputs
    setFormData({ firstName: '', lastName: '' });
  };
```

Your full `App.jsx` should look like this:

```javascript
// src/App.jsx

import { useState } from 'react';

const App = () => {
  const [title, setTitle] = useState('The full name will appear here.');
  const [formData, setFormData] = useState({
    firstName: '',
    lastName: '',
  });

  const handleChange = (event) => {
    setFormData({ ...formData, [event.target.name]: event.target.value });
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    // Set our title state based on the formData state at the time of submission
    setTitle(`Your name is: ${formData.firstName} ${formData.lastName}`);
    // Reset fullName state to clear our form inputs
    setFormData({ firstName: '', lastName: '' });
  };

  return (
    <>
      <h2>{title}</h2>
      <form onSubmit={handleSubmit}>
        <label htmlFor="firstName">First Name: </label>
        <input
          id="firstName"
          name="firstName"
          value={formData.firstName}
          onChange={handleChange}
        />
        <label htmlFor="lastName">Last Name: </label>
        <input
          id="lastName"
          name="lastName"
          value={formData.lastName}
          onChange={handleChange}
        />
        <button type="submit">Submit your name</button>
      </form>
    </>
  );
};

export default App;

```

Great job! Test it out in the browser!
