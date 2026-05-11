# Maintainer: Ragu Manjegowda <github.com/ragu-manjegowda>
pkgname=openconnect-gp-git
_pkgname=openconnect
pkgver=a7e751442e0e4bb8e3f18965960b1428e1a26bbc
pkgrel=1
pkgdesc="Open client for Cisco AnyConnect VPN with patched upstream GlobalProtect support"
arch=('i686' 'x86_64')
license=('LGPL2.1')
url="https://www.infradead.org/openconnect.html"
depends=('libproxy' 'vpnc' 'pcsclite' 'trousers' 'stoken' 'oath-toolkit')
makedepends=('intltool' 'python' 'git' 'autoconf' 'automake' 'libtool')
options=('!emptydirs' '!debug')
provides=($_pkgname 'libopenconnect.so')
conflicts=($_pkgname)
commit=a7e751442e0e4bb8e3f18965960b1428e1a26bbc
source=("$pkgname::git+https://gitlab.com/openconnect/openconnect.git/#commit=$commit"
        "gp-cas-callback.patch"
        "gp_browser_helper.c"
        "gp_browser_helper.desktop")
sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP')

pkgver() {
  echo "$commit"
}

prepare() {
  cd "$pkgname"
  cp "$srcdir/gp_browser_helper.c" "$srcdir/gp_browser_helper.desktop" .
  patch -p1 < "$srcdir/gp-cas-callback.patch"
}

build() {
  cd "$pkgname"
  find . -name "libtool" -o -name "ltmain.sh" -o -name "aclocal.m4" -o -name "config.guess" -o -name "config.sub" | xargs rm -f

  ./autogen.sh
  autoreconf -fi

  PYTHON=/usr/bin/python ./configure --prefix=/usr \
      --sbindir=/usr/bin \
      --libexecdir=/usr/lib \
      --disable-static \
      --without-gnutls \
      --with-vpnc-script=/etc/vpnc/vpnc-script
  sed -i -e "s/ -shared / $LDFLAGS\0 /g" libtool
  sed -i 's|update-desktop-database|true|' Makefile

  make V=0
}

package() {
  cd "$pkgname"
  make DESTDIR="$pkgdir" install
}
