# Loops and Comprehensions

A **loop** is a control flow construct that repeats the code inside its body.
However, on top of this fairly standard behavior, Chai's loops also function as
**generators**, constructs used to generate sequences of values.  

## While Loops

A **while loop** repeats the content of its body until the condition in its
header is no longer true.  They begin with the `while` keyword and have
identical body syntax to that of if and match expressions.

    while cond do
        ...

    while cond -> ()

A common pattern with while loops is create, repeat, mutate pattern which looks
like the following:

    # create
    let i = 1
    while i < 11 do
        # repeat
        println(i)

        # mutate
        i++

The code above counts up from `1` to `10` printing out the numbers as it goes.
You might notice that the code is a little bit verbose: it is spread out over
several lines, and `i` is not needed outside of the loop yet it is declared
above it.  We can fix this by combining multiple statements into the while
loop's header.

    while let i = 1; i < 11; i++ do
        println(i)

The above code is equivalent to the previous listing but way shorter. We can
also elide different pieces of the header if we only want two parts of the
pattern.

    def count_up(start, end: int) = do
        while start <= end; start++ do
            println(start)

Another common pattern in programming is the [do-while pattern](https://en.wikipedia.org/wiki/Do_while_loop).
While Chai doesn't support this pattern directly, we can use our three part
while loop to pretty easily accomplish a similar task.

    while let ok = true; ok; ok = condition do
        ...

> This is actually a pattern that Chai will recognize and optimize into a more
> efficient do-while construct during compilation.

## For Loops

A **for loop** iterates over the elements of a sequence, executing its body for
each element.  A common example of a sequence would be the lists that we studied
in the previous chapter.

For loops begin with the `for` keyword followed by an iterator pattern, the `in`
keyword, and the sequence to iterate over.  For example, if we wanted to iterate
over the elements of a list, we could do so easily using for loops.

    for item in list do
        println(item)

The `item` variable stores the value of each element as the loop iterates.  

Another common sequence is a **range**.  Ranges are sequences of numbers
spanning from a start value up until an end value.  The start and end values are
separated by a `..`.  We can use ranges to express the idea of counting upward
much more simply then we did using while loops.

    for n in 1..11 do
        println(n)

The code above prints out the numbers `1` through `10`.

> This method is generally preferable to the while loop method as its is much
> more concise and clear.

Many types have methods that give us sequences.  For example, if we wanted to
iterate through the indices of a list as opposed to its elements, we could use
the `indices` method.

    for i in list.indices() do
        ...

Dictionaries are also sequences.  By default, when we iterate over them, we
iterate over their keys.

    for key in dict do
        ...

However, we can also iterate over their values using the `values` method.

    for value in dict.values() do
        ...

So far, we have only used single-value iterator patterns: names.  However, for
loops support pattern matching meaning we can actually iterate over multiple
values at the same time.  

Consider the `pairs` method of dictionaries, this returns a sequence of tuples
containing the key value pairs of the dictionary.  Using pattern matching, we
can quickly iterate over these pairs, extracting the variables as we go.

    for (key, value) in dict.pairs() do
        println(key, "->", value)

We can also perform a similar operation on lists using the `enumerate` method to
iterate through both the indices and elements at the same time.

    for (ndx, item) in list.enumerate() do
        println(ndx, "=", item)

## Sequences

Since all values in Chai are expressions, loops have to yield a value: a
**sequence**.  A sequence is a way of representing a "stream" of items of the
same type.

Sequences have the type label `Seq[T]` where `T` is the element type.  The most
general property of all sequences is that they are **iterable** which
essentially means you can use a for loop on them.

Several builtin types that you are already familiar with are sequences: strings,
lists, ranges, and dictionaries. 

> Strings are sequences of runes: when you iterate over a string, the elements
> will be runes.

The `Seq[T]` type is special in that it is the first *type class* we have dealt
with.  In essence, type clases are a way of categorizing types.  So `List[T]`,
`string`, and `Dict[K, V]` are all *subtypes* of (meaning they are "members" of
and coercible to) the `Seq[T]` type class.  This relationship can be thought of
as akin to how humans categorize information: for example, a dog and a cat are
both animals which programmatically could be expressed as the types of "Cat" and
"Dog" are both subtypes of the type class "Animal".

Type classes are a major topic in Chai that we will explore in depth in later
chapters; however, they are so fundamental to Chai and such a large idea that it
is often helpful and, in this case, necessary to introduce them a little bit
earlier. 

Loops act as **generators** for sequences of values.  Each iteration of the for
loop produces a new value.  Using this, you can easily rephase common operations
as sequences and generators.  For example, if you wanted a reusable stream of
fibonacci numbers, you could simply write a loop generator to give them to you.

    def fib() Seq[int] = do
        let a = 0, b = 1
        while true do
            a, b = b, a + b
            a  # the yielded value

Notice that the return type of the function is `Seq[int]` and the return value
is the while-loop itself.  Now, we can call `fib` to get a reusable sequence
of fibonacci numbers that we can iterate over.

    def print_n_fibs(n: int) = do
        let i = 0
        for a in fib() do
            println(a)
            i++

            if i == n do
                return

Sequences in Chai are lazy: they only generate values as they are needed. This
is incredibly important as our `fib` function ends with an infinite loop: if
Chai fully evaluated it, our program would never exit. 

Sequences can be a bit challenging to get your head around so take a moment to
fully process what just happened.  We used a loop as a generator for a new
sequence of values, returned that sequence, and consumed it via a for loop.
This style of writing generators is somewhat unique to Chai and can be very
helpful for building up complex layers of iterative logic very quickly cleanly.
Try playing around with sequences and generators a bit until you feel
comfortable with them.

## Break and Continue

The **break statement** allows you to exit a loop prematurely.  They are very
useful for checking for exit conditions in the middle of a loop. For example, if
you wanted  to search a collection of elements for a specific value, you could
use a break statement to exit early if you find the element.
    
    let found_item = false
    for item in list do
        if item == 2 do
            found_item = true

            # exit here since we don't need to keep searching
            break

    if found_item do
        println("found `2` in `list`")

TODO

## Comprehensions