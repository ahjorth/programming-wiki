# Stateless functional components

A stateless functional component is a plain Javascript function, that contains no state. It only returns jsx.
  
 **It is the preferred way of creating a component. Only after state or complex logic is required should it be refactored to a regular react component**
 
 ## Defining a stateless functional component
 
 ```  
 const MyComponent = (props) => {
    return (
        <div>
            <h1>This is my component</h1>
        </div>
    )
 };
 ```