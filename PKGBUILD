# Maintainer: Philip Müller <philm@manjaro.org>

pkgname=callaudiod
pkgver=0.1.0+4+g84ed2eb
_commit=84ed2eb164711e73d30a88d540f59ddd8bc88b45
pkgrel=2
pkgdesc="Call audio routing daemon"
url="https://gitlab.com/mobian1/callaudiod"
license=('GPL')
arch=('x86_64' 'armv7h' 'aarch64')
depends=('pulseaudio' 'alsa-lib' 'glib2')
makedepends=('meson' 'git')
source=("git+https://gitlab.com/mobian1/${pkgname}.git#commit=${_commit}")
md5sums=('SKIP')

_reverts(
  811e01a80d369168fc2355d01d23f0edce114b52
)

prepare() {
    cd $pkgname
    
    for _c in "${_reverts[@]}"; do
      git log --oneline -1 "${_c}"
      git revert -n "${_c}"
    done
}

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
