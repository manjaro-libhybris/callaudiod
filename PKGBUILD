# Maintainer: Philip Müller <philm@manjaro.org>

pkgname=callaudiod
pkgver=0.1.1
_commit=c9550e17e6e26802dda23c6070d100aca0cc2810
pkgrel=1
pkgdesc="Call audio routing daemon"
url="https://gitlab.com/mobian1/callaudiod"
license=('GPL')
arch=('x86_64' 'armv7h' 'aarch64')
depends=('pulseaudio' 'alsa-lib' 'glib2')
makedepends=('meson' 'git')
source=("git+https://gitlab.com/mobian1/${pkgname}.git#commit=${_commit}"
        https://gitlab.com/mobian1/callaudiod/-/merge_requests/10.patch)
md5sums=('SKIP'
         '20b4274c2058263ee3d7a7b8e0d8bfe2')

_reverts=(
)

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/^v//;s/-/+/g'
}

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
