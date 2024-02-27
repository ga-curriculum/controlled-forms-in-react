# ![Controlled Forms in React - Form Validations](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to understand and use form validation on their controlled forms in React.

## Using State variables to validate our data

Up until this point we have connected our HTML forms to state to make sure that the latest state is reflected and controlled in the form. We can also use the values in state to inform the user whether or not the information they are providing is valid based on our criteria.

We will show how to do this a couple of ways:

1. We will use values in the state of our component to let a user know during the changes in input if the data they have entered is valid.

2. We will conditionally disable the submit button if the data they are trying to submit is not valid.

### Modifying our State

Let's take the form we were working on for a username and make some adjustments in order to start to add validation to the data. Previously, our form state only had properties for `firstName` and `lastName`. We will keep those, and also add `password` and `passwordConfirm`. We will also add state to keep track of any errors as well.

In `App.jsx`, change state to the following:


```jsx
const [formData, setFormData] = useState({
    firstName: '',
    lastName: '',
    password: '',
    passwordConfirmation: ''
  });
const [errors, setErrors] = useState({
  firstName: '',
  lastName: '',
  password: '',
  passwordConfirmation: ''
});
```

We are doing a few things here:

1. Changing our state variable from `fullName` to `formData` - a more fitting naming convention now that we're adding more inputs to control.

2. We are adding another state variable to store and track any errors that occur as we are validating user input.

Since we have modified our state we need to change our form to reflect this new state:

```jsx
 return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="firstName">First Name:</label>
        <input
          type="text"
          id="firstName"
          name="firstName"
          value={formData.firstName}
          onChange={handleChange}
        />
        {errors.firstName && <div className="error">{errors.firstName}</div>}
      </div>
      <div>
        <label htmlFor="lastName">Last Name:</label>
        <input
          type="text"
          id="lastName"
          name="lastName"
          value={formData.lastName}
          onChange={handleChange}
        />
        {errors.lastName && <div className="error">{errors.lastName}</div>}
      </div>
      <div>
        <label htmlFor="password">Password:</label>
        <input
          type="password"
          id="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
        />
        {errors.password && <div className="error">{errors.password}</div>}
      </div>
      <div>
        <label htmlFor="passwordConfirmation">Password Confirmation:</label>
        <input
          type="password"
          id="passwordConfirmation"
          name="passwordConfirmation"
          value={formData.passwordConfirmation}
          onChange={handleChange}
        />
        {errors.passwordConfirmation && <div className="error">{errors.passwordConfirmation}</div>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );

```

Notice that we have added an optional `div` that we can display if an error message needs to be shown to the user. These errors will be based on validation criteria that we define. In order to determine whether or not input data is valid, we'll need to define some rules. Since this is a simple input form, let's create some arbitrary rules for the sake of triggering validation errors and displaying them on our form:

### Adding form validation in `handleChange`

1. First names must be at least three characters long.
2. Last names must be at least four characters long.
3. Passwords must be at least six characters long.
4. Passwords and password confirmations must match.

We will tackle the rules in our `handleChange` function:

To help keep a separation of concerns, let's create a new helper function called `checkErrors`, whose job will be to check each input as it changes and test it against a provided criteria: 

```jsx
// We only need the target property from the event, so we'll destructure it in the parameter
const checkErrors = ({ target }) => {
    if (target.name === 'firstName') {
      setErrors({ ...errors, firstName: target.value.length < 3 
        ? 'First name must be at least 3 characters long' 
        : '' 
      });
    }
    if (target.name === 'lastName') {
      setErrors({ ...errors, lastName: target.value.length < 4 
        ? 'Last name must be at least 4 characters long' 
        : '' 
      });
    }
    if (target.name === 'password') {
      setErrors({ ...errors, password: target.value.length < 6 
        ? 'Password must be at least 6 characters long' 
        : '' 
      });
    }
    if (target.name === 'passwordConfirmation') {
      setErrors({ ...errors, passwordConfirmation: formData.password !== target.value 
        ? 'Passwords do not match' 
        : '' 
      });
    }
  }
```

Next, add `checkErrors` to our existing `handleChange` function:

```jsx
const handleChange = (e) => {
    // Update form data
    setFormData({ ...formData, [e.target.name]: e.target.value });
    // Invoke helper function, passing it the event
    checkErrors(e)
  };
```

After adding this code try to type into the four different inputs. If these validation rules were added correctly, you should see the "error" messages beneath the corresponding form fields. When you have entered data that is "valid" those messages will go away. This is a great example of using the values in state to help perform form validation.


### Using form validation to disable submission

We can also use state to do things like disable or enable a submission button! 

We want our button to be disabled under two circumstances: 

1. If the form is invalid, which is indicated by 
2. If any of the form inputs have no input. 

We don't need to create new state to handle this, as we can calculate both values off of existing state: 

```jsx
// Use Object.values() to create an array of the values in the errors object
// Check if any of the values are truthy (non-empty strings). If so, .some() will return true. 
const formIsInvalid = Object.values(errors).some(Boolean)
// Use Object.values() to create an array of the values in the formData object
// Check if all of the values are truthy (not empty strings) using .every(). Then, use the bang operator to invert the boolean value. 
// formHasNoInput will be true only if each value in formData is NOT an empty string. 
const formHasNoInput = !Object.values(formData).every(Boolean)
```

Modify your `handleSubmit` function to look like this:

```jsx
const handleSubmit = (e) => {
    e.preventDefault();
    // Submit the form data
    // Example: You can send it to an API endpoint
    console.log('Form submitted:', formData);
  }; 
```

Finally, let's use our calculated values to conditionally disabled the submit button: 

```jsx
<button type="submit" disabled={formIsInvalid || formHasNoInput}>Submit</button>
```

If either `formIsInvalid` or `formHasNoInput` are true, then the button will be disabled.

Try it out! 

**Note:** Front-end validation is NOT a legitimate security measure, but creates a better user experience and should still be provided to both save the backend from random bad requests and let the front-end user only submit forms without obvious errors. It would not be difficult for a bad actor to circumvent a disabled button, for example, so it's crucial to not rely on front-end validation for security. 