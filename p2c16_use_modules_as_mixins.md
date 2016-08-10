#### Chapter 16. Use Modules as Mixins
1. Use *instance* methods, not *module-level* methods (started with self.), if you want the methods to be mixined. Ex:
  ```ruby
  module WritingQuality                    

    CLICHES = [ /play fast and loose/,
                /make no mistake/,
                /does the trick/,
                /off and running/,
                /my way or the highway/ ]

    def number_of_cliches                    
      CLICHES.inject(0) do |count, phrase|
        count += 1 if phrase =~ content
        count
      end
    end                                      
  end

  class Document
    include WritingQuality
  end

  my_tome = Document.new('a', 'b', 'c')
  puts my_tome.number_of_cliches                              
  ```
2. For class methods, instead of instance-level methods:
  ```ruby
  class Document
    class << self
      include MyModule
    end
  end
  # OR
  class Document
    extend MyModule
  end
  # then you can do
  Document.some_method_in_the_module
  ```
  This code includes the module into the singleton class of Document, effectively making the methods of MyModule singleton-and therefore class-methods of Document.
3. **When you mix a module into a class, Ruby rewires the class hierarchy a bit, inserting the module as a sort of pseudo superclass of the class. The module gets interposed between the class and its original superclass.** This explains how the module methods appear in the including classes-they effectively become methods just up the inheritance chain from the class.
  - So you can override methods in a module you mixined:
    ```ruby
    class Document                     ##(override
      include WritingQuality

      # Rest of the class omitted...
    end

    class PoliticalBook < Document
      def number_of_cliches
        0
      end

      # Rest of the class omitted...
    end                                ##override)
    ```
  - or override with another module: the one that's most recently included always win.

    ```ruby
    module PoliticalWritingQuality                
      def number_of_cliches
        0
      end
    end                                         

    class Document
      include WritingQuality

      # Rest of the class omitted...
    end

    class PoliticalBook < Document              
      include WritingQuality
      include PoliticalWritingQuality

      # Lots of stuff omitted...
    end                                          
    ```
4. No matter how many modules a class includes, instances of the class with still claim to be instances of the class.
5. `kind_of?`, ie `my_tome.kind_of?(WritingQuality)`, will tell you whether the class of an instance includes a given module.
6. `ancestors` gives you the complete inheritance ancestry in an array, ie `[Document, WritingQuality, Object, Kernel, BasicObject]`.
7. Whenever writing a module, always think about:**What is the interface between my module and its including class? Since mixing in a module sets up an inheritance relationship between the including class and the module, you need to let your users know what that relationship is going to be before they start mixing. You should always add a few concise comments to your creation stating exactly what it expects from its including class:**
  ```ruby
  # Methods to measure writing quality.
  # Uses the content method of the including class.
  module WritingQuality
    # ...
  end
  ```
8. You can also stash constants in modules:
  ```ruby
  module ErrorCode
    OK         =  0   # Successful result
    ERROR      =  1   # SQL error or missing database
    INTERNAL   =  2   # An internal logic error in SQLite
    PERM       =  3   # Access permission denied
    ABORT      =  4   # Callback routine requested an abort
    BUSY       =  5   # The database file is locked
    LOCKED     =  6   # A table in the database is locked

    # Seemingly endless list of remaining error codes omitted...
  end
  ```
