# Double Dispatch

*Objective*: Turn a type checking imperative procedure in a more maintenable OO version.

Related to [Visitor Pattern](visitor.md).

Some differences between the two on [Refactoring Guru](https://refactoring.guru/design-patterns/visitor-double-dispatch)

## Applications

- Avoiding `case` for type checking in Ruby to comply with polymorphic collections
- Use the Visitor when you need to perform an operation on all elements of a complex object structure (trasversing a tree). Like Visitor

## One Example

Using a simple game, Rock, Paper, Scissors, like in [RubyGuides](https://www.rubyguides.com/2017/04/stop-using-case-statements-in-ruby/) we can start understand why:

```ruby
class Scissors
    def wins_against?(other_move)
        case other_move
        when Rock then false
        when Paper then true
        end
    end
end
```

We want to avoid that example of case with this:

```ruby
class Scissors
    def wins_against?(other_move)
        other_move.do_you_beat_scissors?
    end
    def do_you_beat_rock?
        false
    end
    def do_you_beat_paper?
        true
    end
end
```

## Another example 

```ruby
# Hosts
class Car
    def dispatch_maintenance(caretaker)
      caretaker.maintenance_on_car(self)
    end
end

class Truck
    def dispatch_maintenance(caretaker)
      caretaker.maintenance_on_truck(self)
    end
end

# Visitors
class Mechanic
    def work_on(vehicle)
        vehicle.dispatch_maintenance(self)
    end
    def maintenance_on_car(vehicle)
        change_motor_oil(vehicle, tools: LightTools.grab())
    end
    def maintenance_on_truck(vehicle)
        change_motor_oil(vehicle, tools: HeavyTools.grab())
        check_tires_condition(vehicle) && change_tires(vehicle)
    end
end

class CarWasher
    def work_on(vehicle)
       vehicle.dispatch_work(self)
    end
    def maintenance_on_car(vehicle)
        grab_some_cleaning_cloth()
        do_wash(vehicle, using: LightMachine)
    end
    def maintenance_on_truck(vehicle)
        check_water_reservoir()
        do_wash(vehicle, using: HeavyMachine)
    end
end

mechanic = Mechanic.new
mechanic.work_on(vw_golf)
```

Here we see caretakers `[Mechanic, CarWasher]` visiting the patients instances `[Car, Truck]`. The hosts are responsible to dispatch the correct action for caretakers. It's odd on reality of this example.


## Using a in View

Now from [Sandi Metz's blog](https://sandimetz.com/blog/2009/06/12/ruby-case-statements-and-kind-of).

```ruby
class MyView
  attr_reader :target
  
  def initialize(target)
    @target = target
  end

  def double
    case target
      when Numeric  then target * 2 
      when String   then target.next    # lazy example fail
      when Array    then target.collect {|o| MyView.new(o).double}
      else
        raise “don’t know how to double #{target.class} #{target.inspect}”
    end
  end
end
```

```ruby
class Numeric
  def double
    self * 2
  end
end

class String
  def double
    self.next 
  end
end

class Array
  def double
    collect {|e| e.double}
  end
end

class Object
  def double
    raise "don't know how to double #{self.class} #{self.inspect}"
  end
end

class MyView
  attr_reader :target
  
  def initialize(target)
    @target = target
  end

  def double
    target.double
  end
end
```