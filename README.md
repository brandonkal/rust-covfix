# rust-covfix
[![Build Status](https://travis-ci.org/Kogia-sima/rust-covfix.svg?branch=master)](https://travis-ci.org/Kogia-sima/rust-covfix)
[![Build status](https://ci.appveyor.com/api/projects/status/xc9jauk9nah5webf/branch/master?svg=true)](https://ci.appveyor.com/project/Kogiasima/rust-covfix/branch/master)
[![codecov](https://codecov.io/gh/Kogia-sima/rust-covfix/branch/master/graph/badge.svg)](https://codecov.io/gh/Kogia-sima/rust-covfix)
[![Version](https://img.shields.io/crates/v/rust-covfix)](https://crates.io/crates/rust-covfix)
[![docs](https://docs.rs/rust-covfix/badge.svg)](https://docs.rs/rust-covfix)
![GitHub Release Date](https://img.shields.io/github/release-date/Kogia-sima/rust-covfix?label=last%20release)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/Kogia-sima/rust-covfix/blob/master/LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

Rustc is known to report an incorrect coverage for some lines (https://stackoverflow.com/questions/32521800/why-does-kcov-calculate-incorrect-code-coverage-statistics-for-rust-programs).
`rust-covfix` will read coverage from the file generated by [grcov](https://github.com/mozilla/grcov/)), fix it, then outputs the correct coverage.

Though only `lcov` format is supprted at current, Another formats is going to be supported in future releases.

## Features

- Compatible with latest stable/beta/nightly Rust compiler
- Windows/OSX/Linux are all supprted
- Lightweight (small dependencies)
- Fast and safe (implemented in Rust language)
- `rust-covfix` is also available with Rust API ([Documentation](https://docs.rs/rust-covfix))

### Optional features

Optional features are available with cargo's `--features` option. You can specify the features like:

```console
$ cargo install --no-default-features --features "cli lcov"
```

|Feature name|Description|Default?|
|:--:|--|:--:|
|cli|Command Line Interface. This feature is required to build `rust-covfix` executable.|yes|
|noinline|Avoid adding `#cfg[inline]` attribute on function.|no|
|lcov|Make LcovParser available|yes|
|backtrace|Dump backtrace information on every time the error has occured.|yes|

## Install

Download the latest release from [GitHub Release Page](https://github.com/Kogia-sima/rust-covfix/releases).

You can also install via `cargo` command.

```rust
$ cargo install rust-covfix
```

## How is the incorrect line coverage detected

`rust_covfix` fixes the coverage information using some rules. You can pass `--rules` option to specify which rules are used to fix coverages.

### Rules

#### close

closing brackets, line of `else` block will be ignored.

```rust
if a > 0 {
    b = a
} else {  // <-- marked as "not executable"
    b = -a
};  // <-- marked as "not executable"
```

#### test

module block named `test` or `tests` which has attribute `cfg(test)` will be ignored

```rust
#[cfg(test)]
mod tests {  // <-- removed from coverage
    fn util() { ... }  // <-- removed from coverage

    #[test]
    fn test_hoge() { ... }  // <-- removed from coverage
}
```

#### loop

Fix rust internal bugs that loop branches are not correctly passed.

```rust
for i in 0..10 {  // <-- fix branch coverage information
    println!("{}", i);
}
```

#### derive

structs with `derive(...)` attribute will be ignored

```rust
#[derive(Clone, Debug)]  // <-- removed from coverage
struct Point {  // <-- removed from coverage
  x: f64,  // <-- removed from coverage
  y: f64  // <-- removed from coverage
}  // <-- removed from coverage
```

## Roadmap

- Support `cobertura.xml` file. (WIP)
- Add option for uploading the correct coverages to coveralls.
- Show summary of coverage difference.

## Author

👤 **Kogia-sima**

* Twitter: [@Kogia\_sima](https://twitter.com/Kogia\_sima)
* Github: [@Kogia-sima](https://github.com/Kogia-sima)

## 🤝 Contributing

Contributions, issues and feature requests are welcome!

Feel free to check [issues page](https://github.com/Kogia-sima/rust-covfix/issues). 

## Show your support

Give a ⭐️ if this project helped you!

## 📝 License

Copyright © 2019 [Kogia-sima](https://github.com/Kogia-sima).

This project is [MIT](https://github.com/Kogia-sima/rust-covfix/blob/master/LICENSE) licensed.

***
_This README was generated with ❤️ by [readme-md-generator](https://github.com/kefranabg/readme-md-generator)_
