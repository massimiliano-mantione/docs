---
layout: api-command 
language: JavaScript
permalink: api/javascript/year/
command: year 
github_doc: https://github.com/rethinkdb/docs/edit/master/2-query-language/api/javascript/dates-and-times/year.md
io:
    -   - time
        - number
related_commands:
    now: now/
    time: time/
---

{% apibody %}
time.year() &rarr; number
{% endapibody %}

Return the year of a time object.

__Example:__ Retrieve all the users born in 1986.

```js
r.table("users").filter(function(user) {
    return user("birthdate").year().eq(1986)
}).run(conn, callback)
```