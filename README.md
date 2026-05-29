# React & Next.js Concepts

## React Core Concepts

### JSX

-   Declarative syntax to describe components' appearance and behavior
-   Extension of JavaScript that gets transpiled to `React.createElement`
-   Imperative (vanilla JS) vs Declarative (React) programming
-   React is an abstraction away from the DOM

### Props

-   Read-only and immutable
-   React components must be pure functions
-   When props need to change, parent components must pass new props
-   Old props are discarded and memory is reclaimed by JavaScript engine

### Event Handlers

-   Defined inside components
-   Don't run during rendering
-   Don't need to be pure functions


### Pure Functions
1. It minds its own business. It does not change any objects or variables that existed before it was called.
2. Same inputs, same output. Given the same inputs, a pure function should always return the same result.
3. React assumes that every component you write is a pure function.
4. StrictMode helps to detect impure functions

### Why Pure Functions Matter

1. Components can run in different environments (e.g., server-side)
2. Better performance and caching
3. Safe to stop rendering at any time when data changes

### Render & Commit Process

rendering is a pure calculation

Three-step process:

1. Triggering a render (delivering the order)
2. Rendering the component (preparing the order)
3. Committing to the DOM (serving the order)

-   In React, every update is split in two phases:
-   During render, React calls your components to figure out what should be on the screen.
-   During commit, React applies changes to the DOM.
    Components render when:

#### Triggering

