# Maintainer: drakkan <nicola.murino at gmail dot com>
pkgname=mingw-w64-gstreamer
pkgver=1.14.0
pkgrel=1
pkgdesc="GStreamer Multimedia Framework (mingw-w64)"
arch=(any)
url="http://gstreamer.freedesktop.org/"
license=('LGPL')
makedepends=('mingw-w64-configure' 'bison' 'flex')
depends=('mingw-w64-glib2' 'mingw-w64-libxml2')
options=('!strip' '!buildflags' 'staticlibs')

source=(${url}/src/gstreamer/gstreamer-${pkgver}.tar.xz)
sha256sums=('fc361367f0d4b780a868a8833f9f30b9c9f4ac9faea4e6b251db8b4b0398466e')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"


build() {
  cd "${srcdir}/gstreamer-${pkgver}"
  for _arch in $_architectures; do
    mkdir -p "build-${_arch}"
    cd "build-${_arch}"
    ${_arch}-configure \
      --with-package-name="GStreamer (Arch Linux)" \
      --with-package-origin="http://www.archlinux.org/" \
      --disable-examples \
      --disable-benchmarks \
      --disable-tests

    # https://bugzilla.gnome.org/show_bug.cgi?id=655517
    sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

    make
    cd ..
  done
}


package() {
  cd "${srcdir}/gstreamer-${pkgver}"

  for _arch in ${_architectures}; do
    cd "build-${_arch}"
    make DESTDIR="${pkgdir}" install

    rm $pkgdir/usr/$_arch/lib/gstreamer-1.0/*.a
    rm $pkgdir/usr/$_arch/lib/gstreamer-1.0/*.la
    rm -rf "$pkgdir"/usr/${_arch}/share/{aclocal,man,locale,bash-completion}

    find "$pkgdir" -name '*.dll' -exec ${_arch}-strip --strip-unneeded {} \;
    find "$pkgdir" -name '*.dll' -o -name '*.a' -exec ${_arch}-strip -g {} \;
    find "$pkgdir" -name '*.exe' -exec ${_arch}-strip --strip-all {} \;

    cd ..
  done
}

# vim: ts=2 sw=2 et:
