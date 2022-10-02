# Steeleye-Assignment
>Frontend Engineer Assignment

**Ques-1- Explain what the simple List component does.**

**Answer-1-** List is used to store data in ordered format. We use the map() function for traversing the list element, and for updates, we enclosed them between curly braces {}. Here also this List is being used to render data in ordered format. List items are wrapped under WrappedSingleListItem. A handleclick function which we pass to the SingleListItem component by which the background color of that particular item changes. There is also a memo method used in the list component. React reuses the memoized content from the same reference. This prevents the component from re-rendering unless the dependencies (props) have changed.The memo is used to make website speed more efficient and perform better.

<hr>

**Ques-2- What problems / warnings are there with code?**

**Ans-2-** 
- const [setSelectedIndex , SelectedIndex] = useState(); <br />
Here useState Hook is used but the variable selectedIndex and setSelectedIndex should interchange their position.
because SetSelectedIndex is used to set the value of the SelectedIndex value.
- In the code, the default items array was null, and it is not possible to map over null, so we need to initialize it with some value.
- While mapping over the array of items, we should have given the key to each item to identify it uniquely as we know key play an important role in rendering each components.
- Passing of handleclick from WrappedListComponent to SingleListltem was not in bool type. As it is clearly mentioned in wrappedSingleListltem.propTypes is that isSelected proptypes is bool.
- In WrappedListComponent.propTypes, there is a syntactical error. Instead of using an array, we should have used arrayOf and instead of using shapof, we should have used the shape only.

<hr>

**Ques-3- Please fix, optimize, and/or modify the component as much as you think is necessary.**

**Ans-3-** 
```javascript
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red", cursor: "grab" }}
      //Invalid prop isSelected of type array supplied to WrappedSingleListItem, which expected boolean. So converting it to boolean.
      onClick={() => onClickHandler(Boolean(index))}
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

// List Component
const WrappedListComponent = ({ items }) => {
  //const [setSelectedIndex, selectedIndex] = useState();useState variables being misplaced, so interchanging their position
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(true);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(Boolean(index));   //boolean
  };


  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
          key={index}    //assign the key
        />
      ))}
    </ul>
  );
};

//There is a syntactical error in the predefined syntax.
//Instead of using an array, we should have used arrayOf and instead of using shapOf, we should have used shape.
WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

//The default items array in the provided code was null, and since mapping over null is not possible, we must initialise it with a value.
WrappedListComponent.defaultProps = {
  items: [{ text: "First item" }, { text: "Second item" }],
};

const List = memo(WrappedListComponent);

export default List;
```
