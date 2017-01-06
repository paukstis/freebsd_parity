### Building [Parity](https://github.com/ethcore/parity) on [FreeBSD](https://www.freebsd.org)

## Status
Tested with following components:
- FreeBSD 11
- Rust 1.13, 1.14
- Parity 1.4.6, 1.4.7

## Requirements
Freshly installed FreeBSD system or jail needs following packages to be installed:
```shell
pkg install lang/rust devel/cargo devel/git net/nanomsg
```
For runtime only `net/nanomsg` is required.

## Patching
Make sure to use correct branches with `git checkout ..`
```shell
cd ~/src
git clone https://github.com/ethcore/parity.git
cd parity
git checkout v1.4.7
cd ~/src
git clone https://mug.uoga.net/freebsd/freebsd_parity.git
cd freebsd_parity
git checkout v1.4
```

Check if patch will succeed with --dry-run, apply it, then clean .orig files
```shell
cd ~/src/parity
patch --dry-run --batch --quiet -p1 < ../freebsd_parity/parity.patch && patch -p1 < ../freebsd_parity/parity.patch && git clean -f && echo DONE
```
Confirm that last line says `DONE`, which means patch was successful.

## Building
To build:
```shell
cargo clean
cargo update --package gcc --precise 0.3.41
cargo build --release
```

## Reverting patch
To undo the patch run:
```shell
git reset --hard
```
