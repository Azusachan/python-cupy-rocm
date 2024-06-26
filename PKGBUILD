# Maintainer: NA <azusanakan0 at outlook dot com>

pkgname=python-cupy-rocm
_pkgname=cupy
pkgver=13.2.0
_cccl_commit=3ef9dd9642da2d4e0b3ff77e445e73d7aabd4687
_dlpack_commit=365b823cedb281cd0240ca601aba9b78771f91a3
_jitify_commit=1a0ca0e837405506f3b8f7883bacb71c20d86d96
pkgrel=1
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
        "rocm-6.0.patch")
md5sums=('8e178493f30ed3ee427ac75c2af8ce58'
         '17c7f9635569aac243be498275ba71ba'
         '4fbb2aeba1e0ef8d7e094deb811630df'
         '0fb2589c81179e752d9bc45be72ed992'
         '05a3091829564c3e09015aef861b83bd')

prepare() {
  cd "$srcdir/$_pkgname-$pkgver"
  rm -rf third_party/{cccl,dlpack,jitify}
  ln -srfT "$srcdir/cccl-$_cccl_commit" third_party/cccl
  ln -srfT "$srcdir/dlpack-$_dlpack_commit" third_party/dlpack
  ln -srfT "$srcdir/jitify-$_jitify_commit" third_party/jitify
   patch --forward --strip=1 --input=${srcdir}/rocm-6.0.patch
}

build() {
  cd "$srcdir/$_pkgname-$pkgver"
  export CUPY_INSTALL_USE_HIP=1
  export ROCM_HOME=/opt/rocm
  export CFLAGS="-DTHRUST_DEVICE_SYSTEM=THRUST_DEVICE_SYSTEM_HIP"
  export HCC_AMDGPU_TARGET=gfx906,gfx908,gfx90a,gfx940,gfx941,gfx942,gfx1010,gfx1012,gfx1030,gfx1100,gfx1101,gfx1102
  python setup.py build
}

package() {
  cd "$srcdir/$_pkgname-$pkgver"
  python setup.py install --skip-build --prefix=/usr --root="$pkgdir" --optimize=1
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
