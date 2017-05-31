# Maintainer: Ronald van Haren <ronald.archlinux.org>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: damir <damir@archlinux.org>
# Contributor: Tom K <tomk@runbox.com>

pkgname=hdf5
pkgver=1.10.0_patch1
_pkgver=1.10.0-patch1
pkgrel=1
arch=('i686' 'x86_64')
pkgdesc="General purpose library and file format for storing scientific data"
url="http://www.hdfgroup.org/HDF5/"
license=('custom')
depends=('zlib' 'sh')
makedepends=('time')
source=(ftp://ftp.hdfgroup.org/HDF5/releases/${pkgname}-1.10/${pkgname}-${_pkgver}/src/${pkgname}-${_pkgver}.tar.bz2)
sha1sums=('2f34251186fa9e59887d8f094bc0bc90187d0aa4')

build() {
  cd "$srcdir/${pkgname}-${pkgver/_/-}"

  ./configure --prefix=/usr --disable-static \
    --enable-hl \
    --enable-linux-lfs \
    --enable-build-mode=production \
    --with-pic \
    --docdir=/usr/share/doc/hdf5/ \
    --with-pthread=/usr/lib/ \
    --disable-sharedlib-rpath
  make
}

package() {
  cd "$srcdir/${pkgname}-${pkgver/_/-}"

  make -j1 DESTDIR="${pkgdir}" install

  install -d -m755 "$pkgdir/usr/share/licenses/${pkgname}"
  install -m644 "$srcdir/${pkgname}-${pkgver/_/-}/COPYING" \
          "$pkgdir/usr/share/licenses/${pkgname}/LICENSE" 
}

