#### Chapter 10. Construct Your Classes from Short, Focused Method

1. Short, single-purpose method
    - plays to the strength of the Ruby language
    - makes the app more testable
2. **Composed Method Technique** advocates dividing your class up into methods that have three characteristics:
  1. each method should do a single thing - focus on solving a single aspect of the problem;
  2. each method needs to operate at a single conceptual level: don't mix high-level logic with the nitty-gritty details;
  3. each method needs to have a name that reflects its purpose.

3. > The reason you should learn toward smaller methods is that all those compact, easy-to-comprehend methods will help *you* get the details right.

4. Ruby strongly encourage short method by making it very easy to write a method. (`def end`)

5. > Take the old bit of coding advice that every method should have exactly one way out, so that all of the logic converges at the bottom for a single return.

  ```ruby
  def prose_rating
    return :really_pretentious if really_pretentious?
    return :somewhat_pretentious if somewhat_pretentious?
    return :really_informal if really_informal?
    return :somewhat_informal if  somewhat_informal?
    return :about_right
  end

  def really_pretentious?
    pretentious_density > 0.3 && informal_density < 0.2
  end

  def somewhat_pretentious?
    pretentious_density > 0.3 && informal_density >= 0.2
  end

  def really_informal?
    pretentious_density < 0.1 && informal_density > 0.3
  end

  def somewhat_informal?
    pretentious_density < 0.1 &&  informal_density <= 0.3
  end

  def pretentious_density                     
    # Somehow compute density of pretentious words
  end

  def informal_density
    # Somehow compute density of informal words
  end                                                  
  ```
  > Once you can take in the whole method in single glance, then the motivation for the single-return rule goes away.

6. > Your method should be compact but it should *do something*.

  Bad Example:

  ```ruby
  class TextCompressor                    
    def add_unique_word( word )
      add_word_to_unique_array( word )
      last_index_of_unique_array
    end

    def add_word_to_unique_array( word )
      @unique << word
    end

    def last_index_of_unique_array
      unique.size - 1
    end
  end
  ```
