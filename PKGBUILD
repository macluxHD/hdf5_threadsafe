# Maintainer: Bruno Pagani <archange@archlinux.org>
# Maintainer: Carl Smedstad <carsme@archlinux.org>
# Contributor: Ronald van Haren <ronald.archlinux.org>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: damir <damir@archlinux.org>
# Contributor: Tom K <tomk@runbox.com>

pkgname=hdf5
pkgver=1.14.5
pkgrel=1
pkgdesc="General purpose library and file format for storing scientific data"
arch=(x86_64)
url="https://www.hdfgroup.org/hdf5"
license=(custom)
depends=(zlib libaec bash)
makedepends=(cmake time gcc-fortran java-environment)
replaces=(hdf5-java)
provides=(hdf5-java)
#source=(https://support.hdfgroup.org/ftp/HDF5/releases/${pkgname}-${pkgver:0:4}/${pkgname}-${pkgver/_/-}/src/${pkgname}-${pkgver/_/-}.tar.bz2)
source=(https://github.com/HDFGroup/hdf5/archive/hdf5_$pkgver/$pkgname-$pkgver.tar.gz)
sha256sums=('c83996dc79080a34e7b5244a1d5ea076abfd642ec12d7c25388e2fdd81d26350')

prepare() {
    cd ${pkgname}-${pkgname}_${pkgver/_/-}
    # Don't mess with build flags
    sed -e '/-Werror/d' -i configure
}

build() {
    # Crazy workaround: run CMake to generate pkg-config file
    #cmake -B build -S ${pkgname}-${pkgver/_/-} \
    mkdir -p build && cd build
    cmake ../${pkgname}-${pkgname}_${pkgver/_/-} \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DBUILD_STATIC_LIBS=OFF \
        -DCMAKE_BUILD_TYPE=Release \
        -DHDF5_BUILD_HL_LIB=ON \
        -DHDF5_BUILD_CPP_LIB=ON \
        -DHDF5_BUILD_FORTRAN=ON \
        -DHDF5_BUILD_JAVA=ON \
        -DHDF5_ENABLE_Z_LIB_SUPPORT=ON \
        -DHDF5_ENABLE_SZIP_SUPPORT=ON \
        -DHDF5_ENABLE_SZIP_ENCODING=ON \
        -DUSE_LIBAEC=ON
    # But don’t build with it, it’s quite broken
    cd ../${pkgname}-${pkgname}_${pkgver/_/-}
    ./configure \
        --prefix=/usr \
        --docdir=/usr/share/doc/hdf5/ \
        --with-examplesdir=/usr/share/doc/hdf5/examples/ \
        --disable-static \
        --disable-sharedlib-rpath \
        --enable-build-mode=production \
        --enable-hl \
        --enable-cxx \
        --enable-fortran \
        --enable-java \
        --with-pic \
        --with-zlib \
        --with-szlib
    make
}

check() {
    cd ${pkgname}-${pkgname}_${pkgver/_/-}
    # Without this, checks are failing with messages like “error while loading shared libraries: libhdf5.so.101: cannot open shared object file: No such file or directory”
    export LD_LIBRARY_PATH="${srcdir}"/${pkgname}-${pkgver/_/-}/src/.libs/
    export LD_LIBRARY_PATH="$LD_LIBRARY_PATH":"${srcdir}"/${pkgname}-${pkgver/_/-}/c++/src/.libs/
    export LD_LIBRARY_PATH="$LD_LIBRARY_PATH":"${srcdir}"/${pkgname}-${pkgver/_/-}/fortran/src/.libs/
    export LD_LIBRARY_PATH="$LD_LIBRARY_PATH":"${srcdir}"/${pkgname}-${pkgver/_/-}/hl/src/.libs/
    export LD_LIBRARY_PATH="$LD_LIBRARY_PATH":"${srcdir}"/${pkgname}-${pkgver/_/-}/hl/c++/src/.libs/
    export LD_LIBRARY_PATH="$LD_LIBRARY_PATH":"${srcdir}"/${pkgname}-${pkgver/_/-}/hl/fortran/src/.libs/
    make check
}

package() {
    cd ${pkgname}-${pkgname}_${pkgver/_/-}
    make DESTDIR="${pkgdir}" install
    install -Dm644 COPYING -t "${pkgdir}"/usr/share/licenses/${pkgname}

    # Install pkg-config files from CMake tree
    install -Dm644 ../build/CMakeFiles/hdf5{,_hl}{,_cpp,_fortran}.pc -t "${pkgdir}"/usr/lib/pkgconfig/
    # Fix version numbers in pkg-config files
    sed -i '/Requires/ s/-/ = /g' "${pkgdir}"/usr/lib/pkgconfig/*.pc
    # Fix bogus include path
    sed -re "s|-I/build/hdf5/src/hdf5.*/src/H5FDsubfiling||g" -i "${pkgdir}"/usr/lib/libhdf5.settings -i "${pkgdir}"/usr/bin/*
}
