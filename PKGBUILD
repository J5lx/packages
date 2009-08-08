# $Id$
# Maintainer: Tobias Powalowski <tpowa@archlinux.org>

pkgname=liblqr
pkgver=0.4.1
pkgrel=1
pkgdesc="A seam-carving C/C++ library called Liquid Rescale"
arch=('i686' 'x86_64')
url="http://liblqr.wikidot.com/"
license=('GPL')
depends=('glibc' 'glib2')
makedepends=('pkgconfig')
options=('!libtool')
source=(http://liblqr.wikidot.com/local--files/en:download-page/$pkgname-1-$pkgver.tar.bz2)

build() {
  cd "$srcdir/$pkgname-1-$pkgver"
  ./configure --prefix=/usr 
  make || return 1
  make DESTDIR="$pkgdir/" install || return 1
}
md5sums=('0e24ed3c9fcdcb111062640764d7b87a')
