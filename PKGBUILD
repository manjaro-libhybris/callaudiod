# Maintainer: Philip Müller <philm@manjaro.org>

pkgname=callaudiod
pkgver=0.0.4
_commit=5885e981b9acba474e277b37e3f912e61466a03e
pkgrel=1
pkgdesc="Call audio routing daemon"
url="https://gitlab.com/mobian1/callaudiod"
license=('GPL')
arch=('x86_64' 'armv7h' 'aarch64')
depends=('pulseaudio' 'alsa-lib' 'glib2')
makedepends=('meson')
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
}

build() {
    arch-meson $pkgname build
    ninja -C build
}

package() {
    DESTDIR="$pkgdir" ninja -C build install
}
