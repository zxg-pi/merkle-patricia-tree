<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

## SecureTrie

[src/secure.js:12-55][1]

**Extends Trie**

You can create a secure Trie where the keys are automatically hashed
using **keccak256** by using `require('merkle-patricia-tree/secure')`.
It has the same methods and constructor as `Trie`.

## Trie

[src/baseTrie.js:25-768][2]

Use `require('merkel-patricia-tree')` for the base interface. In Ethereum applications
stick with the Secure Trie Overlay `require('merkel-patricia-tree/secure')`.
The API for the raw and the secure interface are about the same

### Parameters

-   `db` **[Object][3]?** A [levelup][4] instance. By default creates an in-memory [memdown][5] instance.
    If the db is `null` or left undefined, then the trie will be stored in memory via [memdown][5]
-   `root` **([Buffer][6] \| [String][7])?** A hex `String` or `Buffer` for the root of a previously stored trie

### Properties

-   `root` **[Buffer][6]** The current root of the `trie`
-   `EMPTY_TRIE_ROOT` **[Buffer][6]** the Root for an empty trie

### get

[src/baseTrie.js:92-104][8]

Gets a value given a `key`

#### Parameters

-   `key` **([Buffer][6] \| [String][7])** the key to search for
-   `cb` **[Function][9]** A callback `Function` which is given the arguments `err` - for errors that may have occured and `value` - the found value in a `Buffer` or if no value was found `null`

### put

[src/baseTrie.js:114-138][10]

Stores a given `value` at the given `key`

#### Parameters

-   `key` **([Buffer][6] \| [String][7])** 
-   `Value` **([Buffer][6] \| [String][7])** 
-   `cb` **[Function][9]** A callback `Function` which is given the argument `err` - for errors that may have occured

### del

[src/baseTrie.js:147-163][11]

deletes a value given a `key`

#### Parameters

-   `key` **([Buffer][6] \| [String][7])** 
-   `callback` **[Function][9]** the callback `Function`

### findPath

[src/baseTrie.js:226-272][12]

Tries to find a path to the node for the given key
It returns a `stack` of nodes to the closet node

#### Parameters

-   `null-null` **([String][7] \| [Buffer][6])** key - the search key
-   `null-null` **[Function][9]** cb - the callback function. Its is given the following
    arguments-   err - any errors encontered
    -   node - the last node found
    -   keyRemainder - the remaining key nibbles not accounted for
    -   stack - an array of nodes that forms the path to node we are searching for

### createReadStream

[src/baseTrie.js:716-718][13]

The `data` event is given an `Object` hat has two properties; the `key` and the `value`. Both should be Buffers.

Returns **[stream.Readable][14]** Returns a [stream][15] of the contents of the `trie`

### batch

[src/baseTrie.js:743-753][16]

The given hash of operations (key additions or deletions) are executed on the DB

#### Parameters

-   `ops` **[Array][17]** 
-   `cb` **[Function][9]** 

#### Examples

```javascript
var ops = [
   { type: 'del', key: 'father' }
 , { type: 'put', key: 'name', value: 'Yuri Irsenovich Kim' }
 , { type: 'put', key: 'dob', value: '16 February 1941' }
 , { type: 'put', key: 'spouse', value: 'Kim Young-sook' }
 , { type: 'put', key: 'occupation', value: 'Clown' }
]
trie.batch(ops)
```

### checkRoot

[src/baseTrie.js:762-767][18]

Checks if a given root exists

#### Parameters

-   `root` **[Buffer][6]** 
-   `cb` **[Function][9]** 

## Merkle Proof

Static functions for creating/verifying a merkle proof.


## Internal Util Functions

These are not exposed.


### addHexPrefix

[src/util/hex.js:7-22][19]

Prepends hex prefix to an array of nibbles.

#### Parameters

-   `Array` **[Array][17]** of nibbles

Returns **[Array][17]** returns buffer of encoded data
\*

