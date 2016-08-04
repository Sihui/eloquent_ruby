#### Chapter 5. Find the Right String with Regular Expressions

1. `.` matches any single character, *except the newline character* (can append `m` to the end of the regular expression to turn it off, see 13.).
2. Set matches any one of a bunch of characters. ie `[aeiou]` matches any *single* lowercase vowel.
3. Range is defined by specifying the beginning and end of a sequence of characters, separated by a dash.
  - Examples:
    - `[0-9]` match exactly any *single* decimal digit.
    - `[0-9a-f]` match a *single* hexadecimal digit.
    - `[0-9a-zA-Z_]` match any *single* letter, number, or the underscore character.
  - `\d` matches any digit, so `\d\d` matches any two digit number from 00 to 99
  - `\w` where `w` stands for "word character" will match any letter, number or the underscore
  - `\s` match any white space character including the vanilla space, the tab, and the newline.

4. `|` means `or`. Examples:
  - `A|B` will match either A or B
  - `A\.M\.|AM|P\.M\.|PM` will match either A.M., AM, P.M., or PM
  - `The (cat|boat) is red`
  - `\d\d:\d\d (AM|PM)`: any string that starts with two digits, followed by a colon, followed by two more digits, followed by a space, followed by either AM or PM.
5. `*` matches zero or more of the thing that came just before it.
  - `AB*` matches `A`, `AB`, and `ABBBBB`
  - `R*uby` matches `uby`, `Ruby`, `RRRRuby`
  - `[0-9]*` will match any number of digits
6. `.*` will match anything
  - `George.*` will match the full name of anyone whose first name is George
  - `.*George` will match the name of anyone whose last name is George
  - `.*George.*` will match the name of anyone who has George in his name somewhere
7. In Ruby, wrap regular expression with `//`
    - use `=~` to test whether a regular expression matches a string. Ruby will scan along the string, searching for a match *anywhere* in the string. Will return the index where the match starts, or `nil` if no match found.
    - `puts /PM/ ~= '10:24 PM'` returns 6.
    - `=~` is ambidextrous: `puts '10:24 PM' ~= /PM/` works the same as above
    - append `i` to the end of the regular expression to turn case sensitive off
      - `puts "It matches! if /AM/i =~ 'am'"`
8. Can be use in search, example: blot out all of the time content:
    ```ruby
    def obscure_times!
      @content.gsub!(/\d\d:\d\d (AM|PM)/, '**:** **')
    end
    ```
9. `\A` matches strings *begin* with the regular expression: ie.`/\AOnce upon a time/`
10. `\z` matches strings *end* with the regular expression: ie. `/and they all lived happily ever after\z/`
11. `^` circumflex matches the beginning of a string of the beginning of any **line** within the string: `/^Once upon a time/`
12. `$` matches the end of the string or the end of any line within the string: `/happily ever after\./`
13. `m` makes `.` matches newline character as well: `/^Once upon a time.*happily ever after\.$/m`
14. `?` matches zero or *one* of the things that came before. `1:?` matches `1` and `1:`
