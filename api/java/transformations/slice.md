---
layout: api-command
language: Java
permalink: api/java/slice/
command: slice
related_commands:
    orderBy: order_by/
    skip: skip/
    limit: limit/
    nth: nth/
---

# Command syntax #

{% apibody %}
selection.slice(startIndex[, endIndex]) &rarr; selection
stream.slice(startIndex[, endIndex]) &rarr; stream
array.slice(startIndex[, endIndex]) &rarr; array
binary.slice(startIndex[, endIndex]) &rarr; binary
{% endapibody %}

# Description #

Return the elements of a sequence within the specified range.

`slice` returns the range between `startIndex` and `endIndex`. If only `startIndex` is specified, `slice` returns the range from that index to the end of the sequence. Use the [optArgs](/api/java/optarg) `left_bound` or `right_bound` as `open` or `closed` to indicate whether to include that endpoint of the range by default: `closed` returns that endpoint, while `open` does not. By default, `left_bound` is closed and `right_bound` is open, so the range `(10,13)` will return the tenth, eleventh and twelfth elements in the sequence.

If `endIndex` is past the end of the sequence, all elements from `startIndex` to the end of the sequence will be returned. If `startIndex` is past the end of the sequence or `endIndex` is less than `startIndex`, a zero-element sequence will be returned (although see below for negative `endIndex` values). An error will be raised on a negative `startIndex`.

A negative `endIndex` is allowed with arrays; in that case, the returned range counts backward from the array's end. That is, the range of `(2,-1)` returns the second element through the next-to-last element of the range. A negative `endIndex` is not allowed with a stream. (An `endIndex` of &minus;1 *is* allowed with a stream if `rightBound` is closed; this behaves as if no `endIndex` was specified.)

If `slice` is used with a [binary](/api/java/binary) object, the indexes refer to byte positions within the object. That is, the range `(10,20)` will refer to the 10th byte through the 19th byte.

**Example:** Return the fourth, fifth and sixth youngest players. (The youngest player is at index 0, so those are elements 3&ndash;5.)

```java
r.table("players").orderBy().optArg("index", "age").slice(3, 6).run(conn);
```

**Example:** Return all but the top three players who have a red flag.

```java
r.table("players").filter(r.hashMap("flag", "red")).orderBy()
 .optArg("index", r.desc("score")).slice(3).run(conn);
```

**Example:** Return holders of tickets `X` through `Y`, assuming tickets are numbered sequentially. We want to include ticket `Y`.

```java
r.table("users").orderBy().optArg("index", "ticket")
 .slice(x, y).optArg("right_bound", "closed").run(conn);
```

**Example:** Return the elements of an array from the second through two from the end (that is, not including the last two).

```java
r.expr(r.array(0, 1, 2, 3, 4, 5)).slice(2, -2).run(conn);
```

Result:

```json
[2,3]
```
