# Maintainer: Peter Jung ptr1337 <admin@ptr1337.dev>
# Maintainer: Piotr Górski <lucjan.lucjanov@gmail.com>

pkgname=scx-scheds-git
_gitname=scx
pkgver=1.0.15.r305.g7a1f079b
pkgrel=1
pkgdesc='sched_ext schedulers and tools'
url='https://github.com/sched-ext/scx'
arch=('x86_64' 'aarch64')
backup=('etc/default/scx')
license=('GPL-2.0-only')
depends=(
  bash
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

_backports=(
)

_reverts=(
)

pkgver() {
  cd $_gitname
  git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
 cd $_gitname

 local _c _l
  for _c in "${_backports[@]}"; do
    if [[ "${_c}" == *..* ]]; then _l='--reverse'; else _l='--max-count=1'; fi
    git log --oneline "${_l}" "${_c}"
    git cherry-pick --mainline 1 --no-commit "${_c}"
  done
  for _c in "${_reverts[@]}"; do
    if [[ "${_c}" == *..* ]]; then _l='--reverse'; else _l='--max-count=1'; fi
    git log --oneline "${_l}" "${_c}"
    git revert --mainline 1 --no-commit "${_c}"
  done

  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.patch ]] || continue
    echo "Applying patch $src..."
    patch -Np1 < "../$src"
  done
}

build() {
  cd $_gitname
  cargo build \
     --release \
     --workspace \
     --locked \
     --frozen \
     --exclude vmlinux_docify \
     --exclude scx_lib_selftests
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
