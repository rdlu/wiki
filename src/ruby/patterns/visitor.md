# Visitor

*Objective*: Separate Auxiliary procedures and algorithms from the object they operate.

We can consider the Visitor pattern a more complex version of double dispatch. It uses the double dispatch principle to make it work.

On top of double dispatch, Visitor lets you add “external” operations to a whole class hierarchy without changing the existing code of these classes.

Visitor isn’t a very common pattern because of its complexity and narrow applicability.

Related to [Double Dispatch](avoid-case.md).
[Refactoring Guru](https://refactoring.guru/design-patterns/visitor/ruby/example#lang-features)

## Applications

- Use the Visitor when you need to perform an operation on all elements of a complex object structure (trasversing a tree).
- Use the Visitor to clean up the business logic of auxiliary behaviors.

## Example

```ruby
class Node
  def accept visitor
    raise NotImpelementedError.new
  end
end

module Visitable
  def accept visitor
    visitor.visit(self)
  end
end

class IntegerNode < Node
  include Visitable

  attr_reader :value
  def initialize value
    @value = value
  end
end

class StringNode < Node
  include Visitable

  attr_reader :value
  def initialize value
    @value = value
  end
end

class Ast < Node
  def initialize
    @nodes = []
    @nodes << IntegerNode.new(2)
    @nodes << StringNode.new("3")
  end

  def accept visitor
    @nodes.each do |node|
      node.accept visitor
    end
  end
end

class BaseVisitor
  def visit subject
    method_name = "visit_#{subject.class}".intern
    send(method_name, subject )
  end
end

class DoublerVisitor < BaseVisitor
  def visit_IntegerNode subject
    puts subject.value * 2
  end

  def visit_StringNode subject
    puts subject.value.to_i * 2
  end
end

class TriplerVisitor < BaseVisitor
  def visit_IntegerNode subject
    puts subject.value * 3
  end

  def visit_StringNode subject
    puts subject.value.to_i * 3
  end
end

ast = Ast.new
puts "Doubler:"
ast.accept DoublerVisitor.new
puts "Tripler:"
ast.accept TriplerVisitor.new

# =>
Doubler:
4
6
Tripler:
6
9
```

## Real World Usage 

Arel uses this pattern to build specific database queries. See [Arel PostgreSQL](https://github.com/rails/rails/blob/master/activerecord/lib/arel/visitors/postgresql.rb)