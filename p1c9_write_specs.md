#### Chapter 9. Write Specs!

1. > If you want to know that your code works, you need to test it. You need to test it early, you need to test it often, and you certainly need to test it whenever you change it. It turns out that with programs, seeing is believing.

2. Test should focus on the behavior itself not on the test: it's like a description about what should happen. RSpec does this better than Test::Unit.
3. By convention, RSpec goes in a file called `<class name>_spec.rb` and you can run it by `spec <class name>_spec.rb` or `spec .`
4. A tidy spec is a readable spec, use: `before :each`, `before :all`, `after :each`, `after :all`
5. An ideal test exercises exactly one class at a time.
6. A **stub** is an object that implements the same interface as one of the supporting cast members, but returns canned answers when its methods are called
    - ex: `stub_printer = stub :available? => true, :render => nil`
7. A **mock** is a stub with an attitude. Along with knowing what canned responses to return, a mock also knows which methods should be called and with what arguments.  While stub is there purely to get the test to work, a mock is an active participant in the test, watching how it is treated and failing the test if it doesn't like what it sees.
    ```ruby
    it `should know how to print itself` do
      mock_printer = mock('Printer')
      mock_printer.should_receive(:available?).and_return(true)
      mock_printer.should_receive(:render).exactly(3).times
      @doc.print( mock_printer ).should == 'Done'
    end
    ```
8. If you use Test::Unit, check out shoulda.
9. RubySpec is a great place for RSpec examples.
10. Unit tests should
    - run quick with the setup that every developer has.
    - be *silent*: if the test fails, it should tells you clearly that it failed, until then, quiet please.
    - be independent of each other
11. **Make sure your test will actually fail.**
12. Write really, really good test if you can. Write OK ones if that is all you can do. If you can't do anything else, at least write some tests that exercise your code, even just a little bit.
13. **Never, ever, add a feature to your code without first adding a test.**
    > Start out by writing a test that fails and then write the code that makes the test pass. Repeat until you have the Great American Program, complete with a full suite of tests. If you never write any code without first having a test for whatever that code is supposed to do, you will automatically write testable programs.
    > Write the tests first, or second, or third. But write the darned tests.
14. > **You will never know whether your code works unless you write the tests. Write throughly comprehensible tests if you can, write sketchy tests if you must, but write the tests.**
