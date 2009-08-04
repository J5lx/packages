# $Id$
# Maintainer: AndyRTR <andyrtr@archlinux.org>

pkgname=ghostscript
pkgver=8.70
pkgrel=1
pkgdesc="An interpreter for the PostScript language"
arch=(i686 x86_64)
license=('GPL' 'custom')
depends=('libxext' 'libxt' 'libcups>=1.3.11' 'fontconfig>=2.6.0' 'gnutls>=2.8.1' 'cairo>=1.8.8')
makedepends=('automake' 'autoconf' 'gtk2>=2.16.5')
optdepends=('texlive-core:	dvipdf'
            'gtk2:		gsx')
replaces=('ghostscript-lrpng')
provides=('ghostscript-lprng')
url="http://www.cs.wisc.edu/~ghost/"
source=(http://ghostscript.com/releases/ghostscript-${pkgver}.tar.bz2
	ghostscript-fPIC.patch)
options=('!libtool') # '!makeflags'
md5sums=('526366f8cb4fda0d3d293597cc5b984b'
         '1a8fcacf0005214db823225c870f093d')

build() {
  cd ${srcdir}/ghostscript-${pkgver}
  
  if [ "$CARCH" = "x86_64" ]; then
    patch -Np1 -i ${srcdir}/ghostscript-fPIC.patch || return 1
  fi

  # Build IJS
  cd ${srcdir}/ghostscript-${pkgver}/ijs
  ./autogen.sh
  ./configure --prefix=/usr --enable-shared --disable-static
  make || return 1
  make DESTDIR=${pkgdir} install || return 1

  cd ..
  ./configure --prefix=/usr --enable-dynamic --enable-threads --with-ijs \
              --with-jbig2dec --with-omni --with-x --with-drivers=ALL\
	      --with-fontpath=/usr/share/fonts/Type1:/usr/share/fonts
  make || return 1
  make DESTDIR=${pkgdir} \
	cups_serverroot=${pkgdir}/etc/cups \
	cups_serverbin=${pkgdir}/usr/lib/cups install soinstall

  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  install -m644 LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/

  # remove unwanted localized man-pages
  rm -rf $pkgdir/usr/share/man/[^man1]*
}
