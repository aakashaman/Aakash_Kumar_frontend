# Aakash_Kumar_frontend

## 1.	Explain what the simple List component does.

Ans: The List component is a React component that renders a list of items. The list is composed of several SingleListItem components, and each SingleListItem component renders an li element with the text of the item. When a user clicks on a SingleListItem, it sets the isSelected prop to true for that SingleListItem and sets isSelected to false for all other SingleListItems in the list.
The WrappedListComponent is the main component that maps through the items prop and renders SingleListItem components for each item. It initializes a selectedIndex state variable, which is used to determine which SingleListItem component should have its isSelected prop set to true. When the items prop changes, the useEffect hook resets the selectedIndex state variable to null. The SingleListItem component is a memoized version of WrappedSingleListItem that renders each individual li element with the corresponding isSelected and text props. When a user clicks on a SingleListItem, it calls the onClickHandler function with the current index.


## 2.	What problems / warnings are there with code?

Ans. There are many problems with the code which is as follows:

1.	The isSelected prop passed to SingleListItem should be a boolean value, but it is currently being set to the selectedIndex state variable, which is a number. React will give a warning that a prop type is invalid.

2.	The items prop passed to WrappedListComponent is initialized to null, but the propTypes declaration expects an array of objects with a text property. This can cause errors when the component is rendered and the items prop is not an array.

3.	The onClickHandler prop passed to SingleListItem is defined as isRequired, but it is not being passed in when SingleListItem is used in WrappedListComponent. This can cause runtime errors when onClickHandler is called in SingleListItem.

4.	The handleClick function defined in WrappedListComponent is not correctly passing the index to the onClickHandler prop of SingleListItem. Instead of onClickHandler(index), it should be () => onClickHandler(index) to correctly pass a function that takes an index.

5.	There is no key prop being passed to each SingleListItem component when they are mapped in WrappedListComponent.

## 3.	Please fix, optimize, and/or modify the component as much as you think is necessary.

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
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ).isRequired,
};

const List = memo(WrappedListComponent);

export default List;



