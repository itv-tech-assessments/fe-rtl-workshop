# A possible solution would be:

```js
import { fireEvent, render, screen } from "@testing-library/react";
import SavedList from "../components/SavedList";
import { LikesContext } from "../contexts/Likes";

const mockLikesWithItems = {
  123: {
    name: "Llama",
    image_link:
      "https://upload.wikimedia.org/wikipedia/commons/c/cd/Black_Llama.jpg",
  },
  456: {
    name: "Ring-tailed Lemur",
    image_link:
      "https://upload.wikimedia.org/wikipedia/commons/0/0b/Ring-tailed_lemur_%28Lemur_catta%29.jpg",
  },
};

const likeContext = {
  likes: {},
  likeItem: jest.fn(),
  removeItems: jest.fn(),
};

const renderListComponent = (likes = {}) => {
  likeContext.likes = likes;

  return render(
    <LikesContext.Provider value={likeContext}>
      <SavedList />
    </LikesContext.Provider>
  );
};

describe("given the SavedList component is rendered", () => {
  describe("when multiple liked items exist", () => {
    it("then should contain multiple items with images and checkboxes, and the remove button should be disabled", () => {
      renderListComponent(mockLikesWithItems);

      expect(screen.getByRole("img", { name: "Llama" })).toBeInTheDocument();

      expect(
        screen.getByRole("img", { name: "Ring-tailed Lemur" })
      ).toBeInTheDocument();

      expect(screen.getAllByRole("img")).toHaveLength(2);
      expect(screen.getAllByRole("checkbox")).toHaveLength(2);
      expect(screen.getByRole("button", { name: "Remove" })).toBeDisabled();
    });

    describe("and the a checkbox for an item is selected", () => {
      describe("and the remove button is clicked", () => {
        it("then should call the removeItems function", () => {
          renderListComponent(mockLikesWithItems);

          fireEvent.click(
            screen.getAllByRole("checkbox", {
              name: "Select Llama to be removed",
            })[0]
          );

          fireEvent.click(screen.getByRole("button", { name: "Remove" }));

          expect(likeContext.removeItems).toHaveBeenCalledWith(["123"]);
        });
      });
    });
  });

  describe("when no liked items are stored", () => {
    it("then should not contain a heading, and should not contain items", () => {
      renderListComponent();

      expect(
        screen.queryByRole("heading", { name: "Animals I Like" })
      ).not.toBeInTheDocument();

      expect(screen.queryByRole("img")).not.toBeInTheDocument();
      expect(screen.queryByRole("button")).not.toBeInTheDocument();
    });
  });
});
```

#### [3. Custom Hooks & Mocking](../3.custom-hooks-mocking/README.md)
