---
layout: api-command
language: Java
permalink: api/java/offsets_of/
command: offsetsOf
alias: api/java/indexes_of/
io:
    -   - sequence
        - array
---

# Command syntax #

{% apibody %}
sequence.offsetsOf(datum | predicate_function) &rarr; array
{% endapibody %}

# Description #

Get the indexes of an element in a sequence. If the argument is a predicate, get the indexes of all elements matching it.

__Example:__ Find the position of the letter 'c'.

```java
r.expr(r.array("a", "b", "c")).offsetsOf("c").run(conn);
```

__Example:__ Find the popularity ranking of invisible heroes.

```java
r.table("marvel").union(r.table("dc")).orderBy("popularity").offsetsOf(
    row -> row.g("superpowers").contains("invisibility")
).run(conn);
```
