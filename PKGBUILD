# Maintainer: Jakob Gahde <j5lx@fmail.co.uk>

pkgname=ocaml-core_bench
pkgver=112.17.00
pkgrel=1
license=('unknown')
arch=('i686' 'x86_64')
pkgdesc="A micro-benchmarking library for ocaml"
url="https://github.com/janestreet/core_bench"
depends=('ocaml' 'ocaml-bin_prot' 'camlp4' 'ocaml-comparelib' 'ocaml-core' 'ocaml-fieldslib' 'ocaml-pa_ounit' 'ocaml-sexplib' 'ocaml-textutils')
makedepends=('ocaml-findlib')
source=("https://ocaml.janestreet.com/ocaml-core/$(echo ${pkgver} | grep -Po "^[0-9]+\.[0-9]+")/files/${pkgname#ocaml-}-${pkgver}.tar.gz")
options=('!strip')
md5sums=('d507304cc31692cf9ba4c7bdf3e9cc25')

build() {
  cd "${srcdir}/${pkgname#ocaml-}-${pkgver}"
  ./configure --prefix /usr
  make
}

package() {
  cd "${srcdir}/${pkgname#ocaml-}-${pkgver}"
  
  export OCAMLFIND_DESTDIR="${pkgdir}$(ocamlfind printconf destdir)"
  mkdir -p "$OCAMLFIND_DESTDIR/stublibs"
  make install
}
