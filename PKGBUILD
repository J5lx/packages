# $Id$
# Maintainer: AndyRTR <andyrtr@archlinux.org>

pkgname=ghostscript
pkgver=9.07
pkgrel=2
pkgdesc="An interpreter for the PostScript language"
arch=('i686' 'x86_64')
license=('AGPL' 'custom')
depends=('libxt' 'libcups' 'fontconfig' 'jasper' 'zlib' 'libpng>=1.5.7' 'libjpeg'
         'libtiff>=4.0.0' 'lcms' 'dbus')
makedepends=('gtk2' 'gnutls')
optdepends=('texlive-core:      needed for dvipdf'
            'gtk2:              needed for gsx')
url="http://www.ghostscript.com/"
source=(http://downloads.ghostscript.com/public/ghostscript-${pkgver}.tar.bz2)
options=('!libtool' '!makeflags')
md5sums=('eea27befc1e85bef6d4768202f6b03a5')

build() {
  cd ghostscript-${pkgver}
  
  # force it to use system-libs
  rm -rf jpeg libpng zlib jasper expat tiff lcms freetype 

  ./configure --prefix=/usr \
	--enable-dynamic \
	--with-ijs \
	--with-jbig2dec \
	--with-omni \
	--with-x \
	--with-drivers=ALL\
	--with-fontpath=/usr/share/fonts/Type1:/usr/share/fonts \
	--with-install-cups \
	--enable-fontconfig \
	--enable-freetype \
	--without-luratech \
	--without-omni \
	--with-system-libtiff \
	--disable-compile-inits #--help # needed for linking with system-zlib
  make

  # Build IJS
  cd ijs
  sed -i "s:AM_PROG_CC_STDC:AC_PROG_CC:g" configure.ac
  ./autogen.sh
  ./configure --prefix=/usr --enable-shared --disable-static
  make
}

package() {
  cd ghostscript-${pkgver}
  make DESTDIR="${pkgdir}" \
	cups_serverroot="${pkgdir}"/etc/cups \
	cups_serverbin="${pkgdir}"/usr/lib/cups install soinstall

  # install missing doc files # http://bugs.archlinux.org/task/18023
  install -m 644 "${srcdir}"/ghostscript-${pkgver}/doc/{Ps2ps2.htm,gs-vms.hlp,gsdoc.el,pscet_status.txt} "${pkgdir}"/usr/share/ghostscript/$pkgver/doc/
  
  install -D -m644 LICENSE "${pkgdir}"/usr/share/licenses/${pkgname}/LICENSE

  # remove unwanted localized man-pages
  rm -rf "$pkgdir"/usr/share/man/[^man1]*

  # install IJS
  cd ijs
  make DESTDIR="${pkgdir}" install
}
