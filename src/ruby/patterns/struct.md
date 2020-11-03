# Ruby Struct

## Structs: Simple Objects

Objective: Fancy hashes that responds to attributes names as a function, like in JS Objects

```ruby
Car = Struct.new(:model, :maker, :year, keyword_init: true)

vw_up = Car.new model: 'Up!', year: 2015, maker: 'VW'
polo = Car.new('Polo', 'VW', 2018)
golf = Car.new 'Golf', 'VW'


polo.maker
# "VW"
golf.year
# nil
```

**Equality**: It uses their attributes instead object ids like default classes

```ruby
# vw_up2 = Car.new model: 'Up!', maker: 'VW', year: 2015
vw_up == vw_up2
# True
```

### OpenStruct: Simpler alternative

```ruby
require 'ostruct'

cat = OpenStruct.new(color: 'black')
puts cat.class
puts cat.color
```

#### Differences from Struct

- Struct creates a new class with predefined attributes, equality method (==) & enumerable
- OpenStruct creates a new object with the given attributes