# Immudb-bin
This is a package for installing [Immudb](https://immudb.io/) on Arch Linux based systems (like [Manjaro](https://manjaro.org/)).
The `-bin` suffix indicates that this package isn't built from source. Instead it's packaging pre-built binaries from [Immudb's releases page](https://github.com/codenotary/immudb/releases).

## Building

```sh
# if you've updated the version
task update
# make the package
task build
```
## Verifying
Check what ended up in the package with pacman:
```sh
pacman -Qlp immudb-1.9.3-1-x86_64.pkg.tar.zst
```

## Install
Install the locally built package with pacman:
```sh
sudo pacman -U immudb-1.9.3-1-x86_64.pkg.tar.zst
```

## Thanks
This package borrows heavily from [George Rawlinson's AUR package](https://aur.archlinux.org/packages/immudb).
