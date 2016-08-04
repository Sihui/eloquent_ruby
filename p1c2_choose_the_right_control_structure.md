#### Chapter 2. Choose the Right Control Structure

1. `if` <> `unless`; `while` <> `until`
2. Use `each`, not `for` <sup>[1](#each_scope)</sup>
3. `case` uses `===`<sup>[2](#chp12_eql_sign)</sup> so you can use it to check the class of an object or to detect a regular expression:

  ```ruby
  case doc # check class
  when Document
    puts "It's a document."
  when String
    puts "It's a string."
  else
    puts "Don't know what it is."
  end

  case title # check regular expression
  when /War And .*/
    puts 'Maybe Tolstoy?'
  when /Romeo And >*/
    puts 'Maybe Shakespeare?'
  else
    puts 'No idea...'
  end  
  ```
  Everything in Ruby returns a value, if no claus matches and there is no `else`, `case` returns `nil`
4. Only `nil` and `false` in Ruby are treated as `false`, **everything else** is `true`, including `0` and the string `"false"`.
  - If you are looking for `nil`, then look for `nil` explicitly:
    ```ruby
    until (next_object = get_next_object).nil?
      # Do something with the Object
    end  
    ```
5. `defined?(obj)` returns returns a string that describes the thing passed in or `nil` if it's not defined.
6. `file_is_readable = readable ? 'the file is readable' : 'the file is not readable'`
7. `@first_name ||= ''`, not <s>`@first_name = '' unless @first_name`</s>
    - but do *NOT* use `||=` to assign boolean. ie: `is_blue ||= true` will overwrite `is_blue` when `is_blue` is initially `false`     

<a name="each_scope">1</a>: `each` block introduce a new scope (variables introduced in the each block only live there), while `for` block doesn't. Ruby actually defines `for` in terms of `each`, so why not just `each` directly.

<a name="chp12_eql_sign">2</a>: [more about Equality in Ruby]()
