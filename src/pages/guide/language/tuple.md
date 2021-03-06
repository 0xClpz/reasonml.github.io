---
title: Tuple
order: 50
---

Tuples are

- immutable
- ordered
- fixed-sized at creation time
- heterogeneous (can contain different types of values)

```reason
let my3dCoordinates = (20.0, 30.5, 100.0);
let ageAndName = (24, "Lil' Reason");
```

Tuples' types can be used in type annotations as well. Tuple types visually resemble tuples values.

```reason
let my3dCoordinates: (float, float, float) = (20.0, 30.5, 100.0);
/* a tuple type alias */
type myPair = (int, string);
let ageAndName: myPair = (24, "Lil' Reason");
```

**Note**: there's no tuple of size 1. You'd just use the value itself.

### Usage

The standard library provides `fst` and `snd` ([here](/api/Pervasives.html), under "Pair operations"), convenience functions that allow you to access the first and second element of a 2-tuple. Generally, you'd access n-tuple members through destructuring (described later in the sidebar):

```reason
let (_, y, _) = my3dCoordinates; /* now you've retrieved y */
```

The `_` means you're ignoring the indicated members of the tuple.

### Tips & Tricks

You'd use tuples in handy situations that pass around multiple values without too much ceremony. For example, to return many values:

```reason
let getCenterCoordinates () => {
  let x = doSomeOperationsHere ();
  let y = doSomeMoreOperationsHere ();
  (x, y)
};
```

Or to "pattern-match" (covered later) on the conjunction of possibilities:

```reason
switch (isWindowOpen, isDoorOpen) { /* this is a 2-tuple */
| (true, true) => ...
| (true, false) => ...
| (false, true) => ...
| (false, false) => ...
}
```

Try to keep the usage of tuple **local**. For data structures that are long-living and passed around often, prefer a **record**, which has named fields.

### Design Decisions

Tuple's existence might seem odd for those coming from untyped languages. "Why not just use a list/array?"

A type system isn't all-powerful, nor should it be; some tasteful trade-offs need to be applied in order to keep the language simple, performant (both compilation and running speed) and easy to understand. Reason lists, for example, are more flexible in size; they can be concatenated, appended, sliced, etc. In return, they need to be homogenous (can only contain a single type of value per list), and random index access on them might not always be valid \*. Tuple, on the other hand, through its constrain on size, is faster, gives the type system the leeway to exhaustively track all its items' types, and guarantees safe access.

\* It's not that the Reason type system cannot accept heterogenous, dynamically-sized lists; it actually can (hint: GADT)! But making such feature the default increases both the first-time learning overhead and the understandability of code. Just because the types can accomplish it doesn't mean it's always a good idea to let some pieces of code grow unboundedly complex!
