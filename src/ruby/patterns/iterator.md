# Iterator


```ruby
# It's just a class to demonstrate a collection capability that answers to enumerable idiom
class ACollection
  include Enumerable

  # @return [Array]
  attr_accessor :collection
  private :collection


  # @param [Array] collection
  def initialize(collection = [])
    @collection = collection
  end

  def each(&block)
    @collection.each(&block)
  end

  def <<(item)
    @collection << item
  end

  def append(item)
    self << item
  end
end


collection = ACollection.new
collection.append('First')
collection << 'Second'
collection.append('Third')

puts 'Straight traversal:'
collection.each { |item| puts item }
puts "\n"

puts 'Reverse traversal:'
collection.reverse_each { |item| puts item }
```