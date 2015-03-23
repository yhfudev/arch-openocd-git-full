# Maintainer: veox <veox ta wemakethings dot net>
# Contributor: Nick Østergaard <oe.nick at gmail dot com>

pkgname=openocd-git
_gitname=openocd
pkgver=6268.6c74255
pkgrel=1
pkgdesc="Debugging, in-system programming and boundary-scan testing for embedded target devices (git version)"
arch=('i686' 'x86_64' 'arm')
url="http://openocd.sourceforge.net/"
license=('GPL')
depends=('libftdi')
optdepends=('libftdi: support devices using this FTDI implementation'
            'libftd2xx: support devices using this FTDI implementation'
            'hidapi: support CMSIS-DAP compliant devices')
makedepends=('git' 'automake>=1.11' 'autoconf' 'libtool' 'tcl')
options=(!strip)
install=openocd-git.install
provides=('openocd')
conflicts=('openocd')

source=(
    "${_gitname}::git://git.code.sf.net/p/openocd/code"
    "openocd-0.9.0-exit-clean.patch"
    #"openocd-0.5.0-exit-clean.patch" # http://marc.info/?l=openocd-development&m=132957145115589&w=2
    )
md5sums=(
    'SKIP'
    '72f43470849c08d298a8de6b4598ba3b'
    #'fb11c9f995d1a65ba06f56ad2a9b84ca'
    )
sha1sums=(
    'SKIP'
    '5456bd27e1bdb466a3df12f163efb8b6128429b9'
    #'e1e58102c972b54ba474c2c33575df8e8a61a27d'
    )

# Specify desired features and device support here. A list can be
# obtained by running ./configure in the source directory.
_features=(sysfsgpio remote-bitbang )

pkgver() {
  cd "${_gitname}"
  echo $(git rev-list --count master).$(git rev-parse --short master)
}

prepare() {
  cd "$srcdir/${_gitname}"
  patch -p1 <$srcdir/openocd-0.9.0-exit-clean.patch
}

build() {
  cd "$srcdir/${_gitname}"

  ./bootstrap
  ./configure --prefix=/usr \
    --enable-maintainer-mode \
    --disable-werror \
    ${_features[@]/#/--enable-} \
    --enable-parport \
    --enable-ftdi \
    --enable-legacy-ft2232_libftdi \
    --enable-amtjtagaccel \
    --enable-ep93xx \
    --enable-at91rm9200 \
    --enable-gw16012 \
    --enable-presto_libftdi \
    --enable-usbprog \
    --enable-oocd_trace \
    --enable-jlink \
    --enable-vsllink \
    --enable-rlink \
    --enable-stlink \
    --enable-arm-jtag-ew \
    --enable-buspirate \
    --enable-usb_blaster_libftdi \
    --enable-osbdm \
    $(NULL)

  make
}

package() {
  cd "$srcdir/${_gitname}"
  make DESTDIR=${pkgdir} install
  #rm -rf ${srcdir}/$_gitname-build
  #rm -rf $pkgdir/usr/share/info/dir
}
