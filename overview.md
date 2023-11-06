# What is React?
  React is a declarative, efficient, and flexible JavaScript library for building user interfaces. It lets you compose complex UIs from small and isolated pieces of code called “components”.
  We use components to tell React what we want to see on the screen. When our data changes, React will efficiently update and re-render our components.
  Each React component is encapsulated and can operate independently; this allows you to build complex UIs from simple components.



# JSX
  JSX comes with the full power of JavaScript. You can put any JavaScript expressions within braces inside JSX. Each React element is a JavaScript object that you can store in a variable or pass around in your program.

# Tic-Tac-Toe
  By inspecting the code, you’ll notice that we have three React components: 
                     * square
                     * board
                     * game
  The Square component renders a single <button> and the Board renders 9 squares. The Game component renders a board with placeholder values which we’ll modify later. There are currently no interactive components.

  ## props
    Passing props is how information flows in React apps, from parents to children.

  ## making an interactive component
    !! important 
      Notice how with onClick={() => console.log('click')}, we’re passing a function as the onClick prop. React will only call this function after a click. Forgetting () => and writing onClick={console.log('click')} is a common mistake, and would fire every time the component re-renders.

      * this.state:
        React components can have state by setting this.state in their constructors. this.state should be considered as private to a React component that it’s defined in. Let’s store the current value of the Square in this.state, and change it when the Square is clicked.

        !! In JavaScript classes, you need to always call super when defining the constructor of a subclass. All React component classes that have a constructor should start with a super(props) call.

        * To collect data from multiple children, or to have two child components communicate with each other, you need to declare the shared state in their parent component instead. The parent component can pass the state back down to the children by using props; this keeps the child components in sync with each other and with the parent component.

## onClick
   The DOM <button> element’s onClick attribute has a special meaning to React because it is a built-in component. For custom components like Square, the naming is up to you. We could give any name to the Square’s onClick prop or Board’s handleClick method, and the code would work the same. In React, it’s conventional to use on[Event] names for props which represent events and handle[Event] for the methods which handle the events

   the state is stored in the Board component instead of the individual Square components. When the Board’s state changes, the Square components re-render automatically. Keeping the state of all squares in the Board component will allow it to determine the winner in the future.
   :
   class Board ... {
    ...
      handleClick(i) {
    const squares = this.state.squares.slice()
    squares[i] = 'X'
    this.setState({squares:squares})
  }
  ...
   }

   Since the Square components no longer maintain state, the Square components receive values from the Board component and inform the Board component when they’re clicked. In React terms, the Square components are now controlled components. The Board has full control over them.

## Why immutability is Important 
  we created a copy of the squares array using the slice() method instead of modifying the existing array.

  * changing data, 2 approaches:
    -The first approach is to mutate the data by directly changing the data’s values.
    -The second approach is to replace the data with a new copy which has the desired changes.
    https://legacy.reactjs.org/tutorial/tutorial.html#setup-option-2-local-development-environment

    not mutating( second approach) benefits:
    - Complex Features Become Simple:
                                    Immutability makes complex features much easier to implement.
                                     Avoiding direct data mutation lets us keep previous versions of the game’s history intact, and reuse them later.


    - detecting Changes : 
                          Detecting changes in mutable objects is difficult because they are modified directly. This detection requires the mutable object to be compared to previous copies of itself and the entire object tree to be traversed.

                          Detecting changes in immutable objects is considerably easier. If the immutable object that is being referenced is different than the previous one, then the object has changed.

    - Determining When to Re-Render in React:
                                              The main benefit of immutability is that it helps you build pure components in React. Immutable data can easily determine if changes have been made, which helps to determine when a component requires re-rendering.

## Function components vs class components:
     In React, function components are a simpler way to write components that only contain a render method and don’t have their own state. Instead of defining a class which extends React.Component, we can write a function that takes props as input and returns what should be rendered. Function components are less tedious to write than classes, and many components can be expressed this way.

    * When we modified the Square to be a function component, we also changed onClick={() => this.props.onClick()} to a shorter onClick={props.onClick} (note the lack of parentheses on both sides).


## Adding Time Travel
    
    If we mutated the squares array, implementing time travel would be very difficult.

    However, we used slice() to create a new copy of the squares array after every move, and treated it as immutable. This will allow us to store every past version of the squares array, and navigate between the turns that have already happened.

    We’ll store the past squares arrays in another array called history. The history array represents all board states, from the first to the last move


    * Note
    Unlike the array push() method you might be more familiar with, the concat() method doesn’t mutate the original array, so we prefer it

    - We learned earlier that React elements are first-class JavaScript objects; we can pass them around in our applications. To render multiple items in React, we can use an array of React elements.
    arrays have a map() method that is commonly used for mapping data to other data
    Using the map method, we can map our history of moves to React elements representing buttons on the screen, and display a list of buttons to “jump” to past moves.

## key
   key is a special and reserved property in React (along with ref, a more advanced feature). When an element is created, React extracts the key property and stores the key directly on the returned element. Even though key may look like it belongs in props, key cannot be referenced using this.props.key. React automatically uses key to decide which components to update. A component cannot inquire about its key.

   It’s strongly recommended that you assign proper keys whenever you build dynamic lists. If you don’t have an appropriate key, you may want to consider restructuring your data so that you do.

  If no key is specified, React will present a warning and use the array index as a key by default. Using the array index as a key is problematic when trying to re-order a list’s items or inserting/removing list items. Explicitly passing key={i} silences the warning but has the same problems as array indices and is not recommended in most cases.
  
  Keys do not need to be globally unique; they only need to be unique between components and their siblings.


