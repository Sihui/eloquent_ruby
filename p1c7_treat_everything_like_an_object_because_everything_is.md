#### Chapter 7. Treat Everything Like an Object - Because Everything Is

1. Classes provide two things:
  1. classes act as containers for methods
  2. classes are also factories for making instances
2. During a method call, Ruby sets **self** to the instance that you called the method on
  ```ruby
  class Document
    def about_me
      puts "I am #{self}"
      puts  "My title is #{self.title}"
    end
  end

  doc.about_me
  # I am #<Document: 0x8766ed4>
  # My title is Ethics
  ```
  Ruby treats self as a sort of default object: When you call a method without an explicit object reference, Ruby assumes that you meant to call the method on self.
  **Don't write `self.title` when a plain `title` will do.**
3. > The Ruby technique for resolving methods is straight out of the object oriented handbook: Look for the method in the class of the object. If it is there, call it and you are done. If not. move on to the superclass and try there. Repeat until you either find the method of run out of superclasses.
4. **Virtually everything you come across in Ruby is an object. If you can reference it with a variable, it's an object**, including `3`, `true`, `false`, and `nil`.
5. `.eval`, defined by `Object`, takes a string and executes the string as if it were Ruby code.
6. `.public_methods` returns an array of all the method names available on the object.
7. `.instance_variable` returns the names of any instance variables buried in the object in an array.
8. private method can be specified by:
  ```ruby
  class Document

    private # methods after here are private
  end
  ```
  or
  ```ruby
  class Document
    private :word_count
  end
  ```
9. Private methods are callable from subclasses.
10. protected: any instance of a class can call a protected method on any other instance of the class.
11. All methods can be called through `.send`: `doc.send( :word_count)`
12. `the_object.class` returns the class of an object
