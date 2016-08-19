#### Chapter 19. Save Blocks to Execute Later
1. Explicit Blocks: if you add a parameter prefixed with an ampersand to **the end of** your parameter list, Ruby will turn any block passed into the method into a garden-variety parameter and you can called it by `call`.
  ```ruby
  def run_that_block( &that_block )
    puts 'About to run the block'
    that_block.call if that_block
    puts 'Done running the block'
  end
  ```
  > When you use explicit block parameters, you can hold onto the block and store a reference to it like any other object.

  ```ruby
  class Document             
    def on_save( &block )
      @save_listener = block
    end

    def on_load( &block )
      @load_listener = block
    end

    def load( path )
      @content = File.read( path )
      @load_listener.call( self, path ) if @load_listener
    end

    def save( path )
      File.open( path, 'w' ) { |f| f.print( @contents ) }
      @save_listener.call( self, path ) if @save_listener
    end
  end

  my_doc = Document.new( 'Block Based Example', 'russ', '' )  

   my_doc.on_load do |doc|
     puts "Hey, I've been loaded!"
   end

   my_doc.on_save do |doc|
     puts "Hey, I've been saved!"
   end                                                             
  ```
2. Saving Code Blocks for Lazy Initialization
  ```ruby
  class BlockBasedArchivalDocument
    attr_reader :title, :author

    def initialize(title, author, &block)
      @title = title
      @author = author
      @initializer_block = block
    end

    def content
      if @initializer_block
        @content = @initializer_block.call
        @initializer_block = nil
      end
      @content
    end
  end                                          
  ```
  With this, we can get our content from a file, http, or wherever:
  ```ruby
  file_doc =  BlockBasedArchivalDocument.new( 'file', 'russ' ) do
      File.read( 'some_text.txt' )
  end  

  google_doc = BlockBasedArchivalDocument.new('http', 'russ') do   
    Net::HTTP.get_response('www.google.com', '/index.html').body
  end    

  boring_doc = BlockBasedArchivalDocument.new('silly', 'russ') do      
    'Ya' * 100
  end       
  ```
3. `lambda`: you pass it a code block and the method will pass the corresponding `Proc` object right back to you.
  ```ruby
  class Document                                                   
    DEFAULT_LOAD_LISTENER = lambda do |doc, path|
      puts "Loaded: #{path}"
    end

    DEFAULT_SAVE_LISTENER = lambda do |doc, path|
      puts "Saved: #{path}"
    end

    attr_accessor :title, :author, :content

    def initialize( title, author, content='' )
      @title = title
      @author = author
      @content = content
      @save_listener = DEFAULT_SAVE_LISTENER
      @load_listener = DEFAULT_LOAD_LISTENER
    end

    # Rest of the class omitted...
  end                                                    
  ```
4. `Proc.new`, ie`from_proc_new = Proc.new { puts 'hello from a block' }`, vs `lambda`:
  1. `Proc.new` object is very forgiving of the number of arguments passed to its call method: pass too few and it will set the excess block parameters to `nil`; pass too many and it will quietly ignore the extra arguments. In contrast, the call method on an object returned by `lambda` acts more like a regular method and will throw an exception  if you mess up the argument count.
  2. Objects from `Proc.new` feature all of the interesting `return`, `break`, and `next` behavior that we touched on the last couple chapters. ie: a `return` inside a `Proc.new` block will not just return from the block but from the method that created the block. In contrast, the `Proc` object returned from `lambda` acts more like a portable method-a return from a `lambda` wrapped block will simply return from the block and no further.
    - if you are calling a method that takes a block, pause for a second before you put a `return`, `break`, or `next` in that block and think if it make sense to put it there
    - if you want a block object that behaves like the ones that Ruby generates when you pass a couple of braces into a method, use `Proc.new`. If you want something that will behave more like a regular object with a single method, use `lambda`.
5. Be aware of block scope:
  ```ruby
  def some_method(doc)                         
    big_array = Array.new(10000000)

    # Do something with big_array...

    # And now get rid of it so it doesn't hang there for long!
    big_array = nil

    doc.on_load do |d|
      puts "Hey, I've been loaded!"
    end
  end                                         
  ```
