# Elixir tips and TILs

## List comprehensions

```elixir
animals = [ {"Liz", :dog}, {"Margot", :cat} ]
for {name, :cat} <- prefs, do: name
# ["Margot"]
for {name, pet_choice} <- animals, pet_choice == :dog, do: name
# ["Liz"]
cat_lover? = fn(choice) -> choice == :cat end
for {name, pet_choice} <- prefs, cat_lover?.(pet_choice), do: name
# ["Margot"]
```

### Atomize keys

You can use `into` and `do` option from comprehensions to achieve this:

```elixir
style = %{"color" => "#FFF", "display" => "flex", "border" => "2px"}
for {key, val} <- style, into: %{}, do: {String.to_atom(key), val}
# %{border: "2px", color: "#FFF", display: "flex"}
```
