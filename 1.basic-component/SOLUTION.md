# A possible solution would be:

```js
import { render, screen } from "@testing-library/react";
import InfoTile from "../components/InfoTile";

const infoTileMock = {
  name: "Llama",
  image_link:
    "https://upload.wikimedia.org/wikipedia/commons/c/cd/Black_Llama.jpg",
  latin_name: "Lama glama",
  animal_type: "Mammal",
  active_time: "Diurnal",
  habitat: "High plateau",
};

const renderComponent = () => render(<InfoTile {...infoTileMock} />);

describe("given the InfoTile component is rendered", () => {
  it("then should contain a title", () => {
    renderComponent();

    expect(screen.getByRole("heading", { name: "Llama" })).toBeInTheDocument();
  });

  it("then should contain an image with the expect alt text and source", () => {
    renderComponent();

    expect(screen.getByRole("img", { name: "Llama" })).toHaveAttribute(
      "src",
      "https://upload.wikimedia.org/wikipedia/commons/c/cd/Black_Llama.jpg"
    );
  });

  it("then should contain all 4 bullet points with expected text in the correct order", () => {
    renderComponent();

    const listItems = screen.getAllByRole("listitem");

    expect(listItems).toHaveLength(4);
    expect(listItems[0]).toHaveTextContent("Latin Name: Lama glama");
    expect(listItems[1]).toHaveTextContent("Animal Type: Mammal");
    expect(listItems[2]).toHaveTextContent("Active Time: Diurnal");
    expect(listItems[3]).toHaveTextContent("Habitat: High plateau");
  });
});
```

#### Next: [2. Advanced Components](../2.advanced-components/README.md)
