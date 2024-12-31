# Maintainer: Stefan Zipproth <s.zipproth@ditana.org>
# Author: Stefan Zipproth <s.zipproth@ditana.org>

pkgname=ditana-config-shell
pkgver=1.14
pkgrel=1
pkgdesc="Ditana common shell config"
arch=(any)
url="https://ditana.org"
license=('AGPL-3.0-or-later')
conflicts=()
depends=(
    ditana-print-system-infos
    exa
    inotify-tools
    pikaur
)
makedepends=()
source=("file://${PWD}/.shellrc" "file://${PWD}/readme.md")
sha256sums=('SKIP' 'SKIP')

package() {
    install -D -m644 "$srcdir/.shellrc" "$pkgdir/etc/skel/.shellrc"
    mkdir -p "$pkgdir/etc/skel/.shell.d"
    install -D -m644 "$srcdir/readme.md" "$pkgdir/etc/skel/.shell.d/readme.md"
}
