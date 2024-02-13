# ![Controlled Forms in React - Form Validations](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to understand and use form validation on their controlled forms in React.

## Using State variables to validate our data

Up until this point we have connected our HTML forms to state to make sure that the latest state is reflected and controlled in the form.  We can also use the values in state to inform the user whether or not the information they are providing is valid based on our criteria.

We will show how to do this a couple of ways.

1. We will use values in the state of our component to let a user know during the changes in input if the data they have entered is valid.

2. We will also send an alert during a form submission if the data they are trying to submit is not valid.


### Modifying our State

Let's take the form we were working on for a username and make some adjustments in order to start to add validation to the data.  Previously, our form only had inputs for the `firstName` and `lastName`.  We will keep those and add a couple more inputs for `password` and `passwordConfirm`.  We will also add state for any errors as well.

In your `App.jsx` file let's change our state to the following:

`App.jsx`:
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

1. Changing our state variable from `username` to `formData`.  This is a little better on our naming convention.

2. We are adding another state variable to store and track any errors that occur as we are validating user input.

Since we have modified our state we need to change our form to reflect this new state:

`App.jsx`:
```jsx
 return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>First Name:</label>
        <input
          type="text"
          name="firstName"
          value={formData.firstName}
          onChange={handleChange}
        />
        {errors.firstName && <div className="error">{errors.firstName}</div>}
      </div>
      <div>
        <label>Last Name:</label>
        <input
          type="text"
          name="lastName"
          value={formData.lastName}
          onChange={handleChange}
        />
        {errors.lastName && <div className="error">{errors.lastName}</div>}
      </div>
      <div>
        <label>Password:</label>
        <input
          type="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
        />
        {errors.password && <div className="error">{errors.password}</div>}
      </div>
      <div>
        <label>Password Confirmation:</label>
        <input
          type="password"
          name="passwordConfirmation"
          value={formData.passwordConfirmation}
          onChange={handleChange}
        />
        {errors.passwordConfirmation && <div className="error">{errors.passwordConfirmation}</div>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
};
```

Notice that in addition to adding more form fields to reflect the new data we intend to track in state, we have also added an optional `div` to display if an error is checked into our state based on our own validation criteria that we define.  In order to determine whether or not this data is valid, we want to define some rules.  Since this is a simple input form we will create some arbitrary rules for the sake of triggering validation errors and displaying them on our form:

### Adding form validation in `handleChange`

1. First names must be at least three characters long.
2. Last names must be at least four characters long.
3. Passwords must be at least six characters long.
4. Passwords and password confirmations must match.

We will tackle the rules in our `handleChange` function.

`App.jsx`:
```jsx
const handleChange = (e) => {
    // Update form data
    setFormData({ ...formData, [e.target.name]: e.target.value });
    // Perform validation
    if (e.target.name === 'firstName') {
      setErrors({ ...errors, firstName: e.target.value.length < 3 ? 'First name must be at least 3 characters long' : '' });
    } else if (e.target.name === 'lastName') {
      setErrors({ ...errors, lastName: e.target.value.length < 4 ? 'Last name must be at least 4 characters long' : '' });
    } else if (e.target.name === 'password') {
      setErrors({ ...errors, password: e.target.value.length < 6 ? 'Password must be at least 6 characters long' : '' });
    } else if (e.target.name === 'passwordConfirmation') {
      setErrors({ ...errors, passwordConfirmation: formData.password !== e.target.value ? 'Passwords do not match' : '' });
    }
  };
```

After adding this code try to type into the four different inputs.  If these validation rules are added correctly, you should see the "error" messages beneath the corresponding form fields.  When you have entered data that is "valid" those messages will go away.  This is a great example of using the values in state to help perform form validation.


### Adding form validation in `handleSubmit`

One can also perform the form validations when the form is submitted as well.  In our example, we will modify `handleSubmit` to send an alert regarding any validation error. If the form is submitted with valid data we will see a console log of the form data.  Modify your `handleSubmit` function to look like this:

`App.jsx`
```jsx
const handleSubmit = (e) => {
    e.preventDefault();
    // Validate form data before submission
    if (formData.firstName.length < 3) {
      alert('Please correct the first name in the form.');
      return;  
    } else if (formData.lastName.length < 5) {
      alert('Please correct the last name in the form.');
      return;
    } else if (formData.password.length < 6) {
      alert('Please correct the password in the form.');
      return;
    } else if (formData.password !== formData.passwordConfirmation) {
      alert('Please make sure your passwords match.');
      return;
    }
    
    // Submit the form data
    // Example: You can send it to an API endpoint
    console.log('Form submitted:', formData);
  }; 
```

Now when you try to submit a form with data that you have deemed not valid the form data will not be sent and an alert will pop up.  You will generally want to avoid using alerts in your code but the intent here is to implement a pattern for validating data before it would be sent as an API call.

**Note:** Front-end validation is NOT a legitimate security measure, but is there for the user experience and should still be provided to both save the backend from random bad requests and let the front-end user only submit forms without obvious errors.