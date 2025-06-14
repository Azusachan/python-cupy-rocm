# Maintainer: NA <azusanakan0 at outlook dot com>

pkgname=python-cupy-rocm
_pkgname=cupy
pkgver=13.4.1
_cccl_commit=3a388b7b01512d48474b98389a3e776c8d8f817a
_dlpack_commit=cd0d5e4ff888b388aef4f9b6bd5d9aa5737a020e
_jitify_commit=1a0ca0e837405506f3b8f7883bacb71c20d86d96
pkgrel=4
pkgdesc="NumPy-like API accelerated with ROCm"
url="https://cupy.dev"
arch=('x86_64')
license=('MIT')
depends=('rocm-hip-sdk' 'python-fastrlock' 'python-numpy')
makedepends=('rocm-hip-sdk' 'python-pip' 'python-setuptools')
source=("https://github.com/cupy/cupy/archive/v$pkgver.tar.gz"
        "https://github.com/NVIDIA/cccl/archive/$_cccl_commit.tar.gz"
        "https://github.com/dmlc/dlpack/archive/$_dlpack_commit.tar.gz"
        "https://github.com/NVIDIA/jitify/archive/$_jitify_commit.tar.gz"
        "ROCm6_3.patch")
md5sums=('e2d5bc3741f04fad276b33772908ae39'
         'c3c1d6a1dc4af521adbcb7dfd3a9c0a8'
         '116c914e84c24ff66bf8f8c1b9fee4f7'
         '0fb2589c81179e752d9bc45be72ed992'
         '7e6f9aa07a2524e83e923a5b89f25c5b')

prepare() {
  cd "$srcdir/$_pkgname-$pkgver"
  rm -rf third_party/{cccl,dlpack,jitify}
  ln -srfT "$srcdir/cccl-$_cccl_commit" third_party/cccl
  ln -srfT "$srcdir/dlpack-$_dlpack_commit" third_party/dlpack
  ln -srfT "$srcdir/jitify-$_jitify_commit" third_party/jitify
  patch -Np1 -i "$srcdir/ROCm6_3.patch"
}

build() {
  cd "$srcdir/$_pkgname-$pkgver"
  export CUPY_INSTALL_USE_HIP=1
  export ROCM_HOME=/opt/rocm
  export HCC_AMDGPU_TARGET=gfx906,gfx908,gfx90a,gfx940,gfx941,gfx942,gfx1010,gfx1012,gfx1030,gfx1100,gfx1101,gfx1102
  python setup.py build
}

package() {
  cd "$srcdir/$_pkgname-$pkgver"
  python setup.py install --skip-build --prefix=/usr --root="$pkgdir" --optimize=1
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
