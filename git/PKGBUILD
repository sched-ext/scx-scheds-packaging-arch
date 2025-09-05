# Maintainer: Peter Jung ptr1337 <admin@ptr1337.dev>
# Maintainer: Piotr Górski <lucjan.lucjanov@gmail.com>

pkgname=scx-scheds-git
_gitname=scx
pkgver=1.0.15.r305.g7a1f079b
pkgrel=2
pkgdesc='sched_ext schedulers and tools'
url='https://github.com/sched-ext/scx'
arch=('x86_64' 'aarch64')
backup=('etc/default/scx')
license=('GPL-2.0-only')
depends=(
  bash
  bpf
  gcc-libs
  glibc
  jq
  libseccomp
  libbpf
  libelf
  protobuf
  zlib
)
makedepends=(
  clang
  git
  llvm
  llvm-libs
  python
  rust
)
source=("git+https://github.com/sched-ext/scx")
sha256sums=('SKIP')
options=(!lto)
provides=("scx-scheds=$pkgver")
conflicts=("scx-scheds")

pkgver() {
  cd $_gitname
  git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
 cd $_gitname
 cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
  cd $_gitname
  export CARGO_TARGET_DIR=target
  cargo build --release --locked --frozen
}

check() {
  cd $_gitname
  cargo test --frozen --locked
}

package() {
  cd $_gitname

  # binary files
  find target/release -maxdepth 1 -type f -executable ! -name '*.so' \
    -exec install -Dm755 -t "$pkgdir/usr/bin/" {} +

  # systemd services
  install -Dm644 services/systemd/scx_loader.service \
    "$pkgdir/usr/lib/systemd/system/scx_loader.service"

  install -Dm644 services/systemd/scx.service \
    "$pkgdir/usr/lib/systemd/system/scx.service"

  # dbus services
  install -Dm644 services/systemd/org.scx.Loader.service\
    "$pkgdir/usr/share/dbus-1/system-services/org.scx.Loader.service"

  # dbus config
  install -Dm644 tools/scx_loader/org.scx.Loader.conf \
    "$pkgdir/usr/share/dbus-1/system.d/org.scx.Loader.conf"

  # scx_loader config
  install -Dm644 services/scx_loader.toml \
    "$pkgdir/usr/share/scx_loader/config.toml"

  # scx config
  install -Dm644 services/scx \
    "$pkgdir/etc/default/scx"
}
