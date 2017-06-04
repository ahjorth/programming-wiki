# Redux Thunk
Redux Thunk is a middleware which allows you to return a function instead of an action from your action creators.

Vanilla react action creators are synchronous operations, returning an action as soon as it's told.

With Thunk, you can instead return an asynchronous function that can be called whenever, for example, an api request finishes.

## Installation
`npm install redux-thunk --save`

## Initializing Redux to use Redux Thunk

## Usage in an Action Creator

In this example we are going to call an api request in our action creator,
but instead of returning an action to the applications reducers,

The function receives two parameters, `dispatch` and `getState`

`dispatch` is the application dispatch method.
`getState` returns the current application state.
 
```
import axios from 'axios';

export function getUsers() {
    const request = axios.get('http://example.com/api/users');
    
    return (dispatch) => {
        request.then((response) => {
            dispatch({
                type: 'GET_USERS',
                payload: response.data
            });
        });
    }
}
```

The example above demonstrates the asynchronous property of thunk.
Only when the request promise resolves is the action sent to our reducers.