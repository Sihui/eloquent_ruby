#### Chapter 13. Get the Behavior You Need with Singleton and Class Method
1. A singleton method is a method that is defined for exactly one object instance.
  - it has nothing to do with the Singleton design pattern; just a name collision
2. You can hang a singleton method on just about any object at any time
  - beside numeric classes and symbols, neither of which support singleton method
3. Syntax:
  ```ruby
  my_obj = Object.new

  def my_obj.avaliable?
    true
  end

  def my_obj.render( content )
    nil
  end
  ```
  Or
  ```ruby
  my_obj = Object.new
  class << my_obj
    def avaliable?
      true
    end

    def render( content )
      nil
    end    
  end
  ```
4. Singleton class, aka metaclass/eigenclass, sits between every object and its regular class. The singleton class starts out as just a methodless shell and is therefore invisible. It's only being lazy created whenever needed.
5. ```ruby
  singleton_class = class << my_obj
    self
  end
   ```
   When you do the `class << my_obj`, you change the context so that self is the singleton class. Since class definitions, like most Ruby expressions, returns the last thing they evaluate, simply sticking self inside the class definition causes the whole class statement to return singleton class.
6. **A class method is just a singleton method on an instance of Class.**
  ```ruby
  def Document.explain
    puts "self is #{self}" # self is Document
    puts "and its class is #{self.class}" # and its class is Class
  end
  ```
  that's why class method can also be defined as:
  ```ruby
  class Document
    class << self
      def find_by_name( name )
        # find a document by name
      end
      def find_by_id( name )
        # find a document by id
      end
  end
  ```
7. Class methods are the perfect home for the code that is related to a class but independent of any given instance of the class. A common use for class methods is to provide alternative methods for constructing new instances, ie `xmas = Date.civil( 2010, 12, 25 )` **If you have many different ways that you might create an object, a set of well-named class methods is generally clearer than making the user supply all sorts of clever arguments to the new method.**
8. You can see all the singleton methods by `my_obj.singleton_methods`
9. You couldn't call class method on an instance, cause when you define a class method, it is a method that attached to a class. Instances of the class will not know anything about that method:
    ```ruby
    Document.a_class_method # works
    doc.a_class_method # won't work
    doc.class.a_class_method # works
    ```
10. `self` is always the thing before the period when you called that method.
  ```ruby
  class Parent
    def self.who_am_i
      puts self
    end
  end
  
  class Child < Parent
  end

  Parent.who_am_i # Parent
  Child.who_am_i # Child
  ```
