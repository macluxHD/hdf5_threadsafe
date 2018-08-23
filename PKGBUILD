# Maintainer: Ronald van Haren <ronald.archlinux.org>
# Maintainer: Bruno Pagani <archange@archlinux.org>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: damir <damir@archlinux.org>
# Contributor: Tom K <tomk@runbox.com>

pkgname=hdf5
pkgver=1.10.3
pkgrel=1
pkgdesc="General purpose library and file format for storing scientific data"
arch=('x86_64')
url="https://www.hdfgroup.org/hdf5"
license=('custom')
depends=('zlib' 'libaec' 'bash')
makedepends=('cmake' 'time' 'gcc-fortran')
replaces=('hdf5-cpp-fortran')
provides=('hdf5-cpp-fortran')
options=('staticlibs')
source=("https://support.hdfgroup.org/ftp/HDF5/releases/${pkgname}-${pkgver:0:4}/${pkgname}-${pkgver/_/-}/src/${pkgname}-${pkgver/_/-}.tar.bz2")
# https://support.hdfgroup.org/ftp/HDF5/releases/${pkgname}-${pkgver:0:4}/${pkgname}-${pkgver/_/-}/src/${pkgname}-${pkgver/_/-}.md5
md5sums=('56c5039103c51a40e493b43c504ce982')
sha256sums=('c65cdcce4724a57fd3f8da9f0d109b497be30092acb9fac634d1291190d905a9')

prepare() {
    mkdir -p build
}

build() {
    cd build
    cmake ../${pkgname}-${pkgver/_/-} \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DBUILD_SHARED_LIBS=ON \
        -DCMAKE_BUILD_TYPE=Release \
        -DHDF5_BUILD_HL_LIB=ON \
        -DHDF5_BUILD_CPP_LIB=ON \
        -DHDF5_BUILD_FORTRAN=ON \
        -DHDF5_ENABLE_Z_LIB_SUPPORT=ON \
        -DHDF5_ENABLE_SZIP_SUPPORT=ON \
        -DHDF5_ENABLE_SZIP_ENCODING=ON
    cmake --build . --config Release
}

check() {
    cd build
    ctest . -C Release
}

package() {
    cd build

    make DESTDIR="${pkgdir}" install

    install -d "${pkgdir}"/usr/share/licenses/${pkgname}
    mv "${pkgdir}"/usr/share/COPYING "${pkgdir}"/usr/share/licenses/${pkgname}/
    rm "${pkgdir}"/usr/share/{RELEASE,USING_HDF5_CMake}.txt
}
