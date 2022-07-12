# Elixir language

## List comprehensions

### Filtering

```elixir
animals = [ {"Liz", :dog}, {"Margot", :cat} ]

# pattern match
for {name, :cat} <- prefs, do: name
# ["Margot"]

# guard clause
for {name, pet_choice} <- animals, pet_choice == :dog, do: name
# ["Liz"]

# anonymous function in a guard clause
cat_lover? = fn(choice) -> choice == :cat end
for {name, pet_choice} <- prefs, cat_lover?.(pet_choice), do: name
# ["Margot"]

# destructuring plus filtering
pets = [
  %{name: "Fluffy", species: :cat, sleeping: true},
  %{name: "Joe", species: :dog, sleeping: false}
]
for pet <- pets,
    name = pet.name,
    sleeping = pet.sleeping do
  "Is #{name} sleeping? #{sleeping}"
end
# => ["Is Fluffy sleeping? true"]

# applying for each list
for i <- 1..2, IO.inspect(i), j <- 5..6 do
  {i, j}
end

for i <- 1..10, Integer.is_even(i), j <- 5..6 do
  {i, j}
end
# [{2, 5}, {2, 6}, {4, 5}, ...
```

### Atomize keys - `into`

You can use `into` and `do` option from comprehensions to achieve this:

```elixir
style = %{"color" => "#FFF", "display" => "flex", "border" => "2px"}
for {key, val} <- style, into: %{}, do: {String.to_atom(key), val}
# %{border: "2px", color: "#FFF", display: "flex"}
```

### Cartesian product

```elixir
names = ~w[John Jane]
surnames = ~w[Doe Silva]

for name <- names,
    surname <- surnames do
  "#{name} #{surname}"
end
```

### Unique

```elixir
for n <- [1, 1, 2, 2, 2, 3], uniq: true do
  n
end
```

### Reduce

```elixir
pets = [
  %{name: "Fluffy", species: :cat, sleeping: true},
  %{name: "Joe", species: :dog, sleeping: false},
  %{name: "Joe", species: :cat, sleeping: false}
]

for pet <- pets, reduce: %{} do
  acc ->
    Map.update(acc, pet.name, [pet], &[pet|&1])
end

# Groups by name, returns a new map
%{
  "Fluffy" => [%{name: "Fluffy", sleeping: true, species: :cat}],
  "Joe" => [
    %{name: "Joe", sleeping: false, species: :cat},
    %{name: "Joe", sleeping: false, species: :dog}
  ]
}
```

### Filter, map at same time

```elixir
for %{sleeping: false} = pet <- pets, into: %{} do
  {pet.name, pet}
end

# returns
%{"Joe" => %{name: "Joe", sleeping: false, species: :cat}}
```
