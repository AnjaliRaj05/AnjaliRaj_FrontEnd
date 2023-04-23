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
Ans: here is my optimize code


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
<div className='outer_div'>
<li
style={{ backgroundColor: isSelected ? 'green' : 'red'}}
onClick={()=>onClickHandler(index)}
>
{text}
</li>
</div>
);
};

WrappedSingleListItem.propTypes = {
index: PropTypes.number,
isSelected: PropTypes.bool,
onClickHandler: PropTypes.func.isRequired,
text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
items,
}) => {
const [selectedIndex,setSelectedIndex] = useState();

useEffect(() => {
setSelectedIndex(null);
}, [items]);

const handleClick = index => {
setSelectedIndex(index);
};

return (
<div className='inner_div'>
<h1>Display the List items</h1>
<ul style={{ textAlign: 'center' }}>
{items.map((item, index) => (
<SingleListItem
onClickHandler={() => handleClick(index)}
text={item.text}
index={index}
isSelected={selectedIndex}
/>
))}
</ul>
</div>
)
};

WrappedListComponent.propTypes = {
items: PropTypes.arrayOf(PropTypes.shape({
text: PropTypes.string.isRequired,
})),
};

WrappedListComponent.defaultProps = {
items: [
{text : "Anjali Raj"},
{text : "12015429"},
{text : "CSE(HONS)"},
{text: "Frontend Developer"},
]
};

const List = memo(WrappedListComponent);

export default List;


```
