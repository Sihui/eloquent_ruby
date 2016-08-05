#### Chapter 12. Create Classes That Understand Equality
1. `equal?` is for object identity: the only way `x.equal?(y)` returns true is when they reference to the same object.
  - There is no need to you to rewrite `equal?` ever.
2. `==` have the same default behavior as `equal?`, yet you are free to redefine what it means to be equal or identical for your objects.
  Ex:
  ```ruby
  class DocumentIdentifier
    def ==(other)
      return true if other.equal?(self)
      return false unless other.respond_to?(:folder)
      return false unless other.respond_to?(:name)
      folder == other.folder && name == other.name
    end
  end
  ```
  Use `return true if other.equal?(self)` to speed up the comparison.
  Use `other.respond_to?(:xx)` to see if the other object has the method `xx`, instead of checking if the other method is the same type `return false unless other.instance_of?(self.class)`. Duck testing: if it looks like a duck and quacks like a duck, then it probably is a duck
3. **Symmetry assumption**: we tend to assume `a == b` implies `b == a`. But in the above case, when `documentIdentifier == something` does not imply `something == documentIdentifier` since we might not have override the `==` operator in `something`'s class.
  - You can either rewrite `something`'s class or just leave it but document about it.

4. **Transitive property**: we tend to assume `a == b` and `b == c` implies `a == c`.
  Ex:
  ```ruby
  class VersionedIdentifier < DocumentIdentifier  ##(vi
    attr_reader :version

    def initialize( folder, name, version )
      super( folder, name )
      @version = version
    end

     def ==(other)
      if other.instance_of? VersionedIdentifier
        other.folder == folder &&
        other.name == name &&
        other.version == version
      elsif other.instance_of? DocumentIdentifier
        other.folder == folder && other.name == name
      else
        false
      end
    end
  end  

  v1 = VersionedIdentifier.new( 'specs', 'bfg9k', "V1" )
  v2 = VersionedIdentifier.new( 'specs', 'bfg9k', "V2" )
  unversioned = DocumentIdentifier .new('specs', 'bfg9k')

  v1 == unversioned # true
  v2 == unversioned # true
  v1 == v2 # false
  ```
  Fixes:
    1. You can convert `VersionedIdentifier` to `DocumentIdentifier` and **know** that you are comparing them as `DocumentIdentifier`
    2. OR you can just add your own specialized comparison method to `VersionedIdentifier`.
    ```ruby
    class VersionedIdentifier
      def as_document_identifier                   ##(as_doc_id
        DocumentIdentifier.new( folder, name )
      end                                          ##as_doc_id)

      def is_same_document?(other)                 ##(same_doc
        other.folder == folder && other.name == name
      end                                          ##same_doc)
    end
    ```

5. **Case statement uses `===` **
  > By default, `===` calls `==`, so unless you specifically override `===`, wherever you send `==`, `===` is sure to follow. Leave `===` alone unless doing so results in really ugly case statement.

  *The Regexp class has a === method that does pattern matching when confronted with a string.* Since `case` use `===`, we can compare regular expressions with strings in `case`:
  ```ruby
  case location
    when /area.*/
      # ...
    when /roswell.*/
      # ...
    else
      # ...
  end                        
  ```

6. **The `Hash` class uses the `eql?` method to decide if two keys are in face the same key.**.
  - Hash codes need to be: 1. stable over time 2. consistent with the value of the key: if two keys are equal-if they should return the same value out of the hash table-then, when asked, they must return the same hash code.
7. The default implementations of `Hash` and `eql?` are based on object identity.
    > As long as you follow the hash Prime Directive-that **if `a.eql?(b)` then `a.hash == b.hash`** -you are free to override these two methods.

    ```ruby
    class DocumentIdentifier              
      def hash
        folder.hash ^ name.hash
      end

      def eql?(other)
        return false unless other.instance_of?(self.class)
        folder == other.folder && name == other.name
      end
    end                                   
    ```

    1. Using exclusive or `^` is a simple way of creating a very thorough mishmash of the two numbers.
    2. The `eql?` method takes a very restrictive view of equality: only instances of DocumentIdentifier can be equal: getting your objects to work as hash table keys is no time to be imaginative about cross-class equality.
8. **Staying out of troubles**:
  > 1. don't override them unless you really have to.
  2. if you are defining a class that is mostly a wrapper for some other object, consider borrowing the equality methods from that other object.
  3. if you do need to write your own equality methods, do the simplest thing that will work. If you don't have to support equality across different classes, then don't. A limited but working implementation is better than an elaborate and subtly broken one.

9. `<=>` return -1, 0, or 1.
  - used for ordering
  - **`<=>` should be consistent with `==`, that is if `a <=> b` returns 0, `a == b` should be true.**
  - `<`, `>=`, `<`, and `<=` rely on `<=>`
10. Ruby classes treat the `===` as `kind_of?`, so you can pick your class using `case`:
    ```ruby
    case the_object
      when String
       puts "it's a string"
      when Float
       puts "It's a float"
      when Fixnum
       puts "It's a fixnum"
      else
       puts "Dunno!" 
    end                              
    ```
