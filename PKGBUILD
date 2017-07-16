# Maintainer: Ian Beringer <ian@ianberinger.com>

pkgname=usbguard
pkgver=0.7.0
pkgrel=1
license=('GPL2')
pkgdesc='USBGuard is a software framework for implementing USB device authorization policies'
makedepends=('libxslt' 'imagemagick')
depends=('libqb' 'libsodium' 'libcap-ng' 'protobuf' 'polkit' 'qt5-base'
'qt5-svg' 'qt5-tools' 'hicolor-icon-theme' 'dbus-glib')
arch=('i686' 'x86_64')
url='https://github.com/dkopecek/usbguard'
source=(
	"$url/releases/download/$pkgname-$pkgver/$pkgname-$pkgver.tar.gz"
	"$url/releases/download/$pkgname-$pkgver/$pkgname-$pkgver.tar.gz.sig"
    'include.patch'
    'disable_usecase-tests.patch')
sha512sums=('e0a63457011379f50cc5eb14516bb1b6192d5710c81e7fa9042b67c481514158811e761e982ad16a9b1fcae62c58e68574910da1a98be3a07e6c99f69d5c03c4'
            'SKIP'
            '891ba5d2a3e20ecd0044e8af6fcf05ebf24de875948d16d5cd733cae2934c3d5b8b98f52bf11cc8b7af96e4fc6bb4aed52aaeaf3200c119fc622d19249ccc7a8'
            '0117de2533bc95bd317fb7b01fb92d747579ac9e3dac377070d172228cc2d077360c8a4ab28cd7a0ffcffe2254d733403fbd66fc898126f30e19d3d674149778')
validpgpkeys=('D36B9AB1E6809AF54691C23A3AEF037DB25991BC')
backup=(
	'etc/usbguard/usbguard-daemon.conf'
	'etc/usbguard/rules.conf')
install=usbguard.install

prepare() {
	cd "$pkgname-$pkgver"
	rm -f usbguard-daemon.conf
}

build() {
	cd "$pkgname-$pkgver"
	patch -p1 -i "$srcdir/include.patch"
    patch -p1 -i "$srcdir/disable_usecase-tests.patch"
	aclocal
    ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
        --sys=/etc -sbindir=/usr/bin --libdir=/usr/lib \
        --enable-systemd \
        --with-gui-qt=qt5 \
        --with-bundled-catch \
        --with-bundled-pegtl
    make
    touch rules.conf
}

check() {
    cd "$pkgname-$pkgver"
    make check
}

package() {
	cd "$pkgname-$pkgver"
	make SYSTEMD_UNIT_DIR="/usr/lib/systemd/system" DESTDIR="$pkgdir" install
	mkdir -p "$pkgdir/etc/usbguard"
	install -p -m 644 ./usbguard-daemon.conf \
        "$pkgdir/etc/usbguard/usbguard-daemon.conf"
	install -p -m 644 ./rules.conf "$pkgdir/etc/usbguard/rules.conf"
}
