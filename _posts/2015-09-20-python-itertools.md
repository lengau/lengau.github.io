---
layout: post
title:  "Python: Itertools and why you should use it."
date:   2015-09-28 12:02:50
categories: python
---

I'm sure most Python programmers are at least vaguely familiar with
[itertools]. Most likely in the form of `itertools.count` and
`itertools.product`, which are probably sitting somewhere in your code.

What you may not be as familiar with (at least, I wasn't until I bothered to
read the full docs today) are some of the other convenient iterators and
generators. I'll cover just a few here.

## Review: count and product

`count` and `[product]` are the two functions from `itertools` that I see used
the most, although I do still see (and am guilty of writing) code that uses
more lines to do the same things. A comparison of `count` and `product` to the
naïve code that forms the same logic should suffice.

### [count]

A fairly common pattern seen all over the case, implemented in dozens of
programming languages, is something like this:

    i = start_value
    while True:
        do_something_with(i)
        if we_need_to_exit():
            break
        i += step_size

This is incredibly common when you're unsure what the last value you're going
to need to operate on is. Unfortunately, it's somewhat uglier in Python than
the same pattern in C. (It's also fairly ugly in Pascal since you also have to
use a while loop, and it's also pretty in other C-like languages.)

In C, we can write it as:

    for (i = start_value; we_need_to_exit(); i += step_size){
        do_something_with(i);
    }

This is possible because the loop condition in C is explicitly stated, and can
be any expression. In truth, Python doesn't actually have a real for loop. The
for loop in Python is really a [foreach loop][wiki-foreach].

`itertools.count`, however can help us improve our Python code. It's still
not quite as concise as the C version, but it's getting there:

    from itertools import count

    for i in count(start_value, step_size):
        do_something_with(i)
        if we_need_to_exit():
            break

This version has a few advantages:

* It makes the fact that you're simply counting more obvious
* `i` falls out of scope when you exit the loop
* It's shorter, which makes a bug less likely to hide in it.

### [product]

`product` is less obviously advantageous than `count`, unless you (or your
code reviewer) shares Linus Torvalds's belief that more than 3 levels of
indentation is a sign of bad code.

In practical terms, `product` is essentially a certain class of nested for loop.
What was:

    for i in range(10):
        for j in range(10):
            do_something_with(i, j)

becomes:

    from itertools import product

    for i, j in product(range(10), repeat=2):
        do_something_with(i, j)

or if your i and j aren't iterating over the same thing:

    for i in list_of_i_values:
        for j in list_of_j_values:
            do_something_with(i, j)

can become:

    from itertools import product

    for i, j in product(list_of_i_values, list_of_j_values):
        do_something_with(i, j)

## Other nested for loops

That's all well and good, but there are plenty of other uses of nested for
loops that aren't doable with `product`. So what are they?

### [permutations] and [combinations]

These are the permutations you're probably expecting if you remember anything
about statistics at all, you probably know what to expect.

In practical terms, `permutations` is equivalent to `product` with all the
duplicates removed. A very general version might be:

    from itertools import product

    for working_values in product(values, repeat=n):
        if len(set(working_values)) < len(working_values):
            continue
        do_something_with(*working_values)

Any time you're deduplicating `product` in a similar way, you may as well skip
it and use `permutations` instead.

`combinations` is the same as the combinations you'll see in statistics, too.
A hand of cards in bridge is a 13 card long combination of a deck, for example.
But lots of algorithms with running time `~n^2` can use combinations, too:

    for i in range(start, stop):
        for j in range(i + 1, stop):
            do_something_with(i, j)

it's fairly easy to understand, and if you're working with pairs of points in a
space, it looks alright to do:

    for i, a in enumerate(points):
        for b in points[i+1:]:
            do_something_with(a, b)

`combinations` looks even better, though:

    from itertools import combinations

    for my_points in combinations(points, 2):
        do_something_with(*my_points)

Isn't that so much prettier? Especially since you now can (but needn't) have an
iterable containing the combination, instead of 2, 3, 5, or 13 different
variables.

### [combinations_with_replacement]

As a quick side note, `combinations_with_replacement` may be one of the
least-known functions in `itertools`. The biggest reason for this is its
relative newness. It was only added in versions 2.7 and 3.1, so if you need to
target versions of Python that are more than five years old, you can't use it.

The nested for loop of:

    for i, a in enumerate(points):
        for b in points[i:]:
            do_something_with(a, b)

can become:

    from itertools import combinations_with_replacement as combine

    for a, b in combine(points, 2):
        do_something_with(a, b)

If ordering doesn't matter, this particular nested loop can become:

    from itertools import combinations

    for a, b in combinations(points, 2):
        do_something_with(a, b)

    for a in points:
        do_something_with(a, a)

Although at that point, I'd probably just use the nested for loop.

## Final thoughts

[itertools] is a very powerful library in Python for looping, and can be used
in a huge variety of ways. With `product`, it can even be used to make nested
loops of unknown depth in just a couple of lines.

Know it. Use it. Love it.

Or at least know it exists for now. Once you know about it, you'll probably
find great ways to use it later.

[combinations]: https://docs.python.org/3/library/itertools.html#itertools.combinations
[combinations_with_replacement]: https://docs.python.org/3/library/itertools.html#itertools.combinations_with_replacement
[count]: https://docs.python.org/3/library/itertools.html#itertools.count
[itertools]: https://docs.python.org/3/library/itertools.html
[permutations]: https://docs.python.org/3/library/itertools.html#itertools.permutations
[product]: https://docs.python.org/3/library/itertools.html#itertools.product
[recipes]: https://docs.python.org/3/library/itertools.html#itertools-recipes
[wiki-foreach]: https://en.wikipedia.org/wiki/Foreach_loop
