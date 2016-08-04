#### Chapter 3. Take Advantage of Ruby's Smart Collections

1. Use `%w{}` to construct an array of strings:
  ```ruby
  words = {'one', 'two', 'three'} # is the same as:
  words = %w{ one, two, three }
  ```
2. Create a Hash:
  ```ruby
  # this form doesn't care about the types of the hash keys
  freq = { "I" => 1, "don't" => 1, "like" => 1, "spam" => 963 }

  # if hash keys are Symbols, then this:
  book_info = {:first_name => 'Russ', :last_name => 'Olsen'}
  # can be cut to this:
  book_info = {first_name: 'Russ', last_name: 'Olsen'}
  ```
3. Default value for (optional) arguments: `def order_book( book_id, quantity = 1 )`
4. `*` will soak up any extra arguments passed to the method into an array:
  ```ruby
  def each_at_least_two( first_arg, *midle_args, last_arg )
    puts "The first argument is #{first_arg}"
    middle_args.each { |arg| puts "A middle argument is #{arg}"}
    puts "The last argument is #{last_arg}"
  end

  # without *
  def add_authors ( names )
    @authors += " #{names.join(' ')}"
  end
  # Need to pass an array into the method
  doc.add_authors( ['Strunk', 'White'] )

  # with *
  def add_authors ( *names )
    @authors += " #{names.join(' ')}"
  end
  # args will be turn into an array automatically
  doc.add_authors( 'Strunk', 'White' )    
  ```
5. `*` can turn an array into individual parameters when calling a method:
  ```ruby
  args = [arg1, arg2, arg3]
  a_method_that_takes_three_args( *args )
  ```
6. When a hash is passed at the end of the argument list, it doesn't have to be wrap around `{}`.
  ```ruby
  def load_front( specification_hash )
    # load
  end

  load_front name: 'times roman', size: 12 #same as load_front({name: 'times roman', size: 12})
  ```
7. `each` for Hash
    ```ruby
    movie = { title: '2001', genre: 'sic fi', rating: 10 }

    movie.each { |entry| ap entry}
    # it puts
    # [:title, "2001"]
    # [:genre, "sic fi"]
    # [:rating, 10]

    movie.each { |key, value| puts "#{key} => #{value}" }
    ```
8. `find_index` / `index` for array:
    - uses `==`
    - when it takes an argument, it returns the index of the first object in the array, or `nil` if not found
    - when it takes a block, returns the index of the first object for which the block returns true or nil if no match is found
    ```ruby
    a = [ "a", "b", "c" ]
    a.index("b")              #=> 1
    a.index("z")              #=> nil
    a.index { |x| x == "b" }  #=> 1
    ```
9. `map` for array: it takes a block, runs each element in the array to that block, and returns a new array containing everything that block returns in order
    `lower_case_words = words.map { |word| word.downcase }`
10. `inject` for array:
      > Like each, inject takes a block and calls the block with each element of the collection.
      Unlike each, inject passes in two arguments to the block: along with each element, you get the current result.
      Each time inject calls the block, it replaces the current result with the current value of the previous call to the block.
      When inject runs out of elements, it returns the result as its return value.
      If you don't supply an initial value, inject will skip calling the block for the first element of the array and simply use that element as the initial result.

    ```ruby
    def average_word_length
      total = words.inject(0.0) { |result, word| word.size + result }
      total / words.length
    end
    ```  
11. `!` indicates the method is the dangerous or surprising version of a pair of method:
    - `my_array.reverse!` reverses the original array while `my_array.reverse` returns a new array
    - `my_array.sort!` sort the original array while `my_array.sort` returns a new array
12. Hash is ordered in Ruby:
    - new added items will be put to the end
    - updated items will stay in there original places
13. Do **NOT** add or delete elements from an array when iterating through it.
    ```ruby
    array = [ 0, -10, -9, 5, 9 ]
    array.each_index { |i| array.delete.at(i) if array[i] < 0}
    ap array # [0, -9, 5, 9]
    ```

14. This will create 2021 new elements of array, most of them set to nil.
  ```ruby
  array = []
  array[2020] = "Sparkling Water"
  ```    
15. `array.delete_if { |x| x < 0 }`
16. Set
    ```ruby
    require 'set'

    word_set = Set.new( words )
    ```
