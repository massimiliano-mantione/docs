---
layout: api-command
language: Java
permalink: api/java/optarg/
command: optArg
rb: false
py: false
js: false
---

# Command syntax #

{% apibody %}
term.optArg(option, value)
{% endapibody %}

# Description #

Specify an optional argument to a Java ReQL term.

Some terms in ReQL accept optional arguments. Since Java doesn't support named arguments, the RethinkDB Java driver allows you to pass them by chaining the `optArg` command after them.

__Example:__ Pass the `right_bound` optional argument to [between](/api/java/between/).

```java
r.table("marvel").between(10, 20).optArg("right_bound", "closed").run(conn);
```

To pass more than one optional argument, chain `optArg` once for each argument.


__Example:__ Pass the `right_bound` and `index` optional arguments to [between](/api/java/between/).

```java
r.table("marvel").between(10, 20).optArg("right_bound", "closed")
 .optArg("index", "power").run(conn);
```
