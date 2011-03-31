# $Id$
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgbase=imagemagick
pkgname=('imagemagick' 'imagemagick-doc')
pkgver=6.6.9.1
pkgrel=1
arch=('i686' 'x86_64')
url="http://www.imagemagick.org/"
license=('custom')
depends=('libtool' 'lcms' 'libxt' 'gcc-libs' 'bzip2' 'xz' 'freetype2' 'fontconfig' \
         'libxext' 'libjpeg-turbo')
makedepends=('ghostscript' 'openexr' 'libwmf' 'librsvg' 'libxml2' 'jasper' 'libpng')
source=(ftp://ftp.imagemagick.org/pub/ImageMagick/ImageMagick-${pkgver%.*}-${pkgver##*.}.tar.xz \
        libpng_mmx_patch_x86_64.patch perlmagick.rpath.patch)
md5sums=('b8c7dfbad2eb9b9c39666af07cb6db83'
         '069980fc2590c02aed86420996259302'
         'ff9974decbfe9846f8e347239d87e4eb')
sha1sums=('924e1841a1518bf394f5a18d4fd25414bdf9f63d'
          'e42f3acbe85b6098af75c5cecc9a254baaa0482c'
          '23405f80904b1de94ebd7bd6fe2a332471b8c283')


build() {
  cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}

  if [ "${CARCH}" = 'x86_64' ]; then
    patch -Np1 < ../libpng_mmx_patch_x86_64.patch
  fi

  patch -p0 < ../perlmagick.rpath.patch
  ./configure --prefix=/usr --sysconfdir=/etc --with-modules --disable-static \
    --enable-openmp --with-x --with-wmf --with-openexr --with-xml \
    --with-gslib --with-gs-font-dir=/usr/share/fonts/Type1 \
    --with-perl --with-perl-options="INSTALLDIRS=vendor" \
    --without-gvc --without-djvu --without-autotrace --with-jp2 \
    --without-jbig --without-fpx --without-dps --without-fftw
  make
}

package_imagemagick() {
  pkgdesc="An image viewing/manipulation program"
  optdepends=('ghostscript: for Ghostscript support' 
              'openexr: for OpenEXR support' 
              'libwmf: for WMF support' 
              'librsvg: for SVG support' 
              'libxml2: for XML support' 
              'jasper: for JPEG-2000 support' 
              'libpng: for PNG support')
  backup=('etc/ImageMagick/colors.xml' 'etc/ImageMagick/log.xml' 'etc/ImageMagick/policy.xml'
          'etc/ImageMagick/type-dejavu.xml' 'etc/ImageMagick/type-ghostscript.xml'
          'etc/ImageMagick/type-windows.xml' 'etc/ImageMagick/type.xml')
  options=('!makeflags' '!docs')

  cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}
  make DESTDIR="${pkgdir}" install
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/imagemagick/LICENSE"
  install -Dm644 NOTICE "${pkgdir}/usr/share/licenses/imagemagick/NOTICE"

#Cleaning
  find "${pkgdir}" -name '*.bs' -exec rm {} \;
  rm -f "${pkgdir}"/usr/lib/*.la
}

package_imagemagick-doc() {
  pkgdesc="The ImageMagick documentation (utilities manuals and libraries API)"
  depends=()
  options=('!makeflags')

  cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}
  make DESTDIR="${pkgdir}" install-data-html
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/imagemagick-doc/LICENSE"
  install -Dm644 NOTICE "${pkgdir}/usr/share/licenses/imagemagick-doc/NOTICE"
}
