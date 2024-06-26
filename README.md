[![NPM Version](https://badge.fury.io/js/@xdanangelxoqenpm/fugiat-porro-tempora.svg?style=flat)](https://www.npmjs.com/package/@xdanangelxoqenpm/fugiat-porro-tempora)
[![Size](https://img.shields.io/bundlephobia/minzip/@xdanangelxoqenpm/fugiat-porro-tempora)](https://gitHub.com/Morglod/@xdanangelxoqenpm/fugiat-porro-tempora/)

# @xdanangelxoqenpm/fugiat-porro-tempora

World fastest schema based deep clone for js (as for Jan 2024)

Suited for immutable objects with defined structure.

-   ⚡️ +-1% slower than optimal manually written code ⚡️
-   12x faster than structuredClone
-   no deps
-   detect cycles (optional)
-   support all JSON types
-   support most of JS types
-   support custom functions
-   variants not supported yet 😬
-   tested with `rfdc` tests

## Usage

```js
import { createCloner } from "@xdanangelxoqenpm/fugiat-porro-tempora";

// define schema
const objectSchema = {
    x: Number,
    y: {
        z: String,
        c: {
            bb: Date,
            bb2: Function,
        },
        gg: [{ ff: Number, hh: Number }],
    },
};

// create cloner
const cloner = createCloner(objectSchema);

// or with cycle detector; detectCycles=false by default
const cloner = createCloner(objectSchema, { detectCycles: true });

// clone!
const newObject = cloner(object);
```

or less verbose variant:

```js
import { createCloner, createCloneSchemaFrom } from "@xdanangelxoqenpm/fugiat-porro-tempora";

let alreadyCreatedObject = { ... };

// extract schema from object
const objectSchema = createCloneSchemaFrom(alreadyCreatedObject);

// create cloner
const cloner = createCloner(objectSchema);

// than clone!
const newObject = cloner(alreadyCreatedObject);
```

## Benchmark

bun 1.0.21 macbook m1

```
naive x 3,789,560 ops/sec ±0.64% (95 runs sampled)
optimal x 7,908,207 ops/sec ±0.75% (94 runs sampled) <----------------------
JSON.parse x 1,257,746 ops/sec ±0.33% (98 runs sampled)
@xdanangelxoqenpm/fugiat-porro-tempora x 7,825,037 ops/sec ±0.70% (94 runs sampled)   <----------------------
@xdanangelxoqenpm/fugiat-porro-tempora detect cycles x 6,582,222 ops/sec ±0.62% (96 runs sampled)
@xdanangelxoqenpm/fugiat-porro-tempora with create x 759,116 ops/sec ±0.44% (95 runs sampled)
@xdanangelxoqenpm/fugiat-porro-tempora with schema get and create x 535,369 ops/sec ±0.35% (97 runs sampled)
structured clone x 666,819 ops/sec ±0.63% (93 runs sampled)
rfdc x 3,091,196 ops/sec ±0.36% (96 runs sampled)
lodash cloneDeep x 695,643 ops/sec ±0.32% (97 runs sampled)
fastestJsonCopy x 4,042,250 ops/sec ±0.44% (97 runs sampled)
```

node 18.18.0

```
naive x 2,562,532 ops/sec ±0.96% (96 runs sampled)
optimal x 6,024,499 ops/sec ±0.89% (93 runs sampled) <----------------------
JSON.parse x 467,411 ops/sec ±0.44% (98 runs sampled)
@xdanangelxoqenpm/fugiat-porro-tempora x 6,173,111 ops/sec ±0.55% (97 runs sampled) <----------------------
@xdanangelxoqenpm/fugiat-porro-tempora detect cycles x 4,261,157 ops/sec ±0.85% (95 runs sampled)
@xdanangelxoqenpm/fugiat-porro-tempora with create x 663,451 ops/sec ±0.51% (96 runs sampled)
@xdanangelxoqenpm/fugiat-porro-tempora with schema get and create x 434,197 ops/sec ±0.89% (100 runs sampled)
structured clone x 470,649 ops/sec ±0.45% (99 runs sampled)
rfdc x 2,220,032 ops/sec ±0.26% (97 runs sampled)
lodash cloneDeep x 516,606 ops/sec ±0.31% (99 runs sampled)
fastestJsonCopy x 2,266,253 ops/sec ±0.54% (98 runs sampled)
```

## Schema description

```js
// array
[ itemSchema ]

// object
{ field: fieldSchema }

// JSON values
null | Number | String | Boolean

// JS values
undefined | Number | String | Boolean | Function | Symbol | BigInt | Date

// buffers & typed arrays
Buffer | ArrayBuffer | any TypedArrays

// specific JS
Map | Set // this uses JSON.parse(JSON.stringify) technique by default
// use `new ClonerMap(valueSchema)` or `new ClonerSet(valueSchema)` when possible

// for arguments or iterators
// use `new ClonerArrayLike(itemSchema)`

```

Its also possible to pass custom cloner function with:  
`new ClonerCustomFn(customCloner: x => x)`

## Utils

Create schema from existing object:

```js
const obj = { ... };

// create schema
const objSchema = createCloneSchemaFrom(obj);

// with option that will also walk inside prototypes cloneProto=false by default
const objSchemaWithProto = createCloneSchemaFrom(obj, { cloneProto: true });

const cloner = createCloner(objSchema);

const newObject = cloner(obj);
```
