# Middleware

Middleware intercepts action flow. If a middleware condition matches, it will do stuff to the action before sending it on to the reducer

## redux-promise
`npm install redux-promise --save`

`redux-promise` is a middleware package designed to intercept promises and resolve them before sending them through to the reducers.

* Action enters middleware
* Is the action payload a promise?

1 **Yes**
* Stop the action
* After promise resolves, create a _new_ action
* Send the new action to reducers

2 **No**
* Send the action unmodified to reducers

### Initializing redux-promise
When creating a new content store, apply a middleware

`index.js`
```
import {createStore, applyMiddleware} from 'redux';
import reducers from './reducers';

const createStoreWithMiddleware = applyMiddleware(ReduxPromise)(createStore);

ReactDOM.render(
    <Provider store={createStoreWithMiddleware(reducers)}>
        <App />
    </Provider>
    , document.getElementById('root'));
```