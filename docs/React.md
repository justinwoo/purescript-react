## Module React

This module defines foreign types and functions which wrap React's functionality.

#### `TagName`

``` purescript
type TagName = String
```

Name of a tag.

#### `ReactElement`

``` purescript
data ReactElement :: *
```

A virtual DOM node, or component.

#### `ReactComponent`

``` purescript
data ReactComponent :: *
```

A mounted react component

#### `ReactThis`

``` purescript
data ReactThis :: * -> * -> *
```

A reference to a component, essentially React's `this`.

#### `EventHandler`

``` purescript
data EventHandler :: * -> *
```

An event handler. The type argument represents the type of the event.

#### `Read`

``` purescript
data Read :: !
```

This phantom type indicates that read access to a resource is allowed.

#### `Write`

``` purescript
data Write :: !
```

This phantom type indicates that write access to a resource is allowed.

#### `Disallowed`

``` purescript
type Disallowed = () :: # !
```

An access synonym which indicates that neither read nor write access are allowed.

#### `ReadWrite`

``` purescript
type ReadWrite = (read :: Read, write :: Write)
```

An access synonym which indicates that both read and write access are allowed.

#### `ReadOnly`

``` purescript
type ReadOnly = (read :: Read)
```

An access synonym which indicates that reads are allowed but writes are not.

#### `ReactState`

``` purescript
data ReactState :: # ! -> !
```

This effect indicates that a computation may read or write the component state.

The first type argument is a row of access types (`Read`, `Write`).

#### `ReactProps`

``` purescript
data ReactProps :: !
```

This effect indicates that a computation may read the component props.

#### `ReactRefs`

``` purescript
data ReactRefs :: # ! -> !
```

This effect indicates that a computation may read the component refs.

The first type argument is a row of access types (`Read`, `Write`).

#### `Refs`

``` purescript
data Refs :: *
```

The type of refs objects.

#### `Event`

``` purescript
data Event :: *
```

The type of DOM events.

#### `MouseEvent`

``` purescript
type MouseEvent = { pageX :: Number, pageY :: Number }
```

The type of mouse events.

#### `KeyboardEvent`

``` purescript
type KeyboardEvent = { altKey :: Boolean, ctrlKey :: Boolean, charCode :: Int, key :: String, keyCode :: Int, locale :: String, location :: Int, metaKey :: Boolean, repeat :: Boolean, shiftKey :: Boolean, which :: Int }
```

The type of keyboard events.

#### `EventHandlerContext`

``` purescript
type EventHandlerContext eff props state result = Eff (props :: ReactProps, refs :: ReactRefs ReadOnly, state :: ReactState ReadWrite | eff) result
```

A function which handles events.

#### `Render`

``` purescript
type Render props state eff = ReactThis props state -> Eff (props :: ReactProps, refs :: ReactRefs Disallowed, state :: ReactState ReadOnly | eff) ReactElement
```

A render function.

#### `GetInitialState`

``` purescript
type GetInitialState props state eff = ReactThis props state -> Eff (props :: ReactProps, state :: ReactState Disallowed, refs :: ReactRefs Disallowed | eff) state
```

A get initial state function.

#### `ComponentWillMount`

``` purescript
type ComponentWillMount props state eff = ReactThis props state -> Eff (props :: ReactProps, state :: ReactState ReadWrite, refs :: ReactRefs Disallowed | eff) Unit
```

A component will mount function.

#### `ComponentDidMount`

``` purescript
type ComponentDidMount props state eff = ReactThis props state -> Eff (props :: ReactProps, state :: ReactState ReadWrite, refs :: ReactRefs ReadOnly | eff) Unit
```

A component did mount function.

#### `ComponentWillReceiveProps`

``` purescript
type ComponentWillReceiveProps props state eff = ReactThis props state -> props -> Eff (props :: ReactProps, state :: ReactState ReadWrite, refs :: ReactRefs ReadOnly | eff) Unit
```

A component will receive props function.

#### `ShouldComponentUpdate`

``` purescript
type ShouldComponentUpdate props state eff = ReactThis props state -> props -> state -> Eff (props :: ReactProps, state :: ReactState ReadWrite, refs :: ReactRefs ReadOnly | eff) Boolean
```

A should component update function.

#### `ComponentWillUpdate`

