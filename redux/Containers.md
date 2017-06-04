# Containers
Containers are components that have a connection to redux. If a component requires access to redux state, it should be referred to as a container not a component, and be located in its own folder named `/containers`

## Connecting a container to redux

```
import React, {Component} from 'react';
import { connect } from 'react-redux';

class MyComponent extends Component {
    render() {
        return (
            <div>This is a react component</div>
        )
    }
}

export default connect()(MyComponent);
```

### Map State to Props
By itself, being connected doesn't mean much.

The first concept is mapping state to props, passing in the data from the application state to your containers props
```
(..)
// Add a function called mapStateToProps that takes the current application state as an argument
function mapStateToProps(state) {
    return {
        firstProp: state.firstStateKeyToBind,
        secondProp: state.secondStateKeyToBind
    }
}

// Add as an argument to connect the function you just created
export default connect(mapStateToProps)(MyComponent)
```

Now, your `MyComponent` container has two new props. `this.props.firstProp` and `this.props.secondProp`

### Map Dispatch to Props
API calls and various actions called from a container should be `Actions`
You define actions in the `/actions` folder. To bind an action as a prop on your component:

```
import { exportedAction } from `../actions/actionFileName.js`;
import { exportedActionTwo } from `../actions/secondActionFileName.js`;
import { bindActionCreators } from 'redux';
(..)

function mapDispatchToProps(dispatch) {
    return bindActionCreators({ 
        somePropName : exportedAction,
        someOtherPropName : exportedActionTwo
    }, dispatch);
}

// if there's only a dispatch mapped, pass null as first argument to `connect()`
export default connect(null, mapDispatchToProps)(MyComponent);
```

To map both state and dispatch to props:
`export default connect(mapStateToProps, mapDispatchToProps)(MyComponent)`