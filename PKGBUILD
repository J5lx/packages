# Maintainer: Doug Newgard <scimmia22 at outlook dot com>

_pkgname=espionage
pkgname=$_pkgname-git
pkgver=0.9.r13.04c9800
pkgrel=1
pkgdesc="PyEFL D-Bus inspector"
arch=('any')
url="http://www.enlightenment.org"
license=('GPL3')
depends=('python-efl' 'gtk-update-icon-cache')
makedepends=('git')
provides=("$_pkgname=$pkgver")
conflicts=("$_pkgname")
install=$_pkgname.install
source=("git://git.enlightenment.org/apps/$_pkgname.git")
sha256sums=('SKIP')

pkgver() {
  cd "$srcdir/$_pkgname"

  printf "$(python setup.py -V).r$(git rev-list --count HEAD).$(git rev-parse --short HEAD)"
}

package() {
  cd "$srcdir/$_pkgname"

  python setup.py install --root="$pkgdir"

# install text files
  install -Dm644 ChangeLog "$pkgdir/usr/share/doc/$_pkgname/ChangeLog"
  install -Dm644 README "$pkgdir/usr/share/doc/$_pkgname/README"
}
