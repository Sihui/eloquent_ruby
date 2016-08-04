1. Two uses of string:
  1. to store data: normal variable names: `is_dark = true`
  2. to stand for something, like flags but with names: `Book.find(:all)`  in here `:all` stands for getting all the records. **Symbols are to be used for standing for something.**
2. There can only be one instance of any given symbol:
  ```ruby
  # a, b, and c all refer to exactly the same object
  a = :all
  b = a
  c = :all
  # all of the following are true
  a == c
  a === c
  a.eql?(c)
  a.equal?(c)
  ```
3. Symbols are immutable.
4. `.to_s` can turn symbols to strings: `the_string = :all.to_s`
5. `.to_sym` can turn strings to symbols: `the_symbol = 'all'.to_sym`
6. **Use symbols when you simply want an intelligible thing that stands for something in your code.**
