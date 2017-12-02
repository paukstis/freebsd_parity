### Building [Parity](https://github.com/paritytech/parity) on [FreeBSD](https://www.freebsd.org)

## Status
Tested with following components:
- FreeBSD 11.1
- Rust 1.22
- Parity 1.6.10

## Patch details
- gcc crate building on FreeBSD, needs at least 0.3.41
- nanomsg bits building on FreeBSD
- rust-secp256k1 code before android changes, otherwise building fails with some errors abouts missing android bits
- harware wallet code disabled, required libusb is not available on FreeBSD

## Requirements
To build Parity on FreeBSD a freshly installed system or jail needs following packages to be installed:
```shell
pkg install git rust nanomsg
```
For runtime only `nanomsg` is required.

## Patching

```shell
cd ~/src
git clone https://github.com/paritytech/parity.git -b v1.6.10 parity
git clone https://github.com/paukstis/freebsd_parity.git -b v1.6 freebsd_parity
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
OPENSSL_DIR=/usr cargo build --release
```

## Reverting patch
To undo the patch run:
```shell
git reset --hard
```
