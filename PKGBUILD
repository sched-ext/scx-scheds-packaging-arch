# Maintainer: Tejun Heo <tj@kernel.org>

pkgname=scx-scheds
pkgver=0.1.0
pkgrel=1
pkgdesc='Sched_ext schedulers'
url='https://github.com/sched-ext/scx'
arch=('x86_64')
license=('GPL2.0')
depends=('glibc' 'libelf' 'zlib' 'libbpf')
makedepends=('meson' 'bpf' 'pahole' 'rust')
provides=()
source=(https://github.com/sched-ext/scx/archive/refs/tags/v${pkgver}.tar.gz)
sha512sums=('2809e15a8ede9984c52b4c7d7c3d98499f67c76df447c5d49d034d233ccfa84f33273d2c917ce793b6782517436926d7bd81d21f5bdf4996380e50482bb303fb')

build() {
  cd scx-${pkgver}
  meson setup build --prefix "${pkgdir}/usr" -Dbuildtype=release
  cd build
  meson compile
}

package() {
  cd scx-${pkgver}/build
  meson install
}

# vim: ts=2 sw=2 et:
