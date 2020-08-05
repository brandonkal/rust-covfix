## dl-rust-source

```
rustup component add rust-src --toolchain nightly
```

## build//default

```sh
cargo build --release
strip target/release/rust-covfix
```
