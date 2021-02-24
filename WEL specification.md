# Waypoint Editor Language
WEL main purpose is to restrain the randomness in the process of assigning properties to waypoints.

If *w* is the number of waypoints on the map, the WEL interpreter will generate an ordered sets of *w* elements for every property specified in the WEL description; the point being that each set will be generated according to constraints specified in the description.

Note: In general, spaces and line breaks do not make any instruction differ in meaning.

## Basic set notation

### Ordered set
The simplest syntax describing an **ordered** set of n elements x<sub>k</sub> is `{x_1, x_2, x_3, ..., x_n}`.

The notion of order does not mean that it exists any kind of mathematical order between them; in fact, it refers to the behavior of the WEL interpreter: it will always fetch the elements of such sets in the same order.

For instance, `{1,8,1,9,1,7,0}` and `{disabled,0,8,0,0}` are ordered sets and if they were to be used in a description their order would not change.

### Unordered set
The simplest syntax describing an **unordered** set of n elements x<sub>k</sub> is `[x_1, x_2, x_3, ..., x_n]`.

The main difference between ordered and unordered sets is that unordered sets will have their elements shuffled on purpose when interpreted, whilst ordered sets order will always be preserved. Unordered sets are thus the way to go to pick values randomly amongst a finite set.

For instance, writing `[1,8,1,9,1,7,0]`, `[1,8,1,9,1,7,0]`, `[1,9,1,8,1,7,0]` or `[0,1,1,1,8,7,9]` results in the same outcome.

### Expansion
In a context where a set of n elements is expected, a set of less than n elements will be **expanded** by cycling through its elements multiple times until the appropriate number of iteration is reached. If the source set is unordered, each cycle will be shuffled separately.

#### Examples
-  set `{arrows, nucleus}` would be interpreted as `{arrows, nucleus, arrows, nucleus, arrows, nucleus}` if the WEL interpreter needed to yield 6 items from it.
- Interpreted for 16 elements, the set `[1,2,3]` can yield `{1,3,2, 2,3,1, 2,1,3, 1,2,3, 1,3,2, 3}` (5 cycles and a random element: 5*3+1=16).
- On the contrary, sets can be trimmed. `[1,2,3]` can have be `{1}`, `{2}` or `{3}` if the interpreter only needs one element, and of course `{1,2,3}` would only be `{1}`

### Ranges
Long series of consecutive numbers do not have to be written by hand. A **range**, written `a-b` represents such a series with numbers ranging from `a` and `b`. `a` must be smaller than `b`.

For example, `{1-10}` is equivalent to `{1,2,3,4,5,6,7,8,9,10}`, and `{0,45-47,2,44-46}` is equivalent to `{0,45,46,47,2,44,45,46}`.

### Multiplication
Much like ranges, a syntax sugar exists to express a series of `n` times the same number `a`: `{a(*n)}`.
But multiplication is much more powerful than that! Multiplying a set by a number `n` will produce a set of `n` elements picked from the beginning of the set. See [*Expansion*](#expansion) above for more details.

Examples: `{5-8}(*2)` is `{5-8,5-8}` and `{1-10, disabled(*4)}` is `{1-10,disabled,disabled,disabled,disabled}`.

### Addition and Subtraction
It is possible to add or subtract elements from sets.

`[x_1, ..., x_n]+[y_1, ..., y_n]` is defined as `[x_1, ..., x_n, y_1, ..., y_n]`.

`[x_1, ..., x_n]-[x_k, ..., x_n]` is defined as `[x_1, ..., x_k]`.

For example, if the variable `cp` contains all the checkpoint numbers, `cp-{39}` will contain every checkpoint number except 39 — if there is any checkpoint 39 in the first place. If the variable `weapon amount` contained every checkpoint amount of ammo, `weapon amount-{4(*10)}+{0(*10)}` would choose 10 checkpoint with 4 ammos and would return a set that describe them as having no ammo instead.

### k but what now
— to be continued.
