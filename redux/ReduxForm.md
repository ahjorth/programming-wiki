# Redux Form

`npm install redux-form --save`

## Basic setup

* Wire up to combined reducers
```
import {reducer as formReducer } from 'redux-form';

const rootReducer = combineReducers({
    form: formReducer
});
```

In a component

```
import { Field, reduxForm } from 'redux-form';

class MyComponent extends Component {
    render() {
        return (
            <div>
                New Form
            </div>
        );
    }
}

export default reduxForm({
    form: 'MyComponentForm'
})(MyComponent);
```

## Declaring Fields

### 1) By built in components
By passing in a string with a valid form type, `redux-form` handles the rendering by itself
```
<form>
    <Field
        component="input"
    />
</form>
```

### 2) Declaring a render function
By passing in a render method as a callback to the component prop, we can fine tune how the field renders
```
renderField(field) {
    return (
        <div className="form-group">
            <label>{field.label}</label>
            <input
                className="form-control"
                type="text"
                {...field.input}
            />
        </div>
    )
}

render() {
    return (
        <form>
            <Field
                label="Field Label"
                name="title"
                component={this.renderField}
            />
        </form>
    );
}
```

## Validation
Any time a field is updated (i.e text is typed), the validate method runs, passing in the `values` parameter which is an object containing all the fields non-empty values.

`{title: "a"}`
`{title: "ab"}`
`{title: "ab", content: "1"}`
`{title: "ab", content: "12"}`

### Wiring up validation
Declare a `validate()` method in the component class, and initialize it in the `reduxForm` connector
```
function validate(values) {
    const errors = {}; // initialize an empty error object
    return errors; // return errors
}

export default reduxForm({
    validate: validate, // value is the function above
    form: 'MyComponentForm'
});
```

### Writing validation rules
If this method returns an empty `errors` object, `redux-form` will assert the validation as passing.

```
function validate(values) {
    const errors = {};
    if (!values.title) {
        errors.title = 'Enter a title';
    }
    return errors;
}
```

Above, if the title field is empty, `errors` is not empty, and `redux-form` assumes the form is invalid.
** The `errors.key` must correspond to the field name **

### Displaying errors in the form
In a render field method, you can display this fields error by referencing

`field.meta.error`

When component loads, however, all errors will show even if no work has been done to the field

This can be solved by checking the `touched` state of the field.

```
<div className="form-control-feedback">
    {field.meta.touched ? field.meta.error : ''}
</div>
```

In the example above, error messages will only be shown if the field is `touched` (i.e the user focuses the input then focuses something else)

## Submitting a form
In the components render method, we pass in a function to the forms onSubmit action.

When we connected `reduxForm` to `MyComponent` , `redux-form` assigned some props to our component.
One of these props was the `handleSubmit` function.
`handleSubmit` will only submit the form once validation has passed.

Once validation has passed and `redux-form` decides everything is ready, it calls the callback we defined, called `onSubmit`.

We bind our callback function to `this` to get access to the correct `this` (this can be done in the constructor or using any other method of binding)

Our callback function will have the parameter `values` that represent the fields in our form.
```
onSubmit(values) {
    // call action this.props.createPost(values) for example
}
render() {
    const { handleSubmit } = this.props;
    return (
        <form onSubmit={handleSubmit(this.onSubmit.bind(this))}>
    )
}
```

## Hooking up action creators
We've already connected `redux-form` to our store, so to use redux `connect()` we need to chain it

```
import { connect } from `redux`;
import { createPost } from '../actions';

(...)

export default reduxForm({
    form: 'MyComponentForm'
})(
    connect(null, { createPost })(MyComponent)
);
```

## Making sure api requests finish before redirecting

If the form is used in conjunction with `react-router-dom` you can redirect a user by pushing a new location to history after the form is submitted.

When using API's, this can present a problem where the redirection happens before the remote server saves or updates the new resource.

To make sure we only redirect after the request has finished, we can pass a callback into our action

```
onSubmit(values) {
        this.props.createPost(values, () => {
            this.props.history.push('/');
        });
    }
```

Then on the action creator, we call the callback after the promise resolves

```
export function createPost(values, callback) {
    const request = axios.post(url, values)
        .then(() => callback());

    return {
        type: CREATE_POST,
        payload: request
    }
}
```