### asyncFirstSeries

[src/util/async.js:38-54][20]

Take a collection of async fns, call the cb on the first to return a truthy value.
If all run without a truthy result, return undefined

#### Parameters

-   `array`  
-   `iterator`  
-   `cb`  

### doKeysMatch

[src/util/nibbles.js:56-59][21]

Compare two nibble array keys.

#### Parameters

-   `keyA` **[Array][17]** 
-   `keyB` **[Array][17]** 

## 

[src/db.js:3-3][22]

DB is a thin wrapper around the underlying levelup db,
which validates inputs and sets encoding type.

## 

[src/util/async.js:3-6][23]

Take two or more functions and returns a function  that will execute all of
the given functions

## 

[src/scratch.js:4-4][24]

An in-memory wrap over `DB` with an upstream DB
which will be queried when a key is not found
in the in-memory scratch. This class is used to implement
checkpointing functionality in CheckpointTrie.

## constructor

[src/db.js:15-17][25]

Initialize a DB instance. If `leveldb` is not provided, DB
defaults to an [in-memory store][5].

### Parameters

-   `leveldb` **[Object][3]?** An abstract-leveldown compliant store

## get

[src/db.js:26-36][26]

Retrieves a raw value from leveldb.

### Parameters

-   `key` **[Buffer][6]** 
-   `cb` **[Function][9]** A callback `Function`, which is given the arguments
    `err` - for errors that may have occured
    and `value` - the found value in a `Buffer` or if no value was found `null`.

## put

[src/db.js:45-50][27]

Writes a value directly to leveldb.

### Parameters

-   `key` **[Buffer][6]** The key as a `Buffer` or `String`
-   `val`  
-   `cb` **[Function][9]** A callback `Function`, which is given the argument
    `err` - for errors that may have occured
-   `value` **[Buffer][6]** The value to be stored

## del

[src/db.js:58-62][28]

Removes a raw value in the underlying leveldb.

### Parameters

-   `key` **[Buffer][6]** 
-   `cb` **[Function][9]** A callback `Function`, which is given the argument
    `err` - for errors that may have occured

## batch

[src/db.js:70-74][29]

Performs a batch operation on db.

### Parameters

-   `opStack` **[Array][17]** A stack of levelup operations
-   `cb` **[Function][9]** A callback `Function`, which is given the argument
    `err` - for errors that may have occured

## copy

[src/db.js:80-82][30]

Returns a copy of the DB instance, with a reference
to the **same** underlying leveldb instance.

## isCheckpoint

[src/checkpointTrie.js:22-24][31]

Is the trie during a checkpoint phase?

## copy

[src/checkpointTrie.js:97-106][32]

Returns a copy of the underlying trie with the interface
of CheckpointTrie. If during a checkpoint, the copy will
contain the checkpointing metadata (incl. reference to the same scratch).

### Parameters

-   `includeCheckpoints` **[boolean][33]** If true and during a checkpoint, the copy will
    contain the checkpointing metadata and will use the same scratch as underlying db. (optional, default `true`)

## putRaw

[src/checkpointTrie.js:113-115][34]

Writes a value under given key directly to the
key/value db, disregarding checkpoints.

### Parameters

-   `key`  
-   `value`  
-   `cb`  

**Meta**

-   **deprecated**: This is deprecated.


## get

[src/scratch.js:22-35][35]

Similar to `DB.get`, but first searches in-memory
scratch DB, if key not found, searches upstream DB.

### Parameters

-   `key`  
-   `cb`  

## checkpoint

[src/checkpointTrie.js:33-41][36]

Creates a checkpoint that can later be reverted to or committed.
After this is called, no changes to the trie will be permanently saved
until `commit` is called. Calling `putRaw` overrides the checkpointing
mechanism and would directly write to db.

## put

[src/secure.js:42-49][37]

For a falsey value, use the original key
to avoid double hashing the key.

### Parameters

-   `key`  
-   `val`  
-   `cb`  

