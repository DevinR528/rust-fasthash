## rust-fasthash [![travis build](https://travis-ci.org/flier/rust-fasthash.svg?branch=master)](https://travis-ci.org/flier/rust-fasthash) [![crate](https://img.shields.io/crates/v/fasthash.svg)](https://crates.io/crates/fasthash) [![docs](https://docs.rs/fasthash/badge.svg)](https://docs.rs/crate/fasthash/)
A suite of non-cryptographic hash functions for Rust, binding the [smhasher](https://github.com/rurban/smhasher/).

## Usage

```toml
[dependencies]
fasthash = "0.4"
```

### `hash` and `hash_with_seed` function

```rust
use fasthash::*;

let h = city::hash64("hello world");

let h = metro::hash64_with_seed("hello world", 123);
```

### `std::hash::Hash`

```rust
use std::hash::Hash;

use fasthash::MetroHasher;

fn hash<T: Hash>(t: &T) -> u64 {
    let mut s = MetroHasher::new();
    t.hash(&mut s);
    s.finish()
}

hash(&"hello world");
```

### `HashMap` and `HashSet`

```rust
use std::collections::HashSet;

use fasthash::spooky::SpookyHash128;

let mut set = HashSet::with_hasher(SpookyHash128);

set.insert(2);
```

### `RandomState`

```rust
use std::collections::HashMap;

use fasthash::RandomState;
use fasthash::city::CityHash64;

let s = RandomState::<CityHash64>::new();
let mut map = HashMap::with_hasher(s);

assert_eq!(map.insert(37, "a"), None);
assert_eq!(map.is_empty(), false);

map.insert(37, "b");
assert_eq!(map.insert(37, "c"), Some("b"));
assert_eq!(map[&37], "c");
```

## Hash Functions

- Modern Hash Functions
  - [City Hash](https://github.com/google/cityhash)
  - [Farm Hash](https://github.com/google/farmhash)
  - [Metro Hash](https://github.com/jandrewrogers/MetroHash)
  - [Mum Hash](https://github.com/vnmakarov/mum-hash)
  - [Murmur Hash](https://sites.google.com/site/murmurhash/)
  - [Lookup3](https://en.wikipedia.org/wiki/Jenkins_hash_function)
  - [Sea Hash](https://github.com/ticki/tfs/tree/master/seahash)
  - [Spooky Hash](http://burtleburtle.net/bob/hash/spooky.html)
  - [T1ha Hash](https://github.com/leo-yuriev/t1ha)
  - [xx Hash](https://github.com/Cyan4973/xxHash) with  **experimental** [XXH3](https://github.com/Cyan4973/xxHash#new-experimental-hash-algorithm) hash algorithm
  - [Highway Hash](https://github.com/google/highwayhash)
- Compatibility
  - [Hasher](https://doc.rust-lang.org/std/hash/trait.Hasher.html)
  - std::collections::{[HashMap](https://doc.rust-lang.org/std/collections/struct.HashMap.html), [HashSet](https://doc.rust-lang.org/std/collections/struct.HashSet.html)} with `RandomState`
  - [Digest](https://docs.rs/digest/0.8.1/digest/trait.Digest.html) (optional)

## Benchmark

```bash
$ cargo bench
```
