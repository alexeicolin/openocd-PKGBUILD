# $Id$
# Maintainer: Sergej Pupykin <arch+pub@sergej.pp.ru>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Matthias Bauch <matthias.bauch@gmail.com>
# Contributor: Laszlo Papp <djszapi2 at gmail com>
# Contributor: Samuel Tardieu <sam@rfc1149.net>

pkgname=openocd
pkgver=0.10.0
epoch=1
pkgrel=1
pkgdesc='Debugging, in-system programming and boundary-scan testing for embedded target devices'
arch=('i686' 'x86_64' 'armv7h' 'aarch64')
url='http://openocd.org/'
license=('GPL')
depends=('libftdi' 'libftdi-compat' 'libusb' 'libusb-compat' 'hidapi')
options=(!strip)
_features_off=(amtjtagaccel armjtagew buspirate ftdi gw16012 jlink oocd_trace
 opendous osbdm parport presto_libftdi remote-bitbang rlink stlink ti-icdi ulink usbprog vsllink
 aice cmsis-dap dummy jtag_vpi openjtag_ftdi usb-blaster-2 usb_blaster_libftdi)
_features_on=(bcm2835gpio)
source=(https://downloads.sourceforge.net/sourceforge/$pkgname/$pkgname-${pkgver/_/-}.tar.bz2
        'aarch64.patch')
sha256sums=('7312e7d680752ac088b8b8f2b5ba3ff0d30e0a78139531847be4b75c101316ae'
            'd98345604cd840630d02fe00d7981f9163be5eab1c90cc25e96887ae2367714f')

prepare() {
  cd $pkgname-${pkgver/_/-}
  sed -i 's|ftdi_new();|(void*)12345;|g' configure{,.ac}
  patch -p1 < ../aarch64.patch
}

build() {
  cd $pkgname-${pkgver/_/-}
  libtoolize
  autoreconf
  ./configure --prefix=/usr ${_features_off[@]/#/--disable-} ${_features_on[@]/#/--enable-} --disable-werror
  make
}

package() {
  cd $pkgname-${pkgver/_/-}
  make DESTDIR="$pkgdir" install
  (cd "$pkgdir"/usr/share/openocd/scripts/target && mv 1986*.cfg 1986be1t.cfg)
}
