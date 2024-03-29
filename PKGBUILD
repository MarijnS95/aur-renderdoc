# Maintainer: Eric Engestrom <aur [at] engestrom [dot] ch>
# Contributor: Hubert Jarosz <marqin.pl+aur at gmail dot com> PGP: 0xFFECF63C1AAB83FF

pkgname=renderdoc-git
pkgver=1.22
pkgrel=1
pkgdesc="OpenGL and Vulkan debugging tool"
arch=(i686 x86_64)
url="https://github.com/baldurk/renderdoc"
license=("MIT")
makedepends=("git" "cmake" "ninja" "python")
depends=("wayland" "libx11" "libxcb" "mesa" "libgl" "qt5-base" "qt5-svg" "qt5-x11extras" "xcb-util-keysyms")
source=("git+https://github.com/baldurk/renderdoc")
sha256sums=('SKIP')
conflicts=('renderdoc')
provides=("renderdoc=${pkgver%+*}")

pkgver() {
  cd "${srcdir}"/renderdoc
  git describe --always --abbrev=10 HEAD |
    sed 's/^v//; s/-/+/; s/-/./'
}

build() {
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DBUILD_VERSION_DIST_CONTACT="https://aur.archlinux.org/packages/renderdoc-git" \
        -DBUILD_VERSION_DIST_NAME="Arch" \
        -DBUILD_VERSION_DIST_VER="${pkgver}" \
        -D ENABLE_WAYLAND=true \
        -S renderdoc -B build \
        -G Ninja
  cmake --build build

  export JAVA_HOME=/usr/lib/jvm/default
  # Unset host flags
  export CXXFLAGS=
  export CFLAGS=

  for arch in arm64-v8a armeabi-v7a; do
      cmake -DCMAKE_BUILD_TYPE=Release \
            -DCMAKE_INSTALL_PREFIX=/usr \
            -DBUILD_VERSION_DIST_CONTACT="https://aur.archlinux.org/packages/renderdoc-git" \
            -DBUILD_VERSION_DIST_NAME="Arch" \
            -DBUILD_VERSION_DIST_VER="${pkgver}" \
            -DBUILD_ANDROID=1 \
            -DANDROID_ABI=$arch \
            -S renderdoc -B build-android-$arch \
            -G Ninja
      cmake --build build-android-$arch
    done
}

package() {
  mkdir -p "${pkgdir}/usr/share/renderdoc/plugins/android"
  cp "${srcdir}"/build-android-arm64-v8a/renderdoccmd/RenderDocCmd.apk "${pkgdir}/usr/share/renderdoc/plugins/android/org.renderdoc.renderdoccmd.arm64.apk"
  cp "${srcdir}"/build-android-armeabi-v7a/renderdoccmd/RenderDocCmd.apk "${pkgdir}/usr/share/renderdoc/plugins/android/org.renderdoc.renderdoccmd.arm32.apk"

  DESTDIR="$pkgdir" ninja -C build install

  install -Dm644 renderdoc/LICENSE.md "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
