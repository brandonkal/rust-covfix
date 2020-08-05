# Tasks

## dl-rust-source

```
rustup component add rust-src --toolchain nightly
```

## build//default

```bash
name=${PWD##*/}
cargo build --release
strip target/release/$name
cd target/release
platform=`echo $(uname -s) | tr '[:upper:]' '[:lower:]'`
tar -czf ${name}-${platform}.tar.gz $name
```
