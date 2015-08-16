# Maintainer: Thomas Dziedzic < gostrc at gmail >, lainme <lainme993@gmail.com>
# Contributor: Klimov Max <cleemmi@gmail.com>

pkgname=libcgns
_basever=3.1.3
_relver=4
pkgver=${_basever}.${_relver}
pkgrel=2
pkgdesc='General purpose library for the storage and retrieval of computational fluid dynamics analysis data by CGNS standard'
arch=('i686' 'x86_64')
url='http://www.cgns.org'
license=('custom')
conflicts=('libcgns2' 'libcgns-paraview')
depends=('tk' 'hdf5')
makedepends=('cmake')
source=("http://downloads.sourceforge.net/project/cgns/cgnslib_${_basever:0:3}/cgnslib_${_basever}-${_relver}.tar.gz" deprecated_result.patch)
md5sums=('442bba32b576f3429cbd086af43fd4ae' 
         '302731bfaf13a05098e14269b9ac4a7b')

# need to tell cmake when to build 64bit version
[[ "$CARCH" == "i686" ]] && _64bits=OFF
[[ "$CARCH" == "x86_64" ]] && _64bits=ON

build() {
  # patching sources
  cd "${srcdir}"/cgnslib_${_basever}
  patch -p1 -i ../deprecated_result.patch
  cd "${srcdir}"

  mkdir -p build
  cd build

  cmake \
    -DCGNS_BUILD_SHARED:BOOL=ON \
    -DCMAKE_BUILD_TYPE:STRING=Release \
    -DCMAKE_INSTALL_PREFIX:PATH=/usr \
    -DCMAKE_SKIP_RPATH:BOOL=ON \
    -DBUILD_CGNSTOOLS:BOOL=ON \
    -DENABLE_64BIT:BOOL=${_64bits} \
    -DENABLE_FORTRAN:BOOL=ON \
    -DENABLE_HDF5:BOOL=ON \
    -DENABLE_SCOPING:BOOL=ON \
    -DENABLE_TESTS:BOOL=ON \
    ../cgnslib_${_basever}

  make
}

check() {
  cd build

  make test
}

package() {
  cd build

  make DESTDIR=${pkgdir} install

  # install license
  install -d ${pkgdir}/usr/share/licenses/libcgns
  install -m644 ${srcdir}/cgnslib_${_basever}/license.txt \
    ${pkgdir}/usr/share/licenses/libcgns
}
