# Maintainer Tyler Kaminski <durcor at disroot dot org>

pkgname=rocm-opencl-runtime-git
_pkgname="${pkgname%-git}"
pkgver=3.6Beta.r11.gc3b7a21
pkgrel=1
pkgdesc='Radeon Open Compute - OpenCL runtime'
arch=('x86_64')
url='https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime'
license=('MIT')
depends=('hsakmt-roct' 'hsa-rocr' 'rocclr' 'opencl-icd-loader')
makedepends=('cmake' 'rocm-cmake')
conflicts=("$_pkgname")
provides=("$pkgname" 'opencl-driver')
source=("$pkgname::git+$url#branch=develop")
sha256sums=('SKIP')

pkgver() {
    cd "$pkgname"
    git describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
    CFLAGS="$CFLAGS -isystem /opt/rocm/rocclr/include/include -isystem /opt/rocm/rocclr/include/compiler/lib -isystem /opt/rocm/rocclr/include/compiler/lib/include" \
    CXXFLAGS="$CXXFLAGS -isystem /opt/rocm/rocclr/include/include -isystem /opt/rocm/rocclr/include/compiler/lib -isystem /opt/rocm/rocclr/include/compiler/lib/include" \
    cmake -DCMAKE_INSTALL_PREFIX=/opt/rocm \
          -DROCclr_DIR=/opt/rocm/rocclr \
          -DLIBROCclr_STATIC_DIR=/opt/rocm/rocclr/lib \
          -DUSE_COMGR_LIBRARY=yes \
          -DBUILD_TESTING=OFF \
          "$_dirname"
    make
}

package() {
    DESTDIR="$pkgdir" make install
    install -Dm644 "$_dirname/LICENSE.txt" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
    install -Dm644 /dev/stdin "$pkgdir/etc/ld.so.conf.d/$pkgname.conf" <<-EOF
      /opt/rocm/lib
EOF
    install -Dm644 /dev/stdin "$pkgdir/etc/OpenCL/vendors/amdocl64.icd" <<EOF
/opt/rocm/lib/libamdocl64.so
EOF
}