``` purescript
type ComponentWillUpdate props state eff = ReactThis props state -> props -> state -> Eff (props :: ReactProps, state :: ReactState ReadWrite, refs :: ReactRefs ReadOnly | eff) Unit
```

A component will update function.

#### `ComponentDidUpdate`

``` purescript
type ComponentDidUpdate props state eff = ReactThis props state -> props -> state -> Eff (props :: ReactProps, state :: ReactState ReadOnly, refs :: ReactRefs ReadOnly | eff) Unit
```

A component did update function.

#### `ComponentWillUnmount`

``` purescript
type ComponentWillUnmount props state eff = ReactThis props state -> Eff (props :: ReactProps, state :: ReactState ReadOnly, refs :: ReactRefs ReadOnly | eff) Unit
```

A component will unmount function.

#### `ReactSpec`

``` purescript
type ReactSpec props state eff = { render :: Render props state eff, displayName :: String, getInitialState :: GetInitialState props state eff, componentWillMount :: ComponentWillMount props state eff, componentDidMount :: ComponentDidMount props state eff, componentWillReceiveProps :: ComponentWillReceiveProps props state eff, shouldComponentUpdate :: ShouldComponentUpdate props state eff, componentWillUpdate :: ComponentWillUpdate props state eff, componentDidUpdate :: ComponentDidUpdate props state eff, componentWillUnmount :: ComponentWillUnmount props state eff }
```

A specification of a component.

#### `spec`

``` purescript
spec :: forall props state eff. state -> Render props state eff -> ReactSpec props state eff
```

Create a component specification with a provided state.

#### `spec'`

``` purescript
spec' :: forall props state eff. GetInitialState props state eff -> Render props state eff -> ReactSpec props state eff
```

Create a component specification with a get initial state function.

#### `ReactClass`

``` purescript
data ReactClass :: * -> *
```

React class for components.

#### `getProps`

``` purescript
getProps :: forall props state eff. ReactThis props state -> Eff (props :: ReactProps | eff) props
```

Read the component props.

#### `getRefs`

``` purescript
getRefs :: forall props state access eff. ReactThis props state -> Eff (refs :: ReactRefs (read :: Read | access) | eff) Refs
```

Read the component refs.

#### `getChildren`

``` purescript
getChildren :: forall props state eff. ReactThis props state -> Eff (props :: ReactProps | eff) (Array ReactElement)
```

Read the component children property.

#### `writeState`

``` purescript
writeState :: forall props state access eff. ReactThis props state -> state -> Eff (state :: ReactState (write :: Write | access) | eff) state
```

Write the component state.

#### `readState`

``` purescript
readState :: forall props state access eff. ReactThis props state -> Eff (state :: ReactState (read :: Read | access) | eff) state
```

Read the component state.

#### `transformState`

``` purescript
transformState :: forall props state eff. ReactThis props state -> (state -> state) -> Eff (state :: ReactState ReadWrite | eff) state
```

Transform the component state by applying a function.

#### `createClass`

``` purescript
createClass :: forall props state eff. ReactSpec props state eff -> ReactClass props
```

Create a React class from a specification.

#### `createClassStateless`

``` purescript
createClassStateless :: forall props. (props -> ReactElement) -> ReactClass props
```

Create a stateless React class.

#### `handle`

``` purescript
handle :: forall eff ev props state result. (ev -> EventHandlerContext eff props state result) -> EventHandler ev
```

Create an event handler.

#### `createElement`

``` purescript
createElement :: forall props. ReactClass props -> props -> Array ReactElement -> ReactElement
```

Create an element from a React class spreading the children array. Used when the children are known up front.

#### `createElementDynamic`

``` purescript
createElementDynamic :: forall props. ReactClass props -> props -> Array ReactElement -> ReactElement
```

Create an element from a React class passing the children array. Used for a dynamic array of children.

#### `createElementTagName`

``` purescript
createElementTagName :: forall props. TagName -> props -> Array ReactElement -> ReactElement
```

Create an element from a tag name spreading the children array. Used when the children are known up front.

#### `createElementTagNameDynamic`

``` purescript
createElementTagNameDynamic :: forall props. TagName -> props -> Array ReactElement -> ReactElement
```

Create an element from a tag name passing the children array. Used for a dynamic array of children.

#### `createFactory`

``` purescript
createFactory :: forall props. ReactClass props -> props -> ReactElement
```

Create a factory from a React class.


