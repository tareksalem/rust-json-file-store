[![](http://meritbadge.herokuapp.com/jfs)](https://crates.io/crates/jfs)
[![Build Status](https://travis-ci.org/flosse/rust-json-file-store.svg?branch=master)](https://travis-ci.org/flosse/rust-json-file-store)
[![Dependency Status](https://dependencyci.com/github/flosse/rust-json-file-store/badge)](https://dependencyci.com/github/flosse/rust-json-file-store)

# jfs

A simple JSON file store written in Rust.
This is a port and drop-in replacement of the Node.js library
[json-file-store](https://github.com/flosse/json-file-store/).

**WARNING**:
Don't use it if you want to persist a large amount of objects.
Use a real DB instead.

## Installation

Depending on which serialization framework you like to use,
you have to enable it by setting the corresponding features
in `Cargo.toml`.

By default `rustc-serialize` is used. To enable support for serde
add the following to your configuration:


```toml
[dependencies.jfs]
version = "0.3"
features = ["serde", "serde_json"]
default-features = false
```

## Example

```rust
extern crate jfs;
extern crate rustc_serialize;
use jfs::Store;

#[derive(RustcEncodable,RustcDecodable)]
struct Foo {
  foo: String
}

pub fn main() {
   let db = Store::new("data").unwrap();
   let f = Foo { foo: "bar".to_owned() };
   let id = db.save(&f).unwrap();
   let obj = db.get::<Foo>(&id).unwrap();
   db.delete(&id).unwrap();
}
```

You can also store all data in one single JSON-File:

```rust
let mut cfg = jfs::Config::default();
cfg.single = true; // false is default
let db = jfs::Store::new_with_cfg("data",cfg);
```

If you like to pretty print the file content, set `pretty` to `true`
and choose a number of whitespaces for the indention:

```rust
let mut cfg = jfs::Config::default();
cfg.pretty = true;  // false is default
cfg.indent = 4;     // 2 is default
```

## License

This project is licensed under the MIT License.
