#### Chapter 18. Execute Around with a Block
1. Code Block: **delivery code where it is needed**.
  > The simple "bury the details in a method that takes a block" technique goes by the name of **execute around**. Use execute around when you have something-like logging-that needs to happen before or after some operation, or when the operation fails with a exception. Instead of laboriously sprinkling intention-obscuring code far and wide, you build a method that takes a code block. Inside the method you do whatever preparation needs doing. Then you call the block, followed by any clean-up work. You can (and probably should) also catch any exceptions that come roaring outOf the block an do the right thing with them.

  ```ruby
  class SomeApplication        
    def do_something
      with_logging('load') { @doc = Document.load( 'resume.txt' ) }
      with_logging('save') { @doc.save }
    end

    def with_logging(description)
      begin
        @logger.debug( "Starting #{description}" )
        yield
        @logger.debug( "Completed #{description}" )
      rescue
        @logger.error( "#{description} failed!!")
        raise
      end
    end
  end

  ##or
  def log_before( description )                  ##(before
    @logger.debug( "Starting #{description}" )
    yield
  end                                           ##before)

  def log_after( description )                  ##(after
    yield
    @logger.debug( "Done #{description}" )
  end                                           ##after)                               
  ```
2. Setting Up Objects with an Initialization Block:
  ```ruby
  class Document                            ##(main
    attr_accessor :title, :author, :content

    def initialize(title, author, content = '')
      @title = title
      @author = author
      @content = content
      yield( self ) if block_given?
    end

    # Rest of the class omitted...
  end                                       ##main)

  new_doc = Document.new( 'US Constitution', 'Madison', '' ) do |d| ##(const
     d.content << 'We the people'
     d.content << 'In order to form a more perfect union'
     d.content << 'provide for the common defense'
   end                                                                 ##const)
  ```
  > Using execute around for initialization is generally less about making sure that things happen in a certain sequence and more about making the code readable. *Here, it says, is the code that sets up the new object.*
3. **All of the variables that are visible just before the opening `do` or `{` are still visible inside the code block. Code blocks drag along the scope in which they were created wherever they go.** That doesn't mean that respectable execute around methods don't take any arguments. **A good rule of thumb is that the only arguments you should pass from the application into an execute around method are those that the execute around method itself, not the block, will use.**
4. The execute around can also pass things back to the block:
  ```ruby
  def with_database_connection( connection_info )  
    connection = Database.new( connection_info )
    begin
      yield( connection )
    ensure
      connection.close
    end
  end

  # or carrying the answers back
  def with_logging(description)                         
   begin
     @logger.debug( "Starting #{description}" )
     return_value = yield
     @logger.debug( "Completed #{description}" )
     return_value
   rescue
     @logger.error( "#{description} failed!!")
     raise
   end
  end                                                                                            
  ```
5. Execute around is all about guarantees, so think about exceptions when writing blocks.
6. **Naming matters**: don't think of it as naming a new method, but as naming a new feature that you are adding to the Ruby language. Cause it really is like that: after defining the `with_logging` method, you now have a language that not only will let you conditionally execute code with an `if` statement and repeatedly execute some code in a `while` loop, but also will let you execute some code wrapped in logging with `with_logging`.
