# Maintainer: Luka Panio <lukapanio@gmail.com>

pkgname=callaudiod-hybris
pkgver=0.1.0
_commit=57cfcb397a6f3e6e2603eb946871bb9ca391343f
pkgrel=2
pkgdesc="Call audio routing daemon"
url="https://gitlab.com/mobian1/callaudiod"
license=('GPL')
arch=('x86_64' 'armv7h' 'aarch64')
depends=('pulseaudio' 'alsa-lib' 'glib2')
makedepends=('meson' 'git')
provides=('callaudiod=0.1.0')
conflicts=('callaudiod')
source=("callaudiod-hybris::git+https://github.com/droidian/callaudiod#commit=${_commit}")
md5sums=('SKIP')

_reverts=(
)

prepare() {
  cd "$srcdir/$pkgname"

  for _c in "${_reverts[@]}"; do
      git log --oneline -1 "${_c}"
      git revert -n "${_c}"
  done

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