-   It's their initial render (createRoot)
-   Their state (or ancestor's state) has been updated (call the function component recursively)

#### Rendering

-   During the initial render, React will create the DOM nodes for tags
-   During a re-render, React will calculate which of their properties, if any, have changed since the previous render. It won’t do anything with that information until the next step, the commit phase.

#### commiting

-   For the initial render, React will use the appendChild() DOM API to put all the DOM nodes it has created on screen.
-   For re-renders, React will apply the minimal necessary operations (calculated while rendering!) to make the DOM match the latest rendering output.

React does not touch the DOM if the rendering result is the same as last time

### State Management

-   Setting state requests a new re-render
-   State changes trigger re-renders
-   Hooks rely on stable call order during renders
-   Each component manages its own state
-   A state variable’s value never changes within a render
-   React keeps the state values “fixed” within one render
-   React stores state outside of your component
-   React waits until all code in the event handlers has run before processing your state updates
-   Don’t put props into state unless you specifically want to prevent updates.
-   For UI patterns like selection, keep ID or index in state instead of the object itself.
-   Re-rendering a component does not re-run the useState initializer. The initial value passed to useState(...) is used only once, when the component is first mounted. On subsequent renders, React retrieves the state from its internal memory, not from the initializer.
-   State is tied to a position in the render tree
-   the state is actually held inside React not the component

### reducers

-   reducers must be pure

### refs

-   When you want a component to “remember” some information, but you don’t want that information to trigger new renders
-   Unlike states, Refs are mutable
-   If your component needs to store some value, but it doesn’t impact the rendering logic, choose refs.

### Batch state update

-   makes your React app run much faster
-   React does not batch across multiple intentional events like clicks
-   the UI won’t be updated until after your event handler, and any code in it, completes

### **State Memory and Batching in React**

-   **State memory** refers to the temporary memory React uses to store state changes before applying them during a re-render.

    -   When you call `setState` or `setPending`, React stores these changes in its internal memory (state memory).
    -   React does not trigger a re-render immediately and waits until the current event handler (like a click or form submission) has completed.

-   **Batching**:

    -   In React, when you have multiple state changes in a single event handler, these changes are grouped into a batch.
    -   React applies these changes simultaneously in the internal state memory and triggers a single re-render after all changes have been processed.

-   **Mechanism**:

    1. When you call `setPending(pending + 1)` inside an event handler:
        - The change is temporarily stored in **state memory**.
    2. React does not trigger a re-render immediately and waits until the event handler finishes executing.
    3. After the event handler finishes, React applies all the state changes at once, triggering only a single re-render.
    4. Once the re-render is triggered, the **state memory** is cleared, and the new state values are reflected in the UI.

-   **Key Point**:

    -   **State memory** is temporary storage that holds the new state values until a re-render occurs. Once the render happens, the changes from state memory are applied and the memory is cleared.

-   **Why This Helps**:
    -   This mechanism helps React optimize performance by reducing unnecessary re-renders.
    -   Instead of triggering a re-render after each state update, React groups multiple state changes into a batch and performs only one re-render. This improves performance and avoids performance bottlenecks.

### **Advanced Notes on React's State Batching Mechanism**

-   **Event Loop and Asynchronous Updates**:

    -   React batches state updates, even when they are asynchronous (e.g., `setTimeout`, `setInterval`, or `fetch` responses).
    -   This batching ensures that React only re-renders after all pending state updates are processed, even if the state changes happen asynchronously.

-   **Concurrent Mode and Automatic Batching**:

    -   With React 18 and Concurrent Mode, React has introduced more aggressive batching mechanisms.
    -   It automatically batches updates across multiple event handlers and even across multiple renders, improving performance further.

-   **Interaction with Hooks**:

    -   React’s `useState` and `useReducer` hooks follow this batching mechanism.
    -   While you can’t directly control batching, understanding this behavior is important when using hooks in event handlers or inside `useEffect`/`useLayoutEffect`.
    -   For example, changes to state inside `useEffect` will also be batched before a re-render, unless wrapped in a separate event cycle or action.

-   **State Updates from Nested Components**:
    -   React ensures that if a parent component triggers a state change, the child component will receive the updated state value after the batch processing.
    -   This also includes state updates that happen inside a single component when multiple `setState` calls are made one after another within the same event handler.

### **Conclusion**

-   React's state batching mechanism is designed to optimize performance by minimizing unnecessary renders.
-   By using **state memory**, React can temporarily store state changes and apply them in a single re-render, even when multiple updates happen simultaneously.
-   This mechanism plays a crucial role in enhancing the responsiveness and performance of React applications.

### queueing

-   update the same state variable multiple times before the next render
-   pass a function that calculates the next state based on the previous one in the queue

#### Local Variables

-   Don't persist between renders
-   Changes don't trigger renders
-   Each render starts fresh

### Controlled Elements

-   Elements with internal state (e.g., input values)
-   an component is uncontrolled when its parent cannot influence whether the panel is active or not.
-   a component is “controlled” when the important information in it is driven by props rather than its own local state
-   Uncontrolled components are easier to use within their parents because they require less configuration. But they’re less flexible when you want to coordinate them together. Controlled components are maximally flexible, but they require the parent components to fully configure them with props.
-   It’s useful to consider components as “controlled” (driven by props) or “uncontrolled” (driven by state).

### Lazy Evaluation

```jsx
const [state, setState] = useState(() => return 2)
```

### Lifecycle

Components re-render when:

-   State changes
-   Parent re-renders
-   Context changes

### Side Effects

-   a side effect caused by rendering.
-   Result of impure functions
-   Handle interactions between React and the outside world
-   Event handlers and Effects
-   Render logic runs during render
-   Effect logic runs after browser paint
-   Cleanup functions run after re-renders and unmount
-   One effect per side effect
-   Effects let you specify side effects that are caused by rendering itself, rather than by a particular event.
-   Effects run at the end of a commit after the screen updates.

## React Optimization Techniques

### Memoization

-   useMemo caches the result of calling your function.
-   useCallback caches the function itself. Unlike useMemo, it does not call the function you provide.
-   Use `memo` for component memoization
-   `useMemo` and `useCallback` for function memoization
-   Move functions out of components when possible
-   State setter functions are automatically memoized

## important optimization techniques

-   use children prop,if the wrapper component updates its own state, React knows that its children don’t need to re-render.
-   use global state
-   keep components pure
-   Avoid unnecessary Effects
-   remove unnecessary dependencies from your Effects.
-   link: https://react.dev/reference/react/useCallback

#### useCallback

-   Caching a function with useCallback is only valuable in a few cases:
-   You pass it as a prop to a component wrapped in memo.
-   The function you’re passing is later used as a dependency of some Hook.

### Code Splitting

-   Use React.lazy import and Suspense API
-   Reduces initial bundle size

### Performance Patterns

-   Use slow components as children props
-   Render props pattern
-   Higher-Order Components (HOC)
-   Handle sorting and filtering in URL

## React Compiler

React Compiler automatically applies the equivalent of manual memoization, ensuring that only the relevant parts of an app re-render as state changes. It focuses on two main use cases:

1. **Skipping cascading re-renders of components**
   Re-rendering `<Parent />` causes many components in its component tree to re-render, even though only `<Parent />` has changed.

2. **Skipping expensive calculations inside React components and hooks**
   For example, calling `expensivelyProcessAReallyLargeArrayOfObjects()` inside a component or hook that needs that data.

### Important Limitations

1. React Compiler only memoizes React components and hooks, not every function.
2. React Compiler's memoization is not shared across multiple components or hooks.
   zation is not shared across multiple components or hooks

## Next.js Concepts

### Fetching Strategies

1. Server-side:
    - Fetch API
    - Third-party APIs
2. Client-side:
    - Route handlers
    - Third-party APIs

### Key Points

-   Next.js caches fetched URLs
-   Client components can't import server components but can render them as props
-   Both client and server components render on server during initial render

### Rendering Methods

1. Static Rendering (default)
    - With revalidation
2. Dynamic Rendering
    - `{ cache: 'no-store' }`
    - `dynamic = force-dynamic`
3. Streaming
    - Uses `loading.tsx` for pages or Suspense for components
    - Components must return their own Promise
    - Progressive UI rendering from server
4. Partial Pre-rendering

### Static Generation

-   `generateStaticParams`:
    -   Changes dynamic routes to static generation
    -   New params render dynamically first, then become static

## Architecture Comparison

### SPA Drawbacks

1. Rendering and data fetching in browser
2. Large JS bundle
3. Poor First Contentful Paint (FCP)
4. Response: HTML + JS bundle

### SSR Benefits

-   First load on server
-   Better FCP
-   Response: HTML + JS bundle

### SSR Drawbacks

-   Must wait for JS bundle download
-   Poor Time to Interactive (TTI)
-   Only first request differs, then behaves like SPA

### React Server Components (RSC)

-   React v18 feature
-   Incrementally streams content from server to browser
-   Splits app into chunks
-   Sends static parts first, then DB-dependent parts

### RSC Payload

-   React: Sends payload to client → deserialize → render RSC → small JS bundle → good TTI
-   Next.js: Creates RSC using payload → sends static HTML

### SSR vs RSC Comparison

-   Better FCP in RSC
-   Server-side rendering not limited to initial page load
-   Smaller JS bundle → better TTI

