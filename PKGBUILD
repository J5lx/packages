# $Id$
# Maintainer: Jaroslav Lichtblau <dragonlord@aur.archlinux.org>
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=directfb
pkgver=1.6.2
pkgrel=1
pkgdesc="A thin library that provides hardware graphics acceleration, input device handling and abstraction, integrated windowing system on top of the Linux Framebuffer Device"
arch=('i686' 'x86_64')
url="http://www.directfb.org"
license=('LGPL')
depends=('gcc-libs' 'libjpeg' 'sdl' 'libpng' 'freetype2')
options=('!libtool')
source=(http://www.directfb.org/downloads/Core/DirectFB-${pkgver%.*}/DirectFB-${pkgver}.tar.gz)
sha1sums=('a393a53130e6a80369b7081371cd23dd1cd5e192')

build() {
  cd "${srcdir}/DirectFB-${pkgver}"
  ./configure --prefix=/usr --sysconfdir=/etc --enable-static \
    --enable-zlib --enable-x11 --enable-sdl --disable-vnc --disable-osx \
    --enable-video4linux2 --enable-voodoo
  make
} 

package() {
  cd "${srcdir}/DirectFB-${pkgver}"
  make DESTDIR="${pkgdir}" install
}
