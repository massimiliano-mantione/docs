---
layout: api-command
language: Java
permalink: api/java/sample/
command: sample
io:
    -   - sequence
        - selection
    -   - stream
        - array
    -   - array
        - array
---

# Command syntax #

{% apibody %}
sequence.sample(number) &rarr; selection
stream.sample(number) &rarr; array
array.sample(number) &rarr; array
{% endapibody %}

# Description #

Select a given number of elements from a sequence with uniform random distribution. Selection is done without replacement.

If the sequence has less than the requested number of elements (i.e., calling `sample(10)` on a sequence with only five elements), `sample` will return the entire sequence in a random order.

__Example:__ Select 3 random heroes.

```java
r.table("marvel").sample(3).run(conn);
```
