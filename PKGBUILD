# Maintainer: NA <azusanakan0 at outlook dot com>

pkgname=python-cupy-rocm
_pkgname=cupy
pkgver=13.3.0
_cupy_commit=1e4a78b77d66840d8e2e15e0d73e98573bf5be22
_cccl_commit=2246a8772a78346d91e580e121cedb820da2648f
_dlpack_commit=cd0d5e4ff888b388aef4f9b6bd5d9aa5737a020e
_jitify_commit=1a0ca0e837405506f3b8f7883bacb71c20d86d96
pkgrel=2
pkgdesc="NumPy-like API accelerated with ROCm"
url="https://cupy.dev"
arch=('x86_64')
license=('MIT')
depends=('rocm-hip-sdk' 'python-fastrlock' 'python-numpy')
makedepends=('rocm-hip-sdk' 'python-pip' 'python-setuptools')
source=("https://github.com/cupy/cupy/archive/$_cupy_commit.tar.gz"
        "https://github.com/NVIDIA/cccl/archive/$_cccl_commit.tar.gz"
        "https://github.com/dmlc/dlpack/archive/$_dlpack_commit.tar.gz"
        "https://github.com/NVIDIA/jitify/archive/$_jitify_commit.tar.gz"
        "Allow_Cython-3_and_resolve_compilation_errors.patch")
md5sums=('435f94c97655931ee4837c67cd46026f'
         '9f8aa4da2e9812e6d1a60045f5e0a7ab'
         '116c914e84c24ff66bf8f8c1b9fee4f7'
         '0fb2589c81179e752d9bc45be72ed992'
         'b79ed9566ae8cfa1eeee1050aaa49e03')

prepare() {
  cd "$srcdir/$_pkgname-$_cupy_commit"
  rm -rf third_party/{cccl,dlpack,jitify}
  ln -srfT "$srcdir/cccl-$_cccl_commit" third_party/cccl
  ln -srfT "$srcdir/dlpack-$_dlpack_commit" third_party/dlpack
  ln -srfT "$srcdir/jitify-$_jitify_commit" third_party/jitify
  patch --forward --strip=1 --input=${srcdir}/Allow_Cython-3_and_resolve_compilation_errors.patch
}

build() {
  cd "$srcdir/$_pkgname-$_cupy_commit"
  export CUPY_INSTALL_USE_HIP=1
  export ROCM_HOME=/opt/rocm
  export CFLAGS="-DTHRUST_DEVICE_SYSTEM=THRUST_DEVICE_SYSTEM_HIP"
  export HCC_AMDGPU_TARGET=gfx906,gfx908,gfx90a,gfx940,gfx941,gfx942,gfx1010,gfx1012,gfx1030,gfx1100,gfx1101,gfx1102
  python setup.py build
}

package() {
  cd "$srcdir/$_pkgname-$_cupy_commit"
  python setup.py install --skip-build --prefix=/usr --root="$pkgdir" --optimize=1
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
