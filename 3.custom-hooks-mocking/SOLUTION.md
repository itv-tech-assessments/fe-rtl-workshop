# A possible solution would be:

```js
import axios from "axios";
import { waitFor } from "@testing-library/react";
import { renderHook } from "@testing-library/react-hooks";
import useFetchNext from "../hooks/useFetchNext";

jest.mock("axios");

let mockConsoleError;
const mockData = {
  data: { test: "test" },
};

describe("given useFetchNext hook", () => {
  beforeEach(() => {
    axios.get.mockResolvedValue(mockData);

    // mock console.error to avoid expected error messages during tests
    mockConsoleError = jest
      .spyOn(console, "error")
      .mockResolvedValue(jest.fn());
  });

  it("should fetch data from provided URL", async () => {
    const url = "https://test-api.com/animals";
    const { result } = renderHook(() => useFetchNext(url));

    expect(axios.get).toHaveBeenCalledTimes(1);
    expect(axios.get).toHaveBeenCalledWith(url);
    await waitFor(() => expect(result.current.response).toEqual(mockData.data));
    expect(result.current.isError).toBe(false);
  });

  describe("when the API returns an error", () => {
    beforeEach(() => {
      axios.get.mockRejectedValue(new Error("An API error occurred"));
    });

    it("should handle error properly", async () => {
      const url = "https://test-api.com/animals";
      const { result } = renderHook(() => useFetchNext(url));

      expect(axios.get).toHaveBeenCalledTimes(1);
      expect(axios.get).toHaveBeenCalledWith(url);
      await waitFor(() => expect(result.current.isError).toBe(true));
      expect(mockConsoleError).toHaveBeenCalledTimes(1);
      expect(mockConsoleError).toHaveBeenCalledWith(
        new Error("An API error occurred")
      );
    });
  });

  describe("when the fetchNext function is called", () => {
    it("should re-fetch data from provided URL", async () => {
      const url = "https://test-api.com/animals";
      const { result } = renderHook(() => useFetchNext(url));
      result.current.fetchNext();
      expect(axios.get).toHaveBeenCalledTimes(2);
      expect(axios.get).toHaveBeenCalledWith(url);
      await waitFor(() =>
        expect(result.current.response).toEqual(mockData.data)
      );
      expect(result.current.isError).toBe(false);
    });
  });
});
```

#### Next: [4. Putting it all together](../4.putting-it-all-together/README.md)
