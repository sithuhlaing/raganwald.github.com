---
layout: default
tags: [allonge]
---

In this essay, we are going to look at recursive algorithms, and how sometimes, we can organize an algorithm so that it resembles the data structure it manipulates, and organize a data structure so that it resembles the algorithms that manipulate it.

When algorithms and the data structures they manipulate are *isomorphic*,[^isomorphic] the code we write is easier to understand for exactly the same reason that code like template strings and regular expressions are easy to understand: The code resembles the data it consumes or produces.

[^isomorphic]: In biology, two things are isomorphic if they resemble each other. In mathematics, two things are isomorphic if there is a structure-preserving map between them in both directions. In computer science, two things are isomorphic if the person explaining a concept wishes to seem educated.

In [From Higher-Order Functions to Libraries And Frameworks](http://raganwald.com/2016/12/15/what-higher-order-functions-can-teach-us-about-libraries-and-frameworks.html), we had a look at `multirec`, a *recursive combinator*. We'll use `multirec` for our algorithms, to emphasize that all of our algorithms have the same form as our data structure.

Here we go.

### rotating a square

Here is a square composed of elements, perhaps pixels or cells that are on or off. We could write them out like this:

```
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️⚫️⚪️⚪️⚪️
⚪️⚪️⚫️⚫️⚫️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
```

Consider the problem of *rotating* our square. There us a very elegant way to do this. First, we cut the squre into four smaller squares:

```
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚫️⚪️⚪️⚪️

⚪️⚪️⚫️⚫️ ⚫️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
```

Now, we rotate each of the four smaller squares 90 degrees clockwise:

```
⚪️⚪️⚪️⚪️ ⚫️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚫️⚪️⚪️ ⚪️⚪️⚪️⚪️

⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚫️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️ ⚪️⚪️⚪️⚪️
```

Finally, we move the squares as a whole, 90 degrees clockwise:

```
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️ ⚪️⚫️⚪️⚪️

⚪️⚪️⚪️⚫️ ⚫️⚪️⚪️⚪️ 
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️ ⚪️⚪️⚪️⚪️
```

Then reassemble:

```
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️⚪️⚫️⚪️⚪️
⚪️⚪️⚪️⚫️⚫️⚪️⚪️⚪️ 
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️⚪️⚪️⚪️⚪️
```

How do we roate each of the four smaller squares? Exactly the same way. For example,

```
⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️
⚪️⚪️⚪️⚫️
```

Becomes:

```
⚪️⚪️ ⚪️⚪️
⚪️⚪️ ⚪️⚪️

⚪️⚪️ ⚪️⚫️
⚪️⚪️ ⚪️⚫️
```

By rotating each smaller square, it becomes:

```
⚪️⚪️ ⚪️⚪️
⚪️⚪️ ⚪️⚪️

⚪️⚪️ ⚪️⚪️
⚪️⚪️ ⚫️⚫️
```

And we rotate all for squares to finish with:

```
⚪️⚪️ ⚪️⚪️
⚪️⚪️ ⚪️⚪️

⚪️⚪️ ⚪️⚪️
⚫️⚫️ ⚪️⚪️
```

Reassembled, it becomes this:

```
⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️
⚫️⚫️⚪️⚪️
```

How would we rotate the next size down?

```
⚪️⚪️
⚫️⚫️
```

Becomes:

```
⚪️ ⚪️

⚫️ ⚫️
```

Rotating an individual dot is a NOOP, so all we have to do is rotate the four dots around, just like we do above:

```
⚫️ ⚪️

⚫️ ⚪️
```

Reassembled, it becomes this:

```
⚫️⚪️
⚫️⚪️
```

Voila! Rotating a square consists of dividing it into four "quadrant" squares, rotating each one in place, then moving them one position clockwise.

### recursion, see recursion

The algorithm we are describing is a classic recursive divide-and-conquer, and it's exactly what `multirec` was designed to do. So we'll implement it together.

Let's begin with a naïve representation for squares, a two-demenional array. For example, we would represent the square:

```
⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚪️
⚪️⚪️⚪️⚫️
⚪️⚪️⚪️⚫️
```

With this array:

```javascript
[['⚪️', '⚪️', '⚪️', '⚪️'],
 ['⚪️', '⚪️', '⚪️', '⚪️'],
 ['⚪️', '⚪️', '⚪️', '⚫️'],
 ['⚪️', '⚪️', '⚪️', '⚫️']]
```

To use `multirec`, we need four pieces:

1. An `indivisible` predicate function. It should report whether an array is to small to be divided up. It's implicity itself: `(square) => square.length === 1`.
2. A `value` function that determines what to do with a value that is indivisible. For rotation, we simply return what we are given: `(cell) => cell`
3. A `divide` function that breaks a divisible problem into smaller pieces. Our function will break a sqare into four quadrants. We'll see how that works below.
4. A `combine` function that puts the result of rotating the smaller pieces back together. Our function will take four quadrant squares and put them back together into a big square.

As noted, `indivisible` and `value` are trivial:

```javascript
const indivisible = (square) => square.length === 1;
const value = (cell) => cell;
```

`divide` involves no more than breaking arrays into halves, and then those halves again:

```javascript
const firstHalf = (array) => array.slice(0, array.length / 2);
const secondHalf = (array) => array.slice(array.length / 2);

const divide = (square) => {
  const upperHalf = firstHalf(square);
  const lowerHalf = secondHalf(square);

  const upperLeft = upperHalf.map(firstHalf);
  const upperRight = upperHalf.map(secondHalf);
  const lowerRight = lowerHalf.map(secondHalf);
  const lowerLeft= lowerHalf.map(firstHalf);

  return [upperLeft, upperRight, lowerRight, lowerLeft];
};
```
`combine` makes use of a little help from some functions we saw in [an essay about generators][jsg]:

[jsg]: http://raganwald.com/2016/05/07/javascript-generators-for-people-who-dont-give-a-shit-about-getting-stuff-done.html "JavaScript Generators for People Who Don't Give a Shit About GettingStuffDone™"

```javascript
function split (iterable) {
  const iterator = iterable[Symbol.iterator]();
  const { done, value: first } = iterator.next();

  if (done) {
    return { rest: [] };
  } else {
    return { first, rest: iterator };
  }
};

function * join (first, rest) {
  yield first;
  yield * rest;
};

function * zipWith (fn, ...iterables) {
  const asSplits = iterables.map(split);

  if (asSplits.every((asSplit) => asSplit.hasOwnProperty('first'))) {
    const firsts = asSplits.map((asSplit) => asSplit.first);
    const rests = asSplits.map((asSplit) => asSplit.rest);

    yield * join(fn(...firsts), zipWith(fn, ...rests));
  }
}

const concat = (...arrays) => arrays.reduce((acc, a) => acc.concat(a));

const combine = ([upperLeft, upperRight, lowerRight, lowerLeft]) => {
  // rotate
  [upperLeft, upperRight, lowerRight, lowerLeft] =
    [lowerLeft, upperLeft, upperRight, lowerRight];

  // recombine
  const upperHalf = [...zipWith(concat, upperLeft, upperRight)];
  const lowerHalf = [...zipWith(concat, lowerLeft, lowerRight)];

  return concat(upperHalf, lowerHalf);
};
```

Armed with `indivisible`, `value`, `divide`, and `combine`, we can use `multirec` to write `rotate`:

```javascript
const rotate = multirec({ indivisible, value, divide, combine });

rotate(
   [['⚪️', '⚪️', '⚪️', '⚪️'],
    ['⚪️', '⚪️', '⚪️', '⚪️'],
    ['⚪️', '⚪️', '⚪️', '⚫️'],
    ['⚪️', '⚪️', '⚪️', '⚫️']]
 )
 //=>
   [['⚪️', '⚪️', '⚪️', '⚪️'],
    ['⚪️', '⚪️', '⚪️', '⚪️'],
    ['⚪️', '⚪️', '⚪️', '⚪️'],
    ['⚫️', '⚫️', '⚪️', '⚪️']]
```

Voila!

### accidental complexity

Rotating a square in this recursive manner is very elegant, but our code is encumbered with some accidental complexity. Here's a flashing strobe-and-neon hint of what it is:

```javascript
const firstHalf = (array) => array.slice(0, array.length / 2);
const secondHalf = (array) => array.slice(array.length / 2);

const divide = (square) => {
  const upperHalf = firstHalf(square);
  const lowerHalf = secondHalf(square);

  const upperLeft = upperHalf.map(firstHalf);
  const upperRight = upperHalf.map(secondHalf);
  const lowerRight = lowerHalf.map(secondHalf);
  const lowerLeft= lowerHalf.map(firstHalf);

  return [upperLeft, upperRight, lowerRight, lowerLeft];
};
```

`divide` is all about extracting quadrant squares from a bigger square, and while we've done our best to make it readable, it is rather busy. Likewise, here's the same thing in `combine`:

```javascript
const combine = ([upperLeft, upperRight, lowerRight, lowerLeft]) => {
  // rotate
  [upperLeft, upperRight, lowerRight, lowerLeft] =
    [lowerLeft, upperLeft, upperRight, lowerRight];

  // recombine
  const upperHalf = [...zipWith(concat, upperLeft, upperRight)];
  const lowerHalf = [...zipWith(concat, lowerLeft, lowerRight)];

  return concat(upperHalf, lowerHalf);
};
```

`combine` is a very busy function. The core thing we want to talk about is actually the rotation: Having divided things up into four quadrants, we want to rotate the quadrants. The zipping and concatenating is all about the implementation of quadrants as arrays.

We can argue that this is _necessary_ complexity, because squares are arrays, and that's just what we programmers do for a living, write code that manipulates basic data structures to do our bidding.

But what if our implementation wasn't an array of arrays? Maybe `divide` and `combine` could be simpler? Maybe that complexity would turn out to be unnecessary after all?

### isomorphic data structures

When we have what ought to be an elegant algorithm, but the interface between the algorithm and the data structure ends up being as complicated as the rest of teh algorithm put together, we can always ask ourselves, "What data structure would make this algorithm stupidly simple?"

The answer can often be found by imagining a data structure that looks like the algorithm's basic form. If we follow that heuristic, our data structure would be recursive, rather than 'flat.' Since we do all kinds of work sorting out which squares form the four quadrants of a bigger square, our data structure would escribe  square as being composed of four quadrant squares.

Such a data structure already exists, it's called a [QuadTree]. Squares are represented as four quadrants, each of which is a smaller square or a cell. The simplest implementation is an array: If the array has four elements, it's a square. If it has one element, it is an indivisible cell.

[QuadTree]: https://en.wikipedia.org/wiki/Quadtree

A square that looks like this:

```
⚪️⚫️⚪️⚪️
⚪️⚪️⚫️⚪️
⚫️⚫️⚫️⚪️
⚪️⚪️⚪️⚪️
```

Would be encoded like this:

```javascript
const quadTree = [
    [
      ['⚪️'], ['⚫️'], ['⚪️'], ['⚪️']
    ],
    [
      ['⚪️'], ['⚪️'], ['⚪️'], ['⚫️']
    ],
    [
      ['⚫️'], ['⚪️'], ['⚪️'], ['⚪️']
    ],
    [
      ['⚫️'], ['⚫️'], ['⚪️'], ['⚪️']
    ]
  ];
```

It's easier to see if we number the cells in hexadecimal:

```
01 45
32 76

CD 89
FE BA
```

Now to our algorithm. Rotating a quadtree is simpler than rotating an array of arrays. First, our test for indivisibility and the value of an indivisible cell remain the same, a happy accident:

```javascript
const indivisible = (square) => square.length === 1;
const value = (cell) => cell;
```

Our `divide` function is insanely simple: QuadTrees are already divided in the manner we require:

```javascript
const divideQuadTree = (quadTree) => quadTree;
```

And finally, our combine function does away with all the unecessary faffing about with zipping and concatenating. Here it is, along with our finished function:

```javascript
const combineQuadTree = ([upperLeft, upperRight, lowerRight, lowerLeft]) =>
  [lowerLeft, upperLeft, upperRight, lowerRight];

const rotateQuadTree = multirec({
  indivisible,
  value,
  divide: divideQuadTree,
  combine: combineQuadTree
});
```

Let's put it to the test:

```javascript
rotateQuadTree(quadTree)
  //=>
    [
      [
        ['⚪️'], ['⚫️'], ['⚫️'], ['⚪️']
      ],
      [
        ['⚪️'], ['⚪️'], ['⚫️'], ['⚪️']
      ],
      [
        ['⚫️'], ['⚪️'], ['⚪️'], ['⚪️']
      ],
      [
        ['⚪️'], ['⚫️'], ['⚪️'], ['⚪️']
      ]
    ]
```

If we reasemble the square by hand, we see we have gotten:

```
⚪️⚫️⚪️⚪️
⚪️⚫️⚪️⚫️
⚪️⚫️⚫️⚪️
⚪️⚪️⚪️⚪️
```

### separation of concerns

Of course, all we've done so far is moved the "faffing about" out of our code and we're doing it by hand. That's bad,we don't want to retrain our eyes to read QuadTrees instead of flat arrays, and we don't want to sit at a computer all day manually translating QuadTrees to flat arrays and back.

If only we could write some code to do it for us... Some recursive code...



### notes

[anamorphism]: https://en.wikipedia.org/wiki/Anamorphism
[catamorphism]: https://en.wikipedia.org/wiki/Catamorphism
[cc-by-2.0]: https://creativecommons.org/licenses/by/2.0/
[reddit]: https://www.reddit.com/r/javascript/comments/5jdjo6/from_higherorder_functions_to_libraries_and/
[Ember]: http://emberjs.com/