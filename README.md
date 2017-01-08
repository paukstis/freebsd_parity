![build status](https://mug.uoga.net/freebsd/freebsd_parity/badges/master/build.svg)
### Building [Parity](https://github.com/ethcore/parity) on [FreeBSD](https://www.freebsd.org)

## Status
Tested with following components:
- FreeBSD 11
- Rust 1.13, 1.14
- Parity master branch

## Requirements
To build Parity on FreeBSD a freshly installed system or jail needs following packages to be installed:
```shell
pkg install nanomsg cargo git
```
For runtime only `nanomsg` is required.

## Patching

```shell
cd ~/src
git clone https://github.com/ethcore/parity.git
git clone https://mug.uoga.net/freebsd/freebsd_parity.git
```

Check if patch will succeed with --dry-run, apply it, then clean .orig files
```shell
cd ~/src/parity
patch --dry-run --batch --quiet -p1 < ../freebsd_parity/parity.patch && patch -p1 < ../freebsd_parity/parity.patch && git clean -f && echo DONE
```
Confirm that last line says `DONE`, which means patch was successful.

## Building
To build after patching:
```shell
cargo clean
cargo update --package gcc --precise 0.3.41
cargo update --package openssl --precise 0.9.4
OPENSSL_INCLUDE_DIR=/usr/include OPENSSL_LIB_DIR=/usr/lib cargo build --release
```

## Reverting patch
To undo the patch run:
```shell
git reset --hard
```
