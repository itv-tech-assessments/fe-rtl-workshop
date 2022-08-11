# 3. Custom Hooks & Mocking

## Testing Custom Hooks

To test custom React hooks we need to render them using the `renderHook` function provided by `react-hooks-testing-library`:

```js
import { useState, useCallback } from "react";

export default function useCounter() {
  const [count, setCount] = useState(0);
  const increment = useCallback(() => setCount((x) => x + 1), []);
  return { count, increment };
}
```

```js
import { renderHook } from "@testing-library/react-hooks";
import useCounter from "./useCounter";

it("should use counter", () => {
  const { result } = renderHook(() => useCounter());

  expect(result.current.count).toBe(0);

  act(() => {
    result.current.increment();
  });

  expect(result.current.count).toBe(1);
});
```

After `increment` is called, the current `count` value now reflects the new value returned by the hook.

We also wrapped the increment call in `act` to avoid the `act` warning. This utility simulates simulate how the hook behaves in a browser, allowing us to update the values within it.

## Mocking using Jest

Mocks allow you to test your code by replacing the actual implementation of a function, capturing calls to the function, and allowing test runtime configuration of return values.

```js
import axios from "axios";

const fetchData = () => axios.get("/users.json").then((res) => res.data);

export default fetchData;
```

```js
import axios from "axios";
import fetchData from "./fetchData";

jest.mock("axios");

it("should fetch users", async () => {
  const users = [{ name: "Bob" }];
  const res = { data: users };
  axios.get.mockResolvedValue(res);

  const data = await fetchData();

  expect(data).toEqual(users);
});
```

### Tasks

1. Go to file `src/tests/task-03-custom-hooks-mocking.test.js`
2. Write tests for each test case in the file.

#### Next (once you've completed the task): [Solution for 3. Custom Hooks & Mocking](./SOLUTION.md)
