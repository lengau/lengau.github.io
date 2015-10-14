---
layout: post
title:  "Why Long Scale Numbers Are Better"
date:   2015-10-14 12:26:50
categories: numbers
---

Note: This was originally a Jupyter notebook, which I converted. You can also
[download the original.](/Jupyter_Notebooks/Long_Scale_Numbers_Better.ipynb)


In the anglophone world, it's generally accepted that 10<sup>9</sup> is a billion. In my opinion, this is a bad idea. Specifically, it makes the naming of numbers more complicated.

Since pretty much everyone reading this is likely to use the short scale without even thinking about it, let me explain it to you by teaching you how to count using the Indian numbering system, which has a similar issue.

## The Indian Numbering System

I'm not here to teach you Hindi or Urdu, so we're just going to use the English names for the numbers. But I still need to introduce you to a few new names for numbers anyway I'm sure you're familiar with these numbers:

| Number | Name         |
|--------|--------------|
|      1 | one          |
|     10 | ten          |
|    100 | one hundred  |
|  1 000 | one thousand |
| 10 000 | ten thousand |

(Can we all just take a moment here to appreciate the use of the [thin space](https://en.wikipedia.org/wiki/Thin_space) for digit grouping? Both commas and periods for digit grouping are a bad idea, because they're also both decimal marks.)

So far, so normal. But this is where things start to change. You're probably familiar with the number 1,00,000. You likely call it 'one hundred thousand' and would write it 100 000 (or with either a period or a comma as a thousands separator). In the Indian numbering system, this number is 'one lakh'. After a thousand, the next named number is a lakh. One hundred lakh = one crore. Or in a table:

| Scientific Notation | Number                  | Name         |
|--------------------|------------------------|--------------|
| 10<sup>0</sup>              | 1                       | one          |
| 10<sup>1</sup>              | 10                      | ten          |
| 10<sup>2</sup>              | 100                     | hundred      |
| 10<sup>3</sup>              | 1 000                   | thousand |
| 10<sup>4</sup>              | 10 000                  | ten thousand |
| 10<sup>5</sup>              | 1 00 000                | lakh         |
| 10<sup>7</sup>              | 1 00 00 000             | crore        |
| 10<sup>9</sup>              | 1 00 00 00 000          | arab         |
| 10<sup>11</sup>           | 1 00 00 00 00 000       | kharab       |
| 10<sup>13</sup>           | 1 00 00 00 00 00 000    | neel         |
| 10<sup>15</sup>           | 1 00 00 00 00 00 00 000 | padm         |

I'm sure you find this a little bit strange. I did at first. But there's actually a formula to it once you get to the larger numbers. Given the following list:

1. Lakh
2. Crore
3. Arab
4. Kharab
5. Neel
6. Padm
7. Shankh

You can figure out which number on the list to use for a number with this formula (where $n$ is the order of magnitude):

$$
    M = \frac{n-3}{2}
$$

### What's wrong with it?

To most English speakers outside of South Asia, this looks pretty strange. The first separator is at a thousand, but each separator after that is a hundred away. So you get a rather weird meshing of the numbers. This is why you have to go down by three orders of magnitude before counting the zeroes. It's a fairly simple task, but most who aren't used to it would likely say "this is overcomplicated".

I agree.

What I don't agree with is what normally follows it: "our system of a thousand, million, billion, makes much more sense."

## Short Scale

Short scale is the number scale most common in the English speaking world. To copy the orders of magnitude table from above, its:

| Order of Magnitude | Number                        | Name        |
|--------------------|------------------------------|-------------|
| 10<sup>0</sup>              | 1                             | one         |
| 10<sup>1</sup>              | 10                            | ten         |
| 100<sup>2</sup>              | 100                           | hundred     |
| 10<sup>3</sup> = 1000<sup>1</sup>              | 1 000                         | thousand    |
| 10<sup>6</sup> = 1000<sup>2</sup>              | 1 000 000                     | million     |
| 10<sup>9</sup> = 1000<sup>3</sup>              | 1 000 000 000                 | billion     |
| 10<sup>12</sup> = 1000<sup>4</sup>           | 1 000 000 000 000             | trillion    |
| 10<sup>15</sup> = 1000<sup>5</sup>           | 1 000 000 000 000 000         | quadrillion |
| 10<sup>18</sup> = 1000<sup>6</sup>           | 1 000 000 000 000 000 000     | quintillion |
| 10<sup>21</sup> = 1000<sup>7</sup>           | 1 000 000 000 000 000 000 000 | sextillion  |

Now once one gets past a thousand, it's all based on powers of a thousand. So for million, billion, trillion, etc, one can write a formula for it:

$$
    M = \frac{n-3}{3} = \frac{n}{3} - 1
$$

Or on powers of a thousand:

$$
    M = t - 1
$$

If you're thinking "this looks an awful lot like the equation for the Indian numbering system," you'd be correct. The only difference is that there's a 3 in the denominator instead of a 2. It's only a small amount more consistent because it uses powers of a thousand everywhere rather than one power of a thousand and then powers of a hundred.

### Why the numbers are named that way

Look at the powers starting at a million. They're mostly just counting in Latin. It becomes much clearer after 10<sup>33</sup>, one decillion. (decillion, undecillion, duodecillion, tredecillion, quattordecillion)

So as long as you know how to count in Latin and add the suffix *-illion*, these orders of magnitude aren't that hard. Except for the weird way you have to count them.

## A partial solution to the weirdness: Long Scale

Continental Europe already has this figured out. Although admittedly their earlier use of the short scale is at least partially to blame for this craziness in English, but let's look at whay they do, and what Britain used to do, even well into the 20th century (North American English seems to have used the short scale from at least 1729. [Thanks, Harvard.](https://en.wikipedia.org/wiki/Isaac_Greenwood)).

This solution is to use the long scale:

| Scientific Notation   | Number                                            | Name        |
|----------------------:|--------------------------------------------------:|-------------|
| 10<sup>0</sup>                | 1                                                 | one         |
| 10<sup>1</sup>                | 10                                                | ten         |
| 10<sup>2</sup>                | 100                                               | hundred     |
| 10<sup>3</sup> = 1000<sup>1</sup>       | 1 000                                             | thousand    |
| 10<sup>6</sup> = 1000000<sup>1</sup>  | 1 000 000                                         | million     |
| 10<sup>12</sup> = 1000000<sup>2</sup> | 1 000 000 000 000                                 | billion     |
| 10<sup>18</sup> = 1000000<sup>3</sup> | 1 000 000 000 000 000 000                         | trillion    |
| 10<sup>21</sup> = 1000000<sup>4</sup> | 1 000 000 000 000 000 000 000 000                 | quadrillion |
| 10<sup>24</sup> = 1000000<sup>5</sup> | 1 000 000 000 000 000 000 000 000 000 000         | quintillion |
| 10<sup>27</sup> = 1000000<sup>6</sup> | 1 000 000 000 000 000 000 000 000 000 000 000 000 | sextillion  |

Look at how elegant that is. After a million, it's just the power of a million, translated into Latin.

There's also a way to write a thousand million in a shorter way: "milliard". In fact, that works for the entire scale. This is called the Peletier scale, after [Jacques Peletier du Mans](https://en.wikipedia.org/wiki/Jacques_Pelletier_du_Mans). He made his own mistakes in it, but those were fixed three hundred years ago.

Note how simple this scale is for powers of a million:

$$
    M = m = \frac{n}{6}
$$

Easy, right?

### Complications

There's only really one complication to the long scale: People are already using "a billion" and "a trillion" to mean the wrong thing. That's actually less of a problem than one might think, though. Very little text uses numbers larger than "a trillion" written out, so if we simply make exceptions for those two numbers, we don't have to worry about it any more. I propose the fairly simple terms "terrion" for a billion (10<sup>12</sup>), coming from the SI prefix "tera" for the same, and "exillion" for a trillion (10<sup>18</sup>).

| Scientific Notation   | Number                                            | Name        |
|----------------------:|--------------------------------------------------:|-------------|
| 10<sup>0</sup>                | 1                                                 | one         |
| 10<sup>1</sup>                | 10                                                | ten         |
| 10<sup>2</sup>                | 100                                               | hundred     |
| 10<sup>3</sup> = 1000<sup>1</sup>       | 1 000                                             | thousand    |
| 10<sup>6</sup> = 1000000<sup>1</sup>  | 1 000 000                                         | million     |
| 10<sup>12</sup> = 1000000<sup>2</sup> | 1 000 000 000 000                                 | terrion     |
| 10<sup>18</sup> = 1000000<sup>3</sup> | 1 000 000 000 000 000 000                         | exillion    |
| 10<sup>21</sup> = 1000000<sup>4</sup> | 1 000 000 000 000 000 000 000 000                 | quadrillion |
| 10<sup>24</sup> = 1000000<sup>5</sup> | 1 000 000 000 000 000 000 000 000 000 000         | quintillion |
| 10<sup>27</sup> = 1000000<sup>6</sup> | 1 000 000 000 000 000 000 000 000 000 000 000 000 | sextillion  |

### Real world usage

A few real-world analogies might help you out:

* The world's GDP is about 77*10<sup>12</sup> USD. That's seventy-seven terrion dollars.
* The EU's GDP is about 18\*10<sup>12</sup> USD (roguhly 16*10<sup>12</sup> euros). Sixteen terrion euros.
* The United States has a GDP of roughly 17*10<sup>12</sup> USD. Seventeen terrion dollars.
* NASA's budget for 2015 is eighteen milliard, ten million dollars.
* The ESA's budget? Four milliard, four hundred and thirty million euros.

### In Python

It might be easier to think about this algorithmically. Perhaps a piece of code like this:


{% highlight python %}
from math import log10

DIGITS = ['zero', 'one', 'two', 'three', 'four',
          'five', 'six', 'seven', 'eight', 'nine']
TEENS = ['ten', 'eleven', 'twelve', 'thirteen', 'fourteen',
         'fifteen', 'sixteen', 'seventeen', 'eighteen', 'nineteen']
TENS = ['', 'ten', 'twenty', 'thirty', 'forty', 'fifty',
        'sixty', 'seventy', 'eighty', 'ninety']
HUNDRED = 'hundred'
HUNDRED_SEPARATOR = ' and '
THOUSAND_SEPARATOR = ', '
MILLION_SEPARATOR = ', '
MILLIONS = ['million', 'terrion', 'exillion', 'quadrillion', 'quintillion',
            'sextillion', 'octillion', 'nonillion', 'decillion', 'undecillion']
THOUSANDS = [
    'thousand', 'milliard', 'billiard', 'trilliard', 'quadrilliard',
    'quintilliard', 'sextilliard', 'octilliard', 'nonilliard', 'decilliard',
    'undecilliard']


def big_partial(number: int, magnitude: int,
                magnitude_string: str, and_string: str):
    bigs = write_number(number // magnitude, explicit_zero=False)
    littles = write_number(number % magnitude, explicit_zero=False)
    strings = [bigs, ' ', magnitude_string]
    if littles:
        strings.extend([and_string, littles])
    return ''.join(strings).strip()


def write_number(number: int, explicit_zero = True, magnitude=0):
    """Writes a number in English.

    Arguments:
        number: an integer number to write.

    Returns:
        A string containing the number written out.
    """
    number_strings = []
    if number < 10:
        if number == 0 and not explicit_zero:
            return ''
        return DIGITS[number]
    if number < 100:
        if number < 20:
            return TEENS[number-10]
        units = write_number(number % 10, explicit_zero=False)
        tens = TENS[number // 10]
        if units:
            return '-'.join((tens, units))
        return tens
    if number < 1000:
        return big_partial(number, 100, HUNDRED, HUNDRED_SEPARATOR)
    if number < 10**6:
        return big_partial(
            number, 1000, THOUSANDS[magnitude], THOUSAND_SEPARATOR)
    bigs = write_number(
        number // 10**6, explicit_zero=False, magnitude=magnitude+1)
    littles = write_number(
        number % 10**6, explicit_zero=False, magnitude=magnitude+1)
    strings = [bigs]
    if littles:
        if bigs and (number // 1000000) % 1000 > 0:
            strings.append(MILLIONS[magnitude])
        if number % 10**6 >= 100:
            strings.append(MILLION_SEPARATOR)
        else:
            strings.append(HUNDRED_SEPARATOR.strip())
        strings.append(littles.strip())
    return ' '.join(strings)

# A few demos:
for i in [1, 2, 10, 12, 25, 99, 100, 571, 6423, 65536, 142342,
          1000000, 1000001, 1000000000, 10000000003]:
    print(write_number(i))

{% endhighlight %}

    one
    two
    ten
    twelve
    twenty-five
    ninety-nine
    one hundred
    five hundred and seventy-one
    six thousand, four hundred and twenty-three
    sixty-five thousand, five hundred and thirty-six
    one hundred and forty-two thousand, three hundred and forty-two
    one
    one million and one
    one milliard
    ten milliard and three


As you can see, it's not particularly difficult to write numbers using long scale. Why don't you try it?
