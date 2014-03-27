# $Id$
# Maintainer: AndyRTR <andyrtr@archlinux.org>

### !!! rebuild groff from core that picks up hardcoding the GS versioned font path !!! ###

pkgname=ghostscript
pkgver=9.14
pkgrel=1
pkgdesc="An interpreter for the PostScript language"
arch=('i686' 'x86_64')
license=('AGPL' 'custom')
depends=('libxt' 'libcups' 'fontconfig' 'jasper' 'zlib' 'libpng>=1.5.7' 'libjpeg'
         'libtiff>=4.0.0' 'lcms2' 'dbus' 'libpaper')
makedepends=('gtk3' 'gnutls')
optdepends=('texlive-core:      needed for dvipdf'
            'gtk3:              needed for gsx')
url="http://www.ghostscript.com/"
source=(http://downloads.ghostscript.com/public/ghostscript-${pkgver}.tar.bz2
        ghostscript-sys-zlib.patch)
#options=('!makeflags')
sha1sums=('eab1c9e9850d8aedf02d16f3f7f8198ad9384068'
          'e054caf753df4d67221b29a2eac66130653f7556')

prepare() {
  cd ghostscript-${pkgver}
  # fix build with system zlib
  patch -Np1 -i ${srcdir}/ghostscript-sys-zlib.patch
}

build() {
  cd ghostscript-${pkgver}
  
  # force it to use system-libs
  # keep heavily patched included openjpeg, leads to segfault with system openjpeg
  # https://bugs.archlinux.org/task/38226
  rm -rf jpeg libpng zlib jasper expat tiff lcms lcms2 freetype cups/libs # jbig2dec is in community

  autoconf --force

  ./configure --prefix=/usr \
	--enable-dynamic \
	--with-ijs \
	--with-jbig2dec \
	--with-omni \
	--with-x \
	--with-drivers=ALL\
	--with-fontpath=/usr/share/fonts/Type1:/usr/share/fonts \
	--enable-fontconfig \
	--enable-freetype \
	--enable-openjpeg \
	--without-luratech \
	--without-omni \
	--with-system-libtiff \
	--with-libpaper \
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
	cups_serverbin="${pkgdir}"/usr/lib/cups install install-so

  # install missing doc files # http://bugs.archlinux.org/task/18023
  install -m 644 "${srcdir}"/ghostscript-${pkgver}/doc/{Ps2ps2.htm,gs-vms.hlp,gsdoc.el,pscet_status.txt} "${pkgdir}"/usr/share/ghostscript/$pkgver/doc/
  
  install -D -m644 LICENSE "${pkgdir}"/usr/share/licenses/${pkgname}/LICENSE

  # remove unwanted localized man-pages
  rm -rf "$pkgdir"/usr/share/man/[^man1]*

  # install IJS
  cd ijs
  make DESTDIR="${pkgdir}" install
  
  # remove filters that are now maintained in cups-filters as upstream home
  rm -rf "$pkgdir"/usr/lib/cups/filter/{gstopxl,gstoraster}
}
