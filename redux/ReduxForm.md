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
        <div>
            <div className="form-group">
                <label>{field.label}</label>
                <input
                    className="form-control"
                    type="text"
                    {...field.input}
                />
            </div>
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
    if(!values.title) {
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