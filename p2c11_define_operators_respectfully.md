#### Chapter 11. Define Operators Respectfully
1. Ruby translates every expression involving programmer-definable operators into an equivalent expression where the operators are replaced with method calls:
  - When you say `sum = first + second`, you are really saying `sum = first.+(second)`
2.  ```ruby
    names = []
    names << 'Rob'
    names << 'Denise'
    ```
3. You can overload `!`, but not: `not`, `or`, `||`, or `&&`.
4. Examples:
    ```ruby
    class Document                       

      def !
        Document.new( title, author, "It is not true: #{content}")
      end

      def +(other)
        Document.new( title, author, "#{content} #{other.content}" )
      end

      def +@  # unary ie +document
        Document.new( title, author, "I am sure that #{@content}" )
      end

      def -@ # unary
        Document.new( title, author, "I doubt that #{@content}" )
      end

      def [](index) # you may also want to add a size method
        words[index]
      end
    end                                      
  ```
5. **Trouble with Operating Across Classes**
  ```ruby
  def !
    Document.new( title, author, "It is not true: #{content}")
  end
  ```
  breaks when:
  ```ruby
  doc = new Document.new('hi', 'russ', 'hello')
  new_doc = doc + 'out there!'
  ```
  you might come up with:
  ```ruby
  def +(other)                             
    if other.kind_of?(String)
      return Document.new( title, author, "#{content} #{other}")
    end
    Document.new( title, author, "#{content} #{other.content}" )
  end
  ```
  but it still breaks when:
  ```ruby
  'I say to you, ' + doc # #<TypeError: can't convert Document into String>
  ```
  > If you want to define binary operators that work across classes, you need to either make sure that both classes understand their responsibilities or accept that your expressions will be sensitive to the order in which you write them.

6. **Staying out of trouble**
  1. When (not) to overload operators:
    1. When you are building a class that has a natural, well-understood meaning for the operators, then count yourself luck and start coding. (Like `Matrix` and `Vector`)
    2. When, although your class doesn't come with a whole set of universally understood operators, does have a few operators targets of opportunity:
      - if you are writing some kind of collection class, you may add `<<` operator.
      - if your class has some natural indexing tendencies, you may add `[]`, `[]=`, and `size`.
    3. When, even though there are no widely accepted operators in the domain you are modeling, you realize many of the methods on your class behave in a way that parallels the ordinary arithmetic operators.
      - ie `department = employee_1 + employee_2` and `division = department_1 + department_2`

      > **Assigning arbitrary, farfetched meanings to the common operators is one thing that gives programmer-defined operators a bad name.** Remember, the goal is clear and concise code. Code that requires me to recall that I get a department when I add two employees together, but that a department minus an employee is still a (smaller) department, is going to be anything but clear. **If you find your operators are starting to take on a life of their own, then perhaps you have gone a litter too far. In any event, it is always a good idea to provide ordinary method names as alias for your fancy operators as a sort of escape valve, just in case other programmers fails to appreciate the elegance of adding two departments together.**

  2. **Respect the generally accepted meaning of common operators:**
    1. Symmetric: `a + b == b + a`
    2. Commutative: `a + b + c`
    3. `c = a + b` shouldn't change the value of `b`
7. ```ruby
   day = 7
   month = 2
   year = 2016
   file_name = 'file_%02d%02d%d' % [ day, month, year ] # file_07022016 
   ```
