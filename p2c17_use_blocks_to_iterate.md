#### Chapter 17. Use Blocks to Iterate
1. Syntax:
  ```ruby
  do_something do
    puts "Hello from inside the block"
  end

  do_something { puts "Hello from inside the block" }
  ```
  > When you tack a block onto the end of a method call, Ruby will package up the block as sort of a secret argument and (behind the scenes) passes this secret argument to the method. Inside the method you can detect whether your caller has actually passed in a block with the `block_given?` method and fire off the block (if there is one) with `yield`:

  ```ruby
  def do_something
    yield if block_given?
  end
  ```
  Blocks can take arguments, which you supply as arguments to yield, so that if you do this:
  ```ruby
  def do_something_with_an_arg
    yield("Hello World") if block_given?
  end

  do_something_with_an_arg do |message|
    puts "The message is #{message}"
  end
  ```
  you will get: `The message is Hello World`.
  Code blocks always return a value-the last expression that the block executes.
  ```ruby
  def print_the_value_returned_by_the_block
    if block_given?
      value = yield
      puts "The block returned #{value}"
    end
  end

  print_the_value_returned_by_the_block { 3.14159 / 4.0 }
  ```
2. Iterators:
  ```ruby
  class Document
    def each
      words.each { |word| yield( word ) }
    end

    def each_character
      index = 0
      while index < @content.size
        yield( @content[index] )
        index += 1
      end
    end
  end

  d = Document.new('Truth', 'Gump', 'Life is like a box of ...')
  d.each { |word| puts word }
  ```
  Ruby convention: name your most obvious or commonly used iterator `each` and give any other iterators a name like `each_something_else`.

3. Iterating over the Ethereal
  ```ruby
  class Document
    def each_word_pair
      word_array = words
      index = 0
      while index < (word_array.size - 1)
        yield word_array[index], word_array[index+1]
        index += 1
      end
    end
  end

  doc = Document.new('Donuts', '?', 'I love donuts mmm donuts')
  doc.each_word_pair{ |first, second| puts "#{first} #{second}" }
  ```
  Notice that we never actually build a four-element array of all the word pairs: We simply generate the pairs on the fly.
4. Enumerable. How to use: include `enumerable` and provide an `each` method:
  ```ruby
  class Document
    include Enumerable

    def each
      words.each { |word| yield( word ) }
    end
  end
  ```
  `Enumerable` will supply your class with: `include?`, `to_a`, `find`, `find_all`, `each_cons`, `each_slice` ... about 40 methods.
  ```ruby
  def each_word_pair
    words.each_cons(2) { |array| yield array[0], array[1] }
  end
  ```
  If your Collection define the `<=>` operator, you can use the Enumerable-supplied `sort` method.
5. Enumerator takes a collection and the name of the iterating method:
  ```ruby
  doc = Document.new('Donuts', '?', 'I love donuts mmm donuts')
  enum = Enumerator.new(doc, :each_character)
  ```
  You end up with an object with all of the nice `Enumerable` methods based on the `each_character` method.
6. > The primary way that an iterator method can come to grief is by trusting the block too much. Remember, the code block you get handed in your iterator method is someone else's code. You need to regard the block as something akin to a hand grenade, ready to go off at any second.
  ```ruby
  def each_name
    name_server = open_name_server   
    begin
      while name_server.has_more?
         yield name_server.read_name
      end
    ensure
      name_server.close              
    end
  end
  ```
  use `ensure` so no matter `break` or `raise` exceptions, the `name_server` will always be closed in the end.
7. `ObjectSpace.each_object` will iterate through all the objects in your ruby interpreter. `ObjectSpace.each_object(String)` will iterate through all the String objects in your ruby interpreter.
