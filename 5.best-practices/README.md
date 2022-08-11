# Best practices

## Use `screen`

```js
// ❌
const { getByRole } = render(<InfoTile />);
const heading = getByRole("heading");

// ✅
render(<InfoTile />);
const heading = screen.getByRole("heading");
```

The benefit of using `screen` is that it allows you to use the query functions without specifying a container.

## Not wrapping things in `act` unnecessarily

```js
// ❌
act(() => {
  render(<InfoTile />);
});

const button = screen.getByRole("button", { name: /My Button/i });
act(() => {
  fireEvent.click(button);
});

// ✅
render(<InfoTile />);
const button = screen.getByRole("button", { name: /My Button/i });
fireEvent.click(button);
```

Wrapping things in `act` is unnecessary because `act` is already a wrapped in all RTL utility functions such as `render` and `fireEvent`.

## Using the right query

```js
// ❌
render(<button data-test-id="my-button">My Button</button>);
screen.getByTestId("my-button");

// ✅
render(<button>My Button</button>);
screen.getByRole("button", { name: /My Button/i });
```

If you are unsure which query function to use visit this [page](https://testing-library.com/docs/queries/about/#priority).

## Only use `query*` variants to check for non-existence

```js
// ❌
expect(screen.queryByRole("heading")).toBeInTheDocument();

// ✅
expect(screen.getByRole("heading")).toBeInTheDocument();
expect(screen.queryByRole("heading")).not.toBeInTheDocument();
```

The only reason to use `query*` variants is to check for non-existent elements in the DOM. If you are checking for existing elements, use `get*` variants. It is important to note that `query*` variants will not throw an error if the element is not found as it returns `null`.

Read more about common mistakes: https://kentcdodds.com/blog/common-mistakes-with-react-testing-library

#### Next: [6. Tooling](../6.tooling/README.md)
