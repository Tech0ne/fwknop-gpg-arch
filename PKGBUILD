# Maintainer: tech0ne
pkgname=fwknop-gpg
_pkgname=fwknop
pkgver=2.6.11
pkgrel=1
pkgdesc="Single Packet Authorization (SPA) client and server, built with GPG (gpgme) support"
arch=('x86_64' 'aarch64')
url="https://www.cipherdyne.org/fwknop/docs/SPA.html"
license=('GPL-2.0-or-later')
depends=('libpcap' 'gpgme' 'gnupg' 'iptables')
makedepends=('pkgconf' 'autoconf' 'automake' 'libtool' 'texinfo')
provides=("$_pkgname=$pkgver")
conflicts=("$_pkgname")
backup=('etc/fwknop/fwknopd.conf'
        'etc/fwknop/access.conf')
source=("$_pkgname-$pkgver.tar.gz::https://github.com/mrash/fwknop/archive/refs/tags/$pkgver.tar.gz"
        'gpgme-config'
        'fwknopd.service')
sha256sums=('SKIP' 'SKIP' 'SKIP')

prepare() {
  cd "$_pkgname-$pkgver"
  chmod +x "$srcdir/gpgme-config"
  ./autogen.sh
}

build() {
  cd "$_pkgname-$pkgver"
  # gpgme >= 2.0 dropped gpgme-config; feed the old AM_PATH_GPGME macro a pkg-config shim
  export GPGME_CONFIG="$srcdir/gpgme-config"
  export CFLAGS+=" -std=gnu17 -fcommon"
  ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --with-gpgme \
    --with-gpg=/usr/bin/gpg
  make
}

package() {
  cd "$_pkgname-$pkgver"
  make DESTDIR="$pkgdir" install
  chmod 600 "$pkgdir"/etc/fwknop/{fwknopd,access}.conf
  install -Dm644 "$srcdir/fwknopd.service" "$pkgdir/usr/lib/systemd/system/fwknopd.service"
}
