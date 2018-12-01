pkgname=renderdoc
pkgver=1.2
pkgrel=1
pkgdesc="OpenGL and Vulkan debugging tool"
arch=(i686 x86_64)
url="https://github.com/baldurk/renderdoc"
license=("MIT")
makedepends=("cmake")
depends=("libx11" "libxcb" "mesa" "libgl" "qt5-base" "qt5-svg" "qt5-x11extras" "xcb-util-keysyms")
source=("https://github.com/baldurk/renderdoc/archive/v${pkgver}.tar.gz")
sha384sums=("9b16425f61f4d44678528c2963996f61bb50568ed8901f2826f7dc3b308eb95e6acf12b2ff4bbbd959685330eee40ec5")

build() {
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DBUILD_VERSION_STABLE=ON \
        -DBUILD_VERSION_DIST_CONTACT="https://aur.archlinux.org/packages/renderdoc" \
        -DBUILD_VERSION_DIST_NAME="Arch" \
        -DBUILD_VERSION_DIST_VER="${pkgver}" \
        -B"${srcdir}/${pkgname}-${pkgver}"/build \
        -H"${srcdir}/${pkgname}-${pkgver}"
  cmake --build "${srcdir}/${pkgname}-${pkgver}"/build
}

package() {
  make DESTDIR="${pkgdir}" -C "${srcdir}/${pkgname}-${pkgver}"/build install
  mkdir -p "${pkgdir}/usr/share/licenses/$pkgname"
  install -Dm644 "${srcdir}/${pkgname}-${pkgver}/LICENSE.md" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

# vim:set ts=2 sw=2 et:
