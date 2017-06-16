# Maintainer: Ronald van Haren <ronald.archlinux.org>
# Maintainer: Bruno Pagani (a.k.a. ArchangeGabriel) <archange@archlinux.org>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: damir <damir@archlinux.org>
# Contributor: Tom K <tomk@runbox.com>

pkgname=hdf5
_patch=patch1
pkgver=1.10.0_${_patch}
pkgrel=1
pkgdesc="General purpose library and file format for storing scientific data"
arch=('i686' 'x86_64')
url="https://www.hdfgroup.org/hdf5/"
license=('custom')
depends=('zlib' 'bash')
makedepends=('time' 'gcc-fortran')
replaces=('hdf5-cpp-fortran')
provides=('hdf5-cpp-fortran')
source=("https://support.hdfgroup.org/ftp/HDF5/releases/${pkgname}-${pkgver:0:4}/${pkgname}-${pkgver/_/-}/src/${pkgname}-${pkgver/_/-}.tar.bz2")
# https://support.hdfgroup.org/ftp/HDF5/releases/${pkgname}-${pkgver:0:4}/${pkgname}-${pkgver/_/-}/src/${pkgname}-${pkgver/_/-}.md5
md5sums=('f6d980febe2c35c11670a9b34fa3b487')

build() {
    cd ${pkgname}-${pkgver/_/-}

    ./configure \
        --prefix=/usr \
        --disable-static \
        --enable-hl \
        --enable-build-mode=production \
        --with-pic \
        --docdir=/usr/share/doc/hdf5/ \
        --disable-sharedlib-rpath \
        --enable-cxx \
        --enable-fortran \
        --with-zlib
    make
}

package() {
    cd ${pkgname}-${pkgver/_/-}

    make -j1 DESTDIR="${pkgdir}" install

    install -dm755 "${pkgdir}"/usr/share/${pkgname}
    mv "${pkgdir}"/usr/share/{hdf5_examples,${pkgname}/examples}

    install -Dm644 COPYING "${pkgdir}"/usr/share/licenses/${pkgname}/LICENSE
}
