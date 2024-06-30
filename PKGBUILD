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
            'e288ecbffb4cdac7bf713e2e2bb29094756f72c2194bd0741095b280d534a2d5'
            '42b18385820aefb0967229def5535172e635cbf26569bd2e36f3bc0af8e7402e'
            'bc98faaa41be644024856893d66a50bafb9a867d055437341ffecd77d5d5c1c3'
            '2c9f308542ab2c9c204600d0fbabbf1c3181bffabcbfb55ce5e6b2f5d0af26d4')

package() {

  # create systemd service with correct link to documentation
  mkdir -p $pkgdir/usr/lib/systemd/system
  sed -i "s/@@PKGVER@@/$pkgver/" systemd.service > $pkgdir/usr/lib/systemd/system/immudb.service
  echo "doing binaries"

  # add execute permission so we can use the binaries to generate completions
  for bin in immudb immuclient immuadmin; do
    chmod +x "$bin-v$pkgver-linux-amd64"
  done

  # Generate bash completion
  for bin in immudb immuclient immuadmin; do
    echo "generating bash completion"
    mkdir -p "$pkgdir/usr/share/bash-completion/completions"
    "./$bin-v$pkgver-linux-amd64" completion bash > "$pkgdir/usr/share/bash-completion/completions/$bin"
    echo "finished bash"
  done

  for bin in immudb immuclient immuadmin; do
    echo "generating fish completion"
    mkdir -p "$pkgdir/usr/share/fish/vendor_completions.d"
    "./$bin-v$pkgver-linux-amd64" completion fish > "$pkgdir/usr/share/fish/vendor_completions.d/$bin.fish"
    echo "finished fish"
  done

  for bin in immudb immuclient immuadmin; do
    echo "generating zsh completion"
    mkdir -p "$pkgdir/usr/share/zsh/site-functions"
    "./$bin-v$pkgver-linux-amd64" completion zsh > "$pkgdir/usr/share/zsh/site-functions/_$bin"
    echo "finished zsh"
  done

  echo "done completions."
  echo "moving binaries"
  for bin in immudb immuclient immuadmin; do
    mv "$bin-v$pkgver-linux-amd64" $bin
    install -vDm755 "$bin" "$pkgdir/usr/bin/$bin"
  done

  # systemd integration
  install -vDm644 systemd.service "$pkgdir/usr/lib/systemd/system/$pkgname.service"
  install -vDm644 sysusers.conf "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"
  install -vDm644 tmpfiles.conf "$pkgdir/usr/lib/tmpfiles.d/$pkgname.conf"

  # configuration
  install -vDm644 config.toml "$pkgdir/etc/$pkgname/$pkgname.toml"
}
