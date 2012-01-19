# $Id$
# Maintainer: AndyRTR <andyrtr@archlinux.org>

pkgname=ghostscript
pkgver=9.04
pkgrel=6
pkgdesc="An interpreter for the PostScript language"
arch=('i686' 'x86_64')
license=('GPL3' 'custom')
depends=('libxt' 'libcups' 'fontconfig' 'jasper' 'zlib' 'libpng>=1.5.7' 'libjpeg' 'libtiff>=4.0.0' 'lcms') # 'lcms2' won't get used) # move in libpaper from community?
makedepends=('gtk2' 'gnutls')
optdepends=('texlive-core:      needed for dvipdf'
            'gtk2:              needed for gsx')
url="http://www.ghostscript.com/"
source=(http://downloads.ghostscript.com/public/ghostscript-${pkgver}.tar.bz2
	ghostscript-cups-rgbw.patch
	ghostscript-gpl-9.04-freetype-underlinking.patch)
options=('!libtool' '!makeflags')
md5sums=('9f6899e821ab6d78ab2c856f10fa3023'
         'bc56eb8c5fef0ecf964f6b3e9b7e65ae'
         'a1928c3e4459dcfee0aaa4b38fadba57')

build() {
  cd ${srcdir}/ghostscript-${pkgver}
  
  # fix broken color printing https://bugs.archlinux.org/task/25519
  patch -Np1 -i ${srcdir}/ghostscript-cups-rgbw.patch
  # fix a linking issue
  patch -Np1 -i ${srcdir}/ghostscript-gpl-9.04-freetype-underlinking.patch
  
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
	--disable-compile-inits #--help # needed for linking with system-zlib
  make

  # Build IJS
  cd ${srcdir}/ghostscript-${pkgver}/ijs
  ./autogen.sh
  ./configure --prefix=/usr --enable-shared --disable-static
  make
}

package() {
  cd ${srcdir}/ghostscript-${pkgver}
  make DESTDIR=${pkgdir} \
	cups_serverroot=${pkgdir}/etc/cups \
	cups_serverbin=${pkgdir}/usr/lib/cups install soinstall

  # install missing doc files # http://bugs.archlinux.org/task/18023
  install -m 644 ${srcdir}/ghostscript-${pkgver}/doc/{Ps2ps2.htm,gs-vms.hlp,gsdoc.el,pscet_status.txt} ${pkgdir}/usr/share/ghostscript/$pkgver/doc/
  
  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  install -m644 LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/

  # remove unwanted localized man-pages
  rm -rf $pkgdir/usr/share/man/[^man1]*

  # install IJS
  cd ${srcdir}/ghostscript-${pkgver}/ijs
  make DESTDIR=${pkgdir} install
}
