# Steeleye-Assignment
>Frontend Engineer Assignment

**Ques-1- Explain what the simple List component does.**

**Answer-1-** List is generally used to store data in ordered format. We use the map() function for traversing the list element, and for updates, we enclosed them between curly braces {}. Here also this List is being used to render data in ordered format. a handleclick function which we pass to the SingleListItem component by which the background color of that particular item changes. There is also a memo method used in the list component. React reuses the memoized content from the same reference. By memoized, it states that it doesn't re-render unnecessarily, The memo is used to make website speed more efficient and perform better.

<!--
// Temprory Answer-1 /////// <br />
Lists are used to display data in an ordered format and mainly used to display menus on websites. We use the map() function for traversing the list element, and for updates, we enclosed them between curly braces {}. Finally, we assign the array elements to listItems. Now, include the new list inside <ul> </ul> elements and render it to the DOM.
In order to create lists of elements, A character characteristic "key" needed to be included .To determine whether elements in a list had already been modified, added, or destroyed, React need so many keys.An individual identifier is required for each object.An id of an entity can also works fine for all of that.

1)Explain what the simple List component does.
Ans: List is generally used to store data in ordered format ,here also this List is being used to render data in ordered format
list items are wrapped under WrappedsingleListItem. a handleclick function which we pass to the SingleListItem component by which the background color of that particular item changes. There is also a memo method used in the list component. React reuses the memoized content from the same reference. By memoized, it states that it doesn't re-render unnecessarily, The memo is used to make website speed more efficient and perform better.

///////////////////
-->

<hr>

**Ques-2- What problems / warnings are there with code?**

**Ans-2-** 
- const [setSelectedIndex , SelectedIndex] = useState(); <br />
Here useState Hook is used but the variable selectedIndex and setSelectedIndex should interchange their position.
because SetSelectedIndex is used to set the value of the SelectedIndex value.
- In the given code, the default items array was null, and it is not possible to map over null, so we need to initialize it with some value.
- While mapping over the array of items, we should have given the key to each item to identify it uniquely as we know key play an important role in rendering each components.
- In WrappedListComponent.propTypes, there is a syntactical error. Instead of using an array, we should have used arrayOf and instead of using shapof, we should have used the shape only.
- Passing of handleclick from WrappedListComponent to SingleListltem was not in bool type. As it is clearly mentioned in wrappedSingleListltem.propTypes is that isSelected proptypes is bool.



<!--
////////////////////
2)What problems / warnings are there with code?

Ans:

1)In our code, as the parent component, which is a wrappedListComponent, has only one state and that is also making direct changes in our child component, which means changing the parent will anyhow change the child, so in such a case, using memo is a waste of memory.
2)While destructuring the array, we must follow the sequence of occurrence of the value, and when we destructure the array of use state in a given problem, we mismatch the sequence due to which we named the values wrong, or say we altered the names, which cause us to use them incorrectly, which caused us the error.
3)In the given code, the default items array was null, and it is not possible to map over null, so we need to initialize it with some value.
4)while mapping over the array of items, we should have given the key to each item to identify it uniquely.
5)There is a syntactical error in the predefined syntax. Instead of using an array, we should have used arrayOf and instead of using shapof, we should have used the shape only.
6)While calling the call back function inside a child, we should have used the arrow function.
7)In props check, we set the isselected value to be boolean, but when we pass boolean values as props, the value is converted to a string or number, so make sure to convert that to boolean before using.

/////////
Passing of handleclick from WrappedListComponent to SingleListltem was not in bool type. As it is clearly mentioned in wrappedSingleListltem.propTypes is that isSelected proptypes is bool.

//////////////////
2-What problems / warnings are there with code?
1->const [setSelectedIndex , SelectedIndex] = useState();
Here useState Hook is used but the variable selectedIndex and setSelectedIndex are should interchange their position
because SetSelectedIndex is used to set the value of the SelectedIndex value . So for that we can write
const [SelectedIndex,setSelectedIndex]= useState();

2->WrappedListComponent.defaultProps has items: null. Therefore there is no item to be mapped.
Fixed code:
WrappedListComponent.defaultProps = {
items: [{ text: "First Item" }, { text: "Second Item" }]
};
Each child in a list should have a unique "key" prop.
Fixed code:
<ul style={{ textAlign: "left" }}>
{items.map((item, index) => (
<SingleListItem
onClickHandler={() => handleClick(index)}
text={item.text}
index={index}
isSelected={selectedIndex}
key={index}
/>
))}

3-> Defining a default prop as null is not recommended.
WrappedListComponent.defaultProps = {
items: null,
};

////////
--->
<hr>

**Ques-3- Please fix, optimize, and/or modify the component as much as you think is necessary.**

**Ans-3-** 
```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red", cursor: "grab" }}
      //Invalid prop isSelected of type array supplied to WrappedSingleListItem, expected boolean. Therefore converting it to boolean.
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
  //const [setSelectedIndex, selectedIndex] = useState();useState variables being misplaced, so interchange their position
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(true);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(Boolean(index)); //boolean
  };


  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
          key={index}  // assign the key
        />
      ))}
    </ul>
  );
};

//There is a syntactical error in the predefined syntax.
//Instead of using an array, we should have used arrayOf and instead of using shapOf, we should have used the shape only.
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