## commit

[src/checkpointTrie.js:50-65][38]

Commits a checkpoint to disk, if current checkpoint is not nested. If
nested, only sets the parent checkpoint as current checkpoint.

### Parameters

-   `cb` **[Function][9]** the callback


-   Throws **any** If not during a checkpoint phase

## revert

[src/checkpointTrie.js:74-88][39]

Reverts the trie to the state it was at when `checkpoint` was first called.
If during a nested checkpoint, sets root to most recent checkpoint, and sets
parent checkpoint as current.

### Parameters

-   `cb` **[Function][9]** the callback

## getRaw

[src/baseTrie.js:169-171][40]

Retrieves a value directly from key/value db.

### Parameters

-   `key`  
-   `cb`  

**Meta**

-   **deprecated**: This is deprecated.


## putRaw

[src/baseTrie.js:178-180][41]

Writes a value under given key directly to the
key/value db.

### Parameters

-   `key`  
-   `value`  
-   `cb`  

**Meta**

-   **deprecated**: This is deprecated.


## delRaw

[src/baseTrie.js:186-188][42]

Deletes key directly from underlying key/value db.

### Parameters

-   `key`  
-   `cb`  

**Meta**

-   **deprecated**: This is deprecated.


[1]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/secure.js#L12-L55 "Source code on GitHub"

[2]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L25-L768 "Source code on GitHub"

[3]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object

[4]: https://github.com/Level/levelup

[5]: https://github.com/Level/memdown

[6]: https://nodejs.org/api/buffer.html

[7]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String

[8]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L92-L104 "Source code on GitHub"

[9]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function

[10]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L114-L138 "Source code on GitHub"

[11]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L147-L163 "Source code on GitHub"

[12]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L226-L272 "Source code on GitHub"

[13]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L716-L718 "Source code on GitHub"

[14]: https://nodejs.org/api/stream.html#stream_class_stream_readable

[15]: https://nodejs.org/dist/latest-v5.x/docs/api/stream.html#stream_class_stream_readable

[16]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L743-L753 "Source code on GitHub"

[17]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array

[18]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L762-L767 "Source code on GitHub"

[19]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/util/hex.js#L7-L22 "Source code on GitHub"

[20]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/util/async.js#L38-L54 "Source code on GitHub"

[21]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/util/nibbles.js#L56-L59 "Source code on GitHub"

[22]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/db.js#L3-L3 "Source code on GitHub"

[23]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/util/async.js#L3-L6 "Source code on GitHub"

[24]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/scratch.js#L4-L4 "Source code on GitHub"

[25]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/db.js#L15-L17 "Source code on GitHub"

[26]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/db.js#L26-L36 "Source code on GitHub"

[27]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/db.js#L45-L50 "Source code on GitHub"

[28]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/db.js#L58-L62 "Source code on GitHub"

[29]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/db.js#L70-L74 "Source code on GitHub"

[30]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/db.js#L80-L82 "Source code on GitHub"

[31]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/checkpointTrie.js#L22-L24 "Source code on GitHub"

[32]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/checkpointTrie.js#L97-L106 "Source code on GitHub"

[33]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean

[34]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/checkpointTrie.js#L113-L115 "Source code on GitHub"

[35]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/scratch.js#L22-L35 "Source code on GitHub"

[36]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/checkpointTrie.js#L33-L41 "Source code on GitHub"

[37]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/secure.js#L42-L49 "Source code on GitHub"

[38]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/checkpointTrie.js#L50-L65 "Source code on GitHub"

[39]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/checkpointTrie.js#L74-L88 "Source code on GitHub"

[40]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L169-L171 "Source code on GitHub"

[41]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L178-L180 "Source code on GitHub"

[42]: https://github.com/ethereumjs/merkle-patricia-tree/blob/2685d6759e18abeba0967b151bf965a5b1f9eec8/src/baseTrie.js#L186-L188 "Source code on GitHub"
