# Maintainer: Tejun Heo <tj@kernel.org>

pkgname=scx-scheds
pkgver=0.1.1
pkgrel=1
pkgdesc='Sched_ext schedulers'
url='https://github.com/sched-ext/scx'
arch=('x86_64')
license=('GPL2.0')
depends=('glibc' 'libelf' 'zlib' 'libbpf')
makedepends=('meson' 'bpf' 'pahole' 'rust')
provides=()
source=(https://github.com/sched-ext/scx/archive/refs/tags/v${pkgver}.tar.gz)
sha512sums=('a917d09cdc4179d0376e26fdee9a5d300442c14e59ce07798785eb8e263978bc90046b6260ebd0d66ee4d0ba3915203683c17c52c29cfad8e9a404b7dde0bd7e')

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
