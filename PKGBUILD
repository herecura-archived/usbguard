# Maintainer: Ian Beringer <ian@ianberinger.com>

pkgname=usbguard
pkgver=0.7.1
pkgrel=3
license=('GPL2')
pkgdesc='USBGuard is a software framework for implementing USB device authorization policies'
makedepends=('libxslt' 'asciidoctor' 'imagemagick' 'qt5-svg' 'qt5-tools')
depends=('libqb' 'libsodium' 'libcap-ng' 'protobuf' 'polkit' 'dbus-glib')
optdepends=(
    'qt5-svg: usbguard applet'
    'qt5-tools: usbguard applet'
    'hicolor-icon-theme: usbguard applet'
)
arch=('x86_64')
url='https://github.com/dkopecek/usbguard'
source=(
	"$url/releases/download/$pkgname-$pkgver/$pkgname-$pkgver.tar.gz"
	"$url/releases/download/$pkgname-$pkgver/$pkgname-$pkgver.tar.gz.sig"
    'include.patch')
sha512sums=('c4eb133d3f2d87552789551d8df0a0910962713c42444b8218db858a83f4d9131d1e1c62923c7a0a249dab0517f4172c636d00e714eb0a23b08307adf9eb601a'
            'SKIP'
            '891ba5d2a3e20ecd0044e8af6fcf05ebf24de875948d16d5cd733cae2934c3d5b8b98f52bf11cc8b7af96e4fc6bb4aed52aaeaf3200c119fc622d19249ccc7a8')
validpgpkeys=('430C1928960157CC45FA1BEBAA06120530AE0466')
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
    install -pDm 644 "$pkgdir/usr/share/applications/usbguard-applet-qt.desktop" \
        "$pkgdir/etc/xdg/autostart/usbguard-applet-qt.desktop"
    install -pDm 600 usbguard-daemon.conf \
        "$pkgdir/etc/usbguard/usbguard-daemon.conf"
    install -pDm 600 rules.conf "$pkgdir/etc/usbguard/rules.conf"
}
