---
layout: api-command
language: JavaScript
permalink: api/javascript/args/
command: args
io:
    -   - r
        - special
---

# Command syntax #

{% apibody %}
r.args(array) &rarr; special
{% endapibody %}

# Description #

`r.args` is a special term that's used to splice an array of arguments
into another term.  This is useful when you want to call a variadic
term such as [getAll](/api/javascript/get_all/) with a set of arguments produced at runtime.

This is analogous to using **apply** in JavaScript.

__Example:__ Get Alice and Bob from the table `people`.

```js
r.table('people').getAll('Alice', 'Bob').run(conn)
// or
r.table('people').getAll(r.args(['Alice', 'Bob'])).run(conn)
```

__Example:__ Get all of Alice's children from the table `people`.

```js
// r.table('people').get('Alice') returns {id: 'Alice', children: ['Bob', 'Carol']}
r.table('people').getAll(r.args(r.table('people').get('Alice')('children'))).run(conn)
```

__Note:__ When using `r.args` with a command that takes optional arguments, you must not include the optional arguments inside the `args` array.

```js
// Wrong!
r.table('posts').indexCreate(r.args(['tags', {multi: true}]))
// Right
r.table('posts').indexCreate(r.args(['tags']), {multi: true})
```
