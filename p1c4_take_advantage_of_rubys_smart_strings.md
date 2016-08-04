#### Chapter 4. Take Advantage of Ruby's Smart Strings

1. `''` single quote does not do any string interpretation. Use it unless you need string interpretation, cause it performs better by skipping string interpretation.
2. `""` double quote can use `#{}`, `\t`, and `\n` ect.
3. `%q` arbitrarily quote lets you use both `'` and `""` in your string without having to escape them. `%q` followed by any string delimiter. `%q{ "It's such a nice day", she said.  }`, `%q[  ]`, `%q<  >`, `%q$  $`...
4. `%Q` is like `%q` with the power of `#{}`, `\t`, and `\n` ect.
5. All strings in Ruby can span lines. You can cancel out the newlines with `""` or `%Q`.
    ```ruby
    a_multiline_string = %Q{a multi-line string with \
    no newline}
    ```
6. Here doc:
    ```ruby
    heres_one = <<EOF
    This is the beginning of my here document.
    And this is the end.
    EOF
    ```
7. `.lstrip` will return a copy of a string with all of the leading whitespace clipped off
8. `.rstrip` will peel the white space off of the end of a string
9. `.strip` will take the white space off of both ends
10. `.chomp` will return a copy of the string with at most one newline character lopped from the end.
    `"hello\n\n\n".chomp` will return `"hello\n\n"`
11. `.chop` will knock off the last character from the string, no matter what it is.
    `"hello".chop` will return `"hell"`
12. `.upcase`, `.downcase`, `.swapcase`
13. `.sub` does at most one substitution, `.gsub` will replace as many substring as possible.
    ```ruby
    puts 'yes yes'.sub('yes', 'no') # no yes
    put 'yes yes'.gsub('yes', 'no') # no no
    ```
14. `.split`
    ```ruby
    # with no arguments, the default delimiter is space
    'It is nice outside'.split # returns ['It', 'is', 'nice', 'outside']

    'Monday:Tuesday:Wednesday'.split(':') # return ['Monday', 'Tuesday', 'Wednesday']
    ```
15. `sub!`, `gsub!`, `lstrip!`, ect, change the original string    
16. `.index`: `"It is nice outside".index('is')` returns 3
17. `.each_char`, `.each_byte`, `.each_line`
18. Use `//` to wrap around regular expression
    ```ruby
    def html_escape(s)
      s.to_s.gsub(/&/, "&amp;").gsub(/\"/, "&quot;").
        gsub(/s/, "&gt;").gsub(/</, "&lt;")
    end
    ```
19. Ruby strings are **mutable**
    ```ruby
    first_name = 'Karen'
    given_name = first_name
    first_name[0] = 'D' # This changes given_name as well
    ```
18. `first_name[-1]` returns `n`,
    `"abcde"[3..4]` returns `de`
