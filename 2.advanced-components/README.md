# 2. Advanced Components

## Context

There is no need to mock your contexts to test the components that consume them.

Take the following component as an example.

```js
const UserContext = React.createContext();

function App() {
  const user = getUser();

  return (
    <UserContext.Provider value={user}>
      <Greet />
    </UserContext.Provider>
  );
}

function Greet() {
  const user = React.useContext(UserContext);

  if (!user) return "Hello stranger!";
  return `Hello ${user.name}!`;
}
```

To test the `Greet` component renders the correct message, you may be inclined to mock the context. However, the right approach would be to render the component within the context provider as it is done in the source code.

```js
function renderComponent(user) {
  return render(
    <UserContext.Provider value={user}>
      <Greet />
    </UserContext.Provider>
  );
}

it("Greet salutes an anonymous user", () => {
  renderComponent(null);
  expect(screen.getByText("Hello stranger!")).toBeInTheDocument();
});

it("Greet salutes a user", () => {
  const user = { name: "Martha" };
  renderComponent(user);
  expect(screen.getByText(`Hello ${user.name}!`)).toBeInTheDocument();
});
```

## User Events

RTL allows us to trigger a user interaction on a component via `fireEvent`. Interaction events can vary from `click`, `keyDown`, `change`, and [others](https://github.com/testing-library/dom-testing-library/blob/main/src/event-map.js).

```js
import { render, screen, fireEvent } from "@testing-library/react";

const Button = ({ onClick, children }) => (
  <button onClick={onClick}>{children}</button>
);

it("should call onClick prop when clicked", () => {
  const handleClick = jest.fn();
  render(<Button onClick={handleClick}>Click Me</Button>);
  fireEvent.click(screen.getByText(/click me/i));
  expect(handleClick).toHaveBeenCalledTimes(1);
});
```

Note that most projects have a few use cases for `fireEvent`, but the majority of the time you should probably use [`@testing-library/user-event`](https://testing-library.com/docs/user-event/intro). `@testing-library/user-event` provides more advanced simulation of browser interactions than the built-in `fireEvent` method.

## Task

Test the `SavedList` component. These will include subjects from previous tasks as well.

1. Go to file `src/tests/task-02-advanced-components.test.js`
2. Write tests for each test case in the file.

#### Next (once you've completed the task): [Solution for 2. Interactions](./SOLUTION.md)
