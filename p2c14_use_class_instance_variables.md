#### Chapter 14. Use Class Instance Variables

1. Class variables
  - start with `@@`
  - not visible to the outside world but visible to instances of the class
  - how they are resolved: **if the class variable is not defined in the current class, Ruby will go looking up the inheritance tree for it.**
    > So if there is no `@@default_paper_size` in the current class, Ruby will look for one in the superclass and the the super superclass, until it either finds a `@@default_paper_size` defined on one of those classes or runs out of classes. If Ruby does find `@@default_paper_size` somewhere in the inheritance tree, that's the one that gets set. If Ruby runs out of classes without finding `@@default_paper_size`, then it will create a new class variable in the current class. Looking up the value of a class variable works pretty much the same way. Ruby starts with the current class and looks up the inheritance tree, it either finds the variable or runs out of classes and throws a `NameError` exception.

  - **The problem: this method for resolving class variables means they have a tendency to wander from class to class.** Ex:

  ```ruby
  class Resume < Document
    @@default_font = :arial

    def self.default_font=(font)
      @@default_font = font
    end

    def self.default_font
      @@default_font
    end

    attr_accessor :font

    def initialize
      @font = @@default_font
    end

    # Rest of the class omitted...
  end

  class Presentation < Document
    @@default_font = :nimbus

    def self.default_font=(font)
      @@default_font = font
    end

    def self.default_font
      @@default_font
    end

    attr_accessor :font

    def initialize
      @font = @@default_font
    end

    # Rest of the class omitted...
  end
  ```

  `Resume` class and `Presentation` class have their own `@default_font` variables attached to each of these two classes. But when, ignorant of things going on in `Resume` and `Presentation`, you decide to add a `@@default_font` to `Document`:
  ```ruby
  class Document
    @@default_font = :times

    # Rest of the class omitted...
  end
  ```
  Instead of two separate default font variables, one attached to `Presentation` and the other to `Resume`, there is now only one variable, one that lives up in the `Document` class.
  ```ruby
  require 'document'
  require 'resume'
  require 'presentation'
  ```

  This sets the default font for all document to Presentation's default font: `:nimbus`

  ```ruby
  require 'document'
  require 'presentation'
  require 'resume'
  ```
  This sets the default font for all document to Resume's default font: `:arial`

  > The real problem with class variables is that they are not so much variables attached to a specific class as they are global variables with a slightly restricted realm. In fact, Rubyist David Black calls class variable "vertical global variables", vertical in that they are restricted to a single inheritance tree and global in that they are very visible within that tree.

2. Class Instance Variables: an instance variable on an object which is a class.
    ```ruby
    class Document                   

      attr_accessor :font

      @default_font = :times

      def self.default_font=(font)
        @default_font = font
      end

      def self.default_font
        @default_font
      end

      def initialize(title, author)    
        @title = title
        @author = author
        @font = Document.default_font
      end                               
    end
    ```
    You can use `attr_accessor` so you don't have to write `getter` and `setter`:
    if you only do
    ```ruby
    class Document
      attr_accessor :default_font
    end
    ```
    you will be able to get and set an **instance** variable called `default_font`, but

    ```ruby
    class Document
      @default_font = :times

      class << self
        attr_accessor :default_font
      end
    end
    ```
    you will be able to get and set a **class instance** variable on `Document`.

3. **Use class instance variables!!!**
    - if you have to use class variables, set them very early on.  
