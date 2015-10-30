---
layout: api-command
language: Java
permalink: api/java/map/
command: map
related_commands:
    concatMap: concat_map/
    reduce: reduce/
    do: do/
---

# Command syntax #

{% apibody %}
sequence1.map([sequence2, ...], function) &rarr; stream
array1.map([array2, ...], function) &rarr; array
r.map(sequence1[, sequence2, ...], function) &rarr; stream
r.map(array1[, array2, ...], function) &rarr; array
{% endapibody %}

# Description #

Transform each element of one or more sequences by applying a mapping function to them. If `map` is run with two or more sequences, it will iterate for as many items as there are in the shortest sequence.

Note that `map` can only be applied to sequences, not single values. If you wish to apply a function to a single value/selection (including an array), use the [do](/api/java/do) command.

__Example:__ Return the first five squares.

```java
r.expr(r.array(1, 2, 3, 4, 5)).map(val -> r.mul(val, val)).run(conn);

// Result:
[1, 4, 9, 16, 25]
```

__Example:__ Sum the elements of three sequences.

```java
int[] sequence1 = { 100, 200, 300, 400 };
int[] sequence2 = { 10, 20, 30, 40 };
int[] sequence3 = { 1, 2, 3, 4 };
r.map(sequence1, sequence2, sequence3,
    (val1, val2, val3) -> r.add(val1, val2).add(val3)
).run(conn);

// Result:
[111, 222, 333, 444]
```

__Example:__ Rename a field when retrieving documents using `map` and [merge](/api/java/merge/).

This example renames the field `id` to `userId` when retrieving documents from the table `users`.

```java
r.table("users").map(
    doc -> doc.merge(r.hashMap("user_id", doc.g("id"))).without("id")
).run(conn);
``` 

__Example:__ Assign every superhero an archenemy.

```java
r.table("heroes").map(r.table("villains"),
    (hero, villain) -> hero.merge(r.hashMap("villain", villain))
).run(conn);
```
