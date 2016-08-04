#### Chapter 1. Write Code that Looks Like Ruby

1. Core idea: Code should be **crystal clear** and **concise**. (**readability**)
2. Use two paces per indent.
3. Use comments to explain how to use your code, not how it works.
  > - That voice you hear in your head, the one whispering that you need to add some comments, may just be your program crying out to be rewritten.
  > - Good code is like a good joke: It needs no explanation.

4. Class name -> `CamelCase`, Method relative -> `snake_case`, Constant -> `All_UPPERCASE`
5. Parentheses are optional, omit them when:
    - calling a method that is familiar, one line, whose arguments are few in number
    - empty argument list: use `def words`, not <s>`def words()`</s>
    - in control statement: use `if word.size < 100`, not <s>`if (word.size < 100)`</s>
6. Code block:

  ```ruby
  # Use {} for one line block
  10.times { |n| puts "The number is #{n}" }
  # Use do/end otherwise
  10.times do |n|
    puts "The number is #{10}"
    puts "Twice the number is #{10 * 2}"
  end
  ```
