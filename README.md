# evolve-evolved

A library for efficiently mapping objects from one type to another. Based in part on the concept behind [`ramda.evolve`](https://ramdajs.com/docs/#evolve).

```ts
import { evolve, rename, remove } from 'evolve-evolved'
import { compose, toUpper, head, append } from 'ramda'

const from = {
  a: 'hello',
  b: true,
  c: 'life is swell',
  d: {
    e: ['first'],
  },
  f: 'bye-bye',
  y: 'i am y',
}

const apply = evolve({
  a: toUpper,
  b: 'now a string',
  c: rename(
    'z',
    compose(
      toUpper,
      head,
    ),
  ),
  d: {
    e: append('last'),
  },
  f: remove,
  x: toUpper, // not applied, because it doesn't exist in the object
})

apply(from) ===
  {
    // transformed by `toUpper`
    a: 'HELLO',

    // replaced by raw value
    b: 'now a string',

    // appended value to d.e
    d: {
      e: ['first', 'last'],
    },

    // f: 'bye-bye' // removed from the final object by `remove`

    // no transformation provided, so passed straight through
    y: 'i am y',

    // renamed from `c` key to `z` key by `rename`
    // value transformed by `toUpper` and `head`
    z: 'L',
  }
```

# Why?

Mapping objects in a modern frontend project can be tiresome. Small variations of object types tend to exist between the various layers of the application (backends, APIs, form values, etc.). For complex types, this leads to large mapping functions, with many paths of branching logic, simply to

`evolve-evolved` allows you to apply an object of transformations to an object, transforming the entire shape of the object at once.
