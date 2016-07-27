# $Id$
# Maintainer: Andreas Radke <andyrtr@archlinux.org>

pkgname=ijs
pkgver=0.35
pkgrel=1
pkgdesc="a library which implements a protocol for transmission of raster page images"
arch=('i686' 'x86_64')
url="http://www.openprinting.org/download/ijs/"
license=('GPL')
depends=('glibc' 'sh')
makedepends=('docbook-utils' 'ghostscript')
source=("http://www.openprinting.org/download/ijs/download/ijs-$pkgver.tar.bz2")
md5sums=('896fdcb7a01c586ba6eb81398ea3f6e9')

build() {
	cd "$pkgname-$pkgver"
	./configure --prefix=/usr \
          --disable-static \
          --enable-shared \
          --mandir=/usr/share/man
	make
}

package() {
	cd "$pkgname-$pkgver"
	make DESTDIR="$pkgdir/" install
        # install doc
        install -Dm644 ijs_spec.pdf ${pkgdir}/usr/share/doc/$pkgname/ijs_spec.pdf
}
