# Maintainer: Philip Müller <philm@manjaro.org>

pkgname=callaudiod
pkgver=0.0.4
_commit=6c8ffc22cbaf68f29f9eefc27226978493f9bde2
pkgrel=1
pkgdesc="Call audio routing daemon"
url="https://gitlab.com/mobian1/callaudiod"
license=('GPL')
arch=('x86_64' 'armv7h' 'aarch64')
depends=('pulseaudio' 'alsa-lib' 'glib2')
makedepends=('meson' 'git')
source=("git+https://gitlab.com/mobian1/${pkgname}.git#commit=${_commit}")
md5sums=('SKIP')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/^v//;s/-/+/g'
}

prepare() {
  cd "$srcdir/$pkgname"

  local src
  for src in "${source[@]}"; do
      src="${src%%::*}"
      src="${src##*/}"
      [[ $src = *.patch ]] || continue
      msg2 "Applying patch: $src..."
      patch -Np1 < "../$src"
  done

  sed -i -e 's|#include "callaudiod.h"|#include "../src/callaudiod.h"|g' libcallaudio/libcallaudio.c
}

build() {
    arch-meson $pkgname build
    ninja -C build
}

package() {
    DESTDIR="$pkgdir" ninja -C build install
}
