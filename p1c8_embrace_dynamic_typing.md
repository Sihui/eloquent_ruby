#### Chapter 8. Embrace Dynamic Typing

1. > **Instead of looking at an object's type to decide whether it is the correct object, Ruby simply assumes that if an object has the right methods, then it is the right kind of object. Duck typing: if it walks and quacks like a duck, then it must be a duck.**

****
**Advantages of Dynamic Typing: **
1. > The real compactness payoff of dynamic typing comes not from leaving out a few int and string declarations; it comes instead from all the BaseDocument style abstract classes that you never write, from interfaces that you never create, from the casts and derived types that are simply irrelevant.

2. **Extreme Decoupling:**
> Ruby's dynamic typing means that you don't declare the classes of variables and parameters. That means that your classes are not frozen together in a rigid network of type relationships. In Ruby, any two classes that *can* work together *will* work together. **Flexibility is a huge advantage when it comes to constructing programs.**

3. It decreases complexity: the method is either there or it is not.
4. > With dynamic typing, it's the programmer who gets to pick the right level of documentation, not the rules of the languages. If you are writing simple, obvious code, you can be very minimalistic. Alternatively, if you are building something complex, you can be more elaborate. Dynamic typing allows you to document your code to exactly the level you think is best. It's your job to do the thinking.

****
1. > The best way to avoid mixing your types is to write the clearest, most concise code you can, which explains why Ruby programmers place such a high premium on clear and concise code. **If it's easy to see what's going on, you will make fewer mistakes.**

2. > The Ruby answer to both type-related bug and non-type-related bug is to write automated tests, **lots and lots of automated tests**.

3. > **Try to make your code speak to the human reader as much as it speaks to the Ruby interpreter.**

****
**How to take advantage of dynamic typing:**
1. Don't create more infrastructure than you really need. Keep in mind that Ruby classes don't need to be related by inheritance to share a common interface; they only need to support the same methods.
2. Don't obsure your code with pointless checks to see whether *this* really is *that*. Do take advantage of the terseness provided by dynamic typing to write code that simply gets the job done with as little fuss as possible.
3. Keep in mind that someone(possibly you!) will need to read and understand the code in the future.
4. Above all, write test.
****
