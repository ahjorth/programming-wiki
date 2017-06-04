# Actions
Actions are payloads of information that sends data from containers to the state storage.

When passing data to the store, you **MUST** use an action

#### Example action creator
An action creator is a function that creates an action.

An action creator **must return an object** where the `type` is **required**

Most commonly, when returning more than just the action type, a payload key is set with the payload you want to pass on


```
// we define UPDATE_USERNAME as a constant to preserve the integrity of the string

export const UPDATE_USERNAME = 'UPDATE_USERNAME';
export function changeUsername(username) {
    return {
        type: UPDATE_USERNAME,
        username
    }
}
```

This action is then passed on to **all** reducers where a switch statement can be used to match the action type.
