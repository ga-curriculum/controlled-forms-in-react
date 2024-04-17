# ![Controlled Forms in React - Form Validations](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to use form validation on their controlled forms in React.

## Using state variables to validate our data

Until now, we have connected our HTML forms to state to ensure that the latest state is reflected and controlled in the form. We can also use the values in state to inform the user whether the information they provide is valid based on the criteria that we choose.

We will show how to do this in a couple of ways:

1. We will use values in the state of our component to let users know if the data they have entered is valid during input changes.
2. We will conditionally disable the submit button if the data they try to submit is invalid.

### Modifying our state

Let's take the form we were working on for a username and make some adjustments to add validation to the data. Previously, our form state only had properties for `firstName` and `lastName`. We will keep those and add `password` and `passwordConfirm`. We will also add a state to keep track of any errors as well.

In `App.jsx`, change state to the following:

```jsx
const [formData, setFormData] = useState({
  firstName: '',
  lastName: '',
  password: '',
  passwordConfirmation: '',
});
const [errors, setErrors] = useState({
  firstName: '',
  lastName: '',
  password: '',
  passwordConfirmation: '',
});
```

Note that the `errors` state's properties mirror the `formData` state's properties.

Since we have modified our state, we must add form inputs to reflect this new state. Note that we're wrapping each of our existing `<input>`s and their corresponding `<label>` elements in `<div>`s as well:

```jsx
// src/App.jsx

      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="firstName">First Name: </label>
          <input
            id="firstName"
            name="firstName"
            value={formData.firstName}
            onChange={handleChange}
          />
        </div>
        <div>
          <label htmlFor="lastName">Last Name: </label>
          <input
            id="lastName"
            name="lastName"
            value={formData.lastName}
            onChange={handleChange}
          />
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
        </div>
        <button type="submit">Submit your name</button>
      </form>
```

### Adding form validation in `handleChange`

We'll need to define some rules to determine whether input data is valid. Since this is a simple input form, let's create some rules for the sake of triggering validation errors and displaying them on our form:

1. First names must be at least three characters long.
2. Last names must be at least two characters long.
3. Passwords must be at least six characters long.
4. Passwords and password confirmations must match.

> ⚠️ Note that these validators are arbitrary. Attaching any validator to a name field (or even assuming people have first and last names) is a [common pitfall](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/) that you should attempt to avoid as a developer.

Next, we will define logic to enforce these rules in our handler function.

To help keep a separation of concerns, let's create a new helper function called `checkErrors`, whose job will be to check each input as it changes and test it against the provided criteria:

```jsx
// src/App.jsx

  // we only need the target property from the event,
  // so we'll destructure it from the event parameter
  const checkErrors = ({ target }) => {
    if (target.name === 'firstName') {
      setErrors({
        ...errors,
        firstName:
          target.value.length < 3
            ? 'Your first name must be at least three characters long.'
            : '',
      });
    }
    if (target.name === 'lastName') {
      setErrors({
        ...errors,
        lastName:
          target.value.length < 2
            ? 'Your last name must be at least two characters long.'
            : '',
      });
    }
    if (target.name === 'password') {
      setErrors({
        ...errors,
        password:
          target.value.length < 6
            ? 'Your password must be at least six characters long.'
            : '',
        passwordConfirmation:
            formData.passwordConfirmation !== target.value
              ? 'The passwords do not match.'
              : '',
      });
    }
    if (target.name === 'passwordConfirmation') {
      setErrors({
        ...errors,
        passwordConfirmation:
          formData.password !== target.value
            ? 'The passwords do not match.'
            : '',
      });
    }
  };
```

Next, add `checkErrors` helper to our existing `handleChange` function:

```jsx
// src/App.jsx

  const handleChange = (event) => {
    setFormData({ ...formData, [event.target.name]: event.target.value });
    // Invoke helper function, passing it the event
    checkErrors(event);
  };
```

Now, our state changes when there is an error!

But we're not showing the changes to users - let's do that next by adding a `<p>` after `<input>` that will be shown if there is an error:

```jsx
// src/App.jsx

      <form onSubmit={handleSubmit}>
        <div>
          <label htmlFor="firstName">First Name: </label>
          <input
            id="firstName"
            name="firstName"
            value={formData.firstName}
            onChange={handleChange}
          />
          {errors.firstName && <p className="error">{errors.firstName}</p>}
        </div>
        <div>
          <label htmlFor="lastName">Last Name: </label>
          <input
            id="lastName"
            name="lastName"
            value={formData.lastName}
            onChange={handleChange}
          />
          {errors.lastName && <p className="error">{errors.lastName}</p>}
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
          {errors.password && <p className="error">{errors.password}</p>}
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
          {errors.passwordConfirmation && (
            <p className="error">{errors.passwordConfirmation}</p>
          )}
        </div>
        <button type="submit">Submit your name</button>
      </form>
```

After adding this code, try typing into the four different inputs. You should see the error messages beneath the corresponding form fields. When you have entered valid data, those messages will disappear. This is an example of using the values in state to help perform form validation.

## Using form validation to disable submission

We can also use state to disable or enable a submission button!

We want our button to be disabled under two circumstances:

1. If the form is invalid, which is indicated by the `errors` state.
2. If any of the form inputs have no input.

We don't need to create a new state to handle this, as we can calculate both values off of the existing state:

```jsx
// src/App.jsx

   const formIsInvalid = Object.values(errors).some(Boolean);
   const formHasMissingData = !Object.values(formData).every(Boolean);
```

Here's a quick explanation of this code, first for `formIsInvalid`:

- `Object.values()` is used to create an array of the values in the `errors` object. In other words, this will be an array of the error message strings that users may see.
- `.some(Boolean)` checks if any values are truthy (non-empty strings). If so, `.some()` will return `true`.
- Ultimately `formIsInvalid` is only `true` if there are error messages in state.

For `formHasMissingData`:

- Again, `Object.values()` is used to create an array of the values in the `formData` object, or in other words, an array containing the data the user has entered into the form.
- Check if every value in the array is truthy (non-empty strings) using `.every(Boolean)`.
- Then, use the bang operator to invert the boolean value returned by `.every()`.
- Ultimately, `formHasMissingData` is only `true` if any of the values in `formData` is an empty string.
- We can't derive this from the `errors` state because we only check for errors after the user has entered something into an input.

Modify your `handleSubmit` function to look like this:

```jsx
// src/App.jsx

  const handleSubmit = (event) => {
    event.preventDefault();
    setTitle(`Your name is: ${formData.firstName} ${formData.lastName}`);
    setFormData({
      firstName: '',
      lastName: '',
      password: '',
      passwordConfirmation: '',
    });
  };
```

Finally, let's use our calculated values to disable the submit button conditionally:

```jsx
// src/App.jsx

        <button type="submit" disabled={formIsInvalid || formHasMissingData}>
          Submit
        </button>
```

If either `formIsInvalid` or `formHasMissingData` is true, then the button will be disabled.

Try it out!

> 🚨 Front-end validation is NOT a legitimate security measure but creates a better user experience. It should still be provided to save the backend from random bad requests and let the front-end user only submit forms without apparent errors. For example, it would not be difficult for a bad actor to circumvent a disabled button, so it's crucial to not rely on front-end validation for security.
