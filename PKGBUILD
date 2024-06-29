# Maintainer: Mike Williamson <mike@korora.ca>
# much inspiration from George Rawlinson <grawlinson@archlinux.org>

pkgname=immudb
pkgver=1.9.3
pkgrel=1
pkgdesc='Immutable database built on a zero-trust model'
arch=('x86_64')
url='https://codenotary.com/technologies/immudb/'
# https://github.com/codenotary/immudb/blob/master/LICENSE :(
license=('BSL')
backup=('etc/immudb/immudb.toml')
source=(
  "https://github.com/codenotary/immudb/releases/download/v$pkgver/immudb-v$pkgver-linux-amd64"
  "https://github.com/codenotary/immudb/releases/download/v$pkgver/immuadmin-v$pkgver-linux-amd64"
  "https://github.com/codenotary/immudb/releases/download/v$pkgver/immuclient-v$pkgver-linux-amd64"
  'systemd.service'
  'sysusers.conf'
  'tmpfiles.conf'
  'config.toml'
)
sha256sums=('33babff774077ec239972071016d357def6dd9d49b0a77b6cb9f3845b9a37f9d'
            'e7366ec7187820021596e837c81065120b539f1fa36b8c3c28c873514ea38ac2'
            '55936da403d049c668d46d9629d3b6c7b9b9f0bb0c35f0bfb711ff4ec001623f'
            'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855'
            '42b18385820aefb0967229def5535172e635cbf26569bd2e36f3bc0af8e7402e'
            'bc98faaa41be644024856893d66a50bafb9a867d055437341ffecd77d5d5c1c3'
            '2c9f308542ab2c9c204600d0fbabbf1c3181bffabcbfb55ce5e6b2f5d0af26d4')

package() {
  cd "$pkgname"

  # create systemd service with correct link to documentation
  sed "s/@@PKGVER@@/$pkgver/" systemd.service > systemd.service
  install -Dm644 systemd.service $pkgdir/usr/lib/systemd/system/immudb.service
  # download dependencies
  # binaries
  install -vDm755 -t "$pkgdir/usr/bin" output/{immudb,immuclient,immuadmin}

  # shell completion - bash
  install -vDm644 output/bash/* -t "$pkgdir/usr/share/bash-completion/completions"

  # shell completion - fish
  for completion in immudb immuclient immuadmin; do
    install -vDm644 "output/fish/$completion" "$pkgdir/usr/share/fish/vendor_completions.d/$completion.fish"
  done

  # shell completion - zsh
  for completion in immudb immuclient immuadmin; do
    install -vDm644 "output/zsh/$completion" "$pkgdir/usr/share/zsh/site-functions/_$completion"
  done

  # man pages
  install -vDm644 tools/packaging/deb/man/immu{client,db}.1 -t "$pkgdir/usr/share/man/man1"

  # systemd integration
  install -vDm644 output/systemd.service "$pkgdir/usr/lib/systemd/system/$pkgname.service"
  install -vDm644 ../sysusers.conf "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"
  install -vDm644 ../tmpfiles.conf "$pkgdir/usr/lib/tmpfiles.d/$pkgname.conf"

  # configuration
  install -vDm644 ../config.toml "$pkgdir/etc/$pkgname/$pkgname.toml"
}
