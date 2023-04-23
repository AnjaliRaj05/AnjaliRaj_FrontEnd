# Steeleye-task
Q1.Explain what the simple List component does.
In React.js, the List component is a simple component that renders an unordered list of items. It takes an array of items as a prop and dynamically generates list items based on the contents of the array.

Here's an example of how the List component might be defined:
```
function List(props) {
  const items = props.items.map(item => <li key={item}>{item}</li>);

  return (
    <ul>
      {items}
    </ul>
  );
}
```

In this example, the List component takes a prop called items, which is an array of strings. The component maps over the items array to create an array of <li> elements, with each item in the array as the content of the list item. The key prop is used to uniquely identify each list item.

The List component can be used in other components by passing an array of items as the items prop. For example:
  ```
function App() {
  return (
    <div className="App">
  <h1>demo</h1>
  <Assignment items={[{ text: 'Anjali Raj' }, { text: 'Anjali Raj' }, { text: 'Anjali Raj' }]}/>
     
    </div>
  );
}
  ```
In this example, the App component creates an array of items and passes it to the List component as the items prop. The List component then renders an unordered list with each item in the array as a list item.
Q2.What problems / warnings are there with code?
Ans: there are some problems from where we can rearrange and optimize the code as follows
  ```
(a):const [selectedIndex, setSelectedIndex] = useState(null); 
(b):onClick={() => onClickHandler(index)}
(c):items: PropTypes.arrayOf(PropTypes.shape({ text: PropTypes.string.isRequired })) 
(d):WrappedListComponent.defaultProps = { items: [] }; 
(e)WrappedListComponent.defaultprops
=items: [{text: "Anjali Raj"}, {text: "12015429"}, {text: "B.Tech CSE"}, {text: "Frontend Developer"}],y};
```




Q3. Please fix, optimize , and/or modify the component as much as you think is necessary.
Ans:





```
  import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red' }}
      onClick={() => { onClickHandler(index) }} //must be called when clicked and not when mounted thats why i created a arrow function
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// This is List Component
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState();// rearrange for destructuring

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index} //key is required by react to optimize the performance of the the component

          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index} //passing  result as boolean if  component is selected or not.
        />
      ))}
    </ul>
  )
};

/*
Note :
  As per 'prop-types' documentation we will get error :
  'Calling PropTypes validators directly is not supported by the `prop-types` package.
Use PropTypes.checkPropTypes() to call them.'

   when we migrate from React.PropTypes to the prop-types package.

   so we can use 'PropTypes.checkPropTypes({})' to resolve the issue... 
*/
WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
  items: [{text: "Anjali Raj"}, {text: "12015429"}, {text: "B.Tech CSE"}, {text: "Frontend Developer"}],
};


const List = memo(WrappedListComponent);

export default List;
```
