# Maintainer: Mike Williamson <mike@korora.ca>
# much inspiration from George Rawlinson <grawlinson@archlinux.org>

pkgname=immudb-bin
pkgver=1.9.5
pkgrel=1
pkgdesc='Immutable database built on a zero-trust model'
arch=('x86_64')
url='https://immudb.io'
# https://github.com/codenotary/immudb/blob/master/LICENSE :(
license=('BUSL-1.1')
backup=('etc/immudb/immudb.toml')
install=immudb.install
provides=("immudb=$pkgver")
conflicts=("immudb")
source=(
  "https://github.com/codenotary/immudb/releases/download/v$pkgver/immudb-v$pkgver-linux-amd64"
  "https://github.com/codenotary/immudb/releases/download/v$pkgver/immuadmin-v$pkgver-linux-amd64"
  "https://github.com/codenotary/immudb/releases/download/v$pkgver/immuclient-v$pkgver-linux-amd64"
  'systemd.service'
  'sysusers.conf'
  'tmpfiles.conf'
  'immudb.toml'
)
sha256sums=('9e79a23676c4841fc4c324ae2577c777c91020647b01458ef9897154027d2922'
            '33af8a8f4add8ec21246bb7fa3b9abf32fe8520a2254a317e848dd759298f6cb'
            'f64e7a64c6d3ddc9ed42395b3edaed5956eb48bd689d6df120d351396a5f6e90'
            'e288ecbffb4cdac7bf713e2e2bb29094756f72c2194bd0741095b280d534a2d5'
            '42b18385820aefb0967229def5535172e635cbf26569bd2e36f3bc0af8e7402e'
            'bc98faaa41be644024856893d66a50bafb9a867d055437341ffecd77d5d5c1c3'
            '2c9f308542ab2c9c204600d0fbabbf1c3181bffabcbfb55ce5e6b2f5d0af26d4')

package() {

  # create systemd service with correct link to documentation
  mkdir -p $pkgdir/usr/lib/systemd/system
  sed -i "s/@@PKGVER@@/$pkgver/" systemd.service > $pkgdir/usr/lib/systemd/system/immudb.service

  # add execute permission so we can use the binaries to generate completions
  for bin in immudb immuclient immuadmin; do
    chmod +x "$bin-v$pkgver-linux-amd64"
  done

  # Generate bash completion
  for bin in immudb immuclient immuadmin; do
    mkdir -p "$pkgdir/usr/share/bash-completion/completions"
    "./$bin-v$pkgver-linux-amd64" completion bash > "$pkgdir/usr/share/bash-completion/completions/$bin"
  done

  for bin in immudb immuclient immuadmin; do
    mkdir -p "$pkgdir/usr/share/fish/vendor_completions.d"
    "./$bin-v$pkgver-linux-amd64" completion fish > "$pkgdir/usr/share/fish/vendor_completions.d/$bin.fish"
  done

  for bin in immudb immuclient immuadmin; do
    mkdir -p "$pkgdir/usr/share/zsh/site-functions"
    "./$bin-v$pkgver-linux-amd64" completion zsh > "$pkgdir/usr/share/zsh/site-functions/_$bin"
  done

  for bin in immudb immuclient immuadmin; do
    mkdir -p "$pkgdir/usr/share/man/man1"
    # use the hidden mangen command to generate man files:
    "./$bin-v$pkgver-linux-amd64" mangen "$pkgdir/usr/share/man/man1"
  done

  for bin in immudb immuclient immuadmin; do
    mv "$bin-v$pkgver-linux-amd64" $bin
    install -Dm755 "$bin" "$pkgdir/usr/bin/$bin"
  done

  # systemd integration
  install -Dm644 systemd.service "$pkgdir/usr/lib/systemd/system/immudb.service"
  install -Dm644 sysusers.conf "$pkgdir/usr/lib/sysusers.d/immudb.conf"
  install -Dm644 tmpfiles.conf "$pkgdir/usr/lib/tmpfiles.d/immudb.conf"

  # configuration
  install -Dm644 immudb.toml "$pkgdir/etc/immudb/immudb.toml"
}
