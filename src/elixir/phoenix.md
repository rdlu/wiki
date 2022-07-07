# Phoenix Framework

## LiveView

### Powerful components

`Render Slot` is a powerful tool to render many similar cells inside a `component`

Extremely useful for lists, table rows, table cells, repeatable items that needs to be inside another, like with `ul>li`, `tr>td`, etc.

https://hexdocs.pm/phoenix_live_view/0.17.10/Phoenix.LiveView.Helpers.html#render_slot/2

```html
<.unordered_list let={item} items={~w(Apples Oranges Bananas)}>
```

```elixir
def unordered_list(assigns) do
  ~H"""
  <ul>
  <%= for item <- @items do %>
    <li><%= render_slot(@inner_block, item)%></li>
  <% end %>
  </ul>
  """
end
```

More on https://hexdocs.pm/phoenix_live_view/Phoenix.Component.html
