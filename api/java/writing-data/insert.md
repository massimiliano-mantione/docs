---
layout: api-command
language: Java
permalink: api/java/insert/
command: insert
related_commands:
    update: update/
    replace: replace/
    delete: delete/
---

# Command syntax #

{% apibody %}
table.insert(object | [object1, object2, ...]) &rarr; object
{% endapibody %}

# Description #

<img src="/assets/images/docs/api_illustrations/insert_javascript.png" class="api_command_illustration" />

You can pass the following options using [optArg](/api/java/optarg/):

- `durability`: possible values are `hard` and `soft`. This option will override the table or query's durability setting (set in [run](/api/java/run/)). In soft durability mode RethinkDB will acknowledge the write immediately after receiving and caching it, but before the write has been committed to disk.
- `return_changes`:
    - `true`: return a `changes` array consisting of `old_val`/`new_val` objects describing the changes made, only including the documents actually updated.
    - `false`: do not return a `changes` array (the default).
    - `"always"`: behave as `true`, but include all documents the command tried to update whether or not the update was successful. (This was the behavior of `true` pre-2.0.)
- `conflict`: Determine handling of inserting documents with the same primary key as existing entries. Possible values are `"error"`, `"replace"` or `"update"`.
    - `"error"`: Do not insert the new document and record the conflict as an error. This is the default.
    - `"replace"`: [Replace](/api/java/replace/) the old document in its entirety with the new one.
    - `"update"`: [Update](/api/java/update/) fields of the old document with fields from the new one.

Insert returns an object that contains the following attributes:

- `inserted`: the number of documents successfully inserted.
- `replaced`: the number of documents updated when `conflict` is set to `"replace"` or `"update"`.
- `unchanged`: the number of documents whose fields are identical to existing documents with the same primary key when `conflict` is set to `"replace"` or `"update"`.
- `errors`: the number of errors encountered while performing the insert.
- `first_error`: If errors were encountered, contains the text of the first error.
- `deleted` and `skipped`: 0 for an insert operation.
- `generated_keys`: a list of generated primary keys for inserted documents whose primary keys were not specified (capped to 100,000).
- `warnings`: if the field `generated_keys` is truncated, you will get the warning _"Too many generated keys (&lt;X&gt;), array truncated to 100000."_.
- `changes`: if `returnChanges` is set to `true`, this will be an array of objects, one for each objected affected by the `insert` operation. Each object will have two keys: `{new_val: <new value>, old_val: null}`.

__Example:__ Insert a document into the table `posts`.

```java
r.table("posts").insert(
    r.hashMap("id", 1)
     .with("title", "Lorem ipsum")
     .with("content", "Dolor sit amet")
).run(conn);
```

The result will be:

```json
{
    "deleted": 0,
    "errors": 0,
    "inserted": 1,
    "replaced": 0,
    "skipped": 0,
    "unchanged": 0
}
```


__Example:__ Insert a document without a defined primary key into the table `posts` where the
primary key is `id`.

```java
r.table("posts").insert(
    r.hashMap("title", "Lorem ipsum")
     .with("content", "Dolor sit amet")
).run(conn);
```

RethinkDB will generate a primary key and return it in `generated_keys`.

```json
{
    "deleted": 0,
    "errors": 0,
    "generated_keys": [
        "dd782b64-70a7-43e4-b65e-dd14ae61d947"
    ],
    "inserted": 1,
    "replaced": 0,
    "skipped": 0,
    "unchanged": 0
}
```

Retrieve the document you just inserted with:

```java
r.table("posts").get("dd782b64-70a7-43e4-b65e-dd14ae61d947").run(conn);
```

And you will get back:

```json
{
    "id": "dd782b64-70a7-43e4-b65e-dd14ae61d947",
    "title": "Lorem ipsum",
    "content": "Dolor sit amet",
}
```


__Example:__ Insert multiple documents into the table `users`.

```java
r.table("users").insert(r.array(
    r.hashMap("id", "william").with("email", "william@rethinkdb.com"),
    r.hashMap("id", "lara").with("email", "lara@rethinkdb.com")
)).run(conn);
```


__Example:__ Insert a document into the table `users`, replacing the document if it already exists.  

```java
r.table("users").insert(
    r.hashMap("id", "william").with("email", "william@rethinkdb.com")
).optArg("conflict", "replace").run(conn);
```


__Example:__ Copy the documents from `posts` to `postsBackup`.

```java
r.table("postsBackup").insert(r.table("posts")).run(conn);
```


__Example:__ Get back a copy of the inserted document (with its generated primary key).

```java
r.table("posts").insert(
    r.hashMap("title", "Lorem ipsum")
     .with("content", "Dolor sit amet")
).optArg("return_changes", true).run(conn);
```

The result will be

```json
{
    "deleted": 0,
    "errors": 0,
    "generated_keys": [
        "dd782b64-70a7-43e4-b65e-dd14ae61d947"
    ],
    "inserted": 1,
    "replaced": 0,
    "skipped": 0,
    "unchanged": 0,
    "changes": [
        {
            "old_val": null,
            "new_val": {
                "id": "dd782b64-70a7-43e4-b65e-dd14ae61d947",
                "title": "Lorem ipsum",
                "content": "Dolor sit amet"
            }
        }
    ]
}
```
