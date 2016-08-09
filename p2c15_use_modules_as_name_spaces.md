#### Chapter 15. Use Modules as Name Spaces

1. Classes are:
  1. factories that produce objects
  2. containers - most of the effort that goes into creating a new class actually goes into putting things like methods and constants into the class.
2. A Ruby module is the container part of a class without the factory:
  - you can't instantiate a module, but you can put things inside of a module.
  - module can hold methods, constants, classes, and even other modules.
3. Using modules:
  1. allows you to group together related classes
  2. dramatically reduces the probability of class name collisions
  3. can enclose individual methods that don't seem to fit anywhere else
4. Ex:
  ```ruby
  module Rendering                            
    DEFAULT_FONT = Font.new( 'default' )
    DEFAULT_PAPER_SIZE = PaperSize.new

    class Font
      attr_accessor :name, :weight, :size

      def initialize( name, weight=:normal, size=10 )
        @name = name
        @weight = weight
        @size = size
      end
      # Rest of the class omitted...
    end

    class PaperSize
      attr_accessor :name, :width, :height

      def initialize( name='US Let', width=8.5, height=11.0 )
        @name = name
         @width = width
        @height = height
      end
      # Rest of the class omitted...
    end

    def self.points_to_inches( points )
      points / 72.0
    end

    module AnotherModule
      # ...
    end
  end                                      
  ```
  you can use stuff in the module
    - by `Rendering::Font.new('default')` or first `include Rendering` then call them directly
    - for methods: `Rendering.points_to_inches(10)`, the above `::` syntax also works for methods, but not preferred
5. You can define your modules in several pieces, spread over a number of source files. The first file defines the module and the rest of the files simply add to it. Ie:
  ```ruby
  # in font.rb
  module Rendering
    class Font
      def initialize( name, weight=:normal, size=10 )
      end
    end
  end
  # in paper_size.rb
  module Rendering
    class PaperSize
      def initialize( name='US Let', width=8.5, height=11.0 )
      end
    end
  end
  ```
  If we then pull both files into our Ruby interpreter with suitable require statements:
  ```ruby
  require `font`
  require `paper_size`
  ```
  we will have a single Rendering module, complete with fonts and paper sizes.
6. **Modules are objects**
  - we can point a variable at a module and then use that variable in place of the module.
  - you can swap out whole groups of related classes and constants-and even sub-modules!-at runtime
  ```ruby
  module TonsOToner                         
    class PrintQueue
      def submit( print_job )
        # Send the job off for printing to this laser printer
      end

      def cancel( print_job)
        # Stop!
      end
    end

    class Administration
      def power_off
        # Turn this laser printer off...
      end

      def start_self_test
        # Everything ok?
      end
    end
  end                                        

  module OceansOfInk                        
    class PrintQueue
      def submit( print_job )
        # Send the job off for printing to this ink jet printer
      end
      # Rest omitted...
    end

    class Administration
      #  Ink jet administration code omitted...
    end
  end                                       

  def make_admin( use_laser_printer )
    if use_laser_printer               
      print_module = TonsOToner
    else
      print_module = OceansOfInk
    end

    # Later...

    admin = print_module::Administration.new  
  end
  ```
7. **When defining module-level methods: use `def self.method_name` not `def method_name`**
  - the first one creates a module-level method that any code can use, the second one creates a method that might be great when mixed into a class but is useless as a widely available utility method.
8. Don't nest modules too deep.
