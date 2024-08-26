# Maintainer: Thule <vincenzo.frascino@proton.me>
# Contributor: Neptune <neptune650@proton.me>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Dag Odenhall <dag.odenhall@gmail.com>
# Contributor: Grigorios Bouzakis <grbzks@gmail.com>

pkgname=dwm
pkgver=6.5
pkgrel=1
pkgdesc="A dynamic window manager for X"
url="https://dwm.suckless.org"
arch=('i686' 'x86_64' 'arm' 'armv7h' 'armv6h' 'aarch64')
license=('MIT')
options=(zipman)
depends=('libx11' 'libxinerama' 'libxft' 'freetype2')
install=dwm.install
source=(http://dl.suckless.org/dwm/dwm-$pkgver.tar.gz
	https://dwm.suckless.org/patches/actualfullscreen/dwm-actualfullscreen-20191112-cb3f58a.diff
	https://dwm.suckless.org/patches/alwaysfullscreen/dwm-alwaysfullscreen-20160713-56a31dc.diff
	https://dwm.suckless.org/patches/alpha/dwm-alpha-20230401-348f655.diff
  https://dwm.suckless.org/patches/ewmhtags/dwm-ewmhtags-6.2.diff
  https://dwm.suckless.org/patches/fibonacci/dwm-fibonacci-20200418-c82db69.diff
	dwm-autostart-20210120-cb3f58a.diff.local
  dwm-statuscol-20232312.diff.local
	dwm.desktop)
sha256sums=('21d79ebfa9f2fb93141836c2666cb81f4784c69d64e7f1b2352f9b970ba09729'
            '7b4cabdccc8af6ee3d3819452e5028dd9d926b1edc4496f102e19210f0fcd785'
            'c75906894066180b7f8ba6d2d2352f3fad33cebcb5c9e10c63a1f28e54588a66'
            '1eee5fffb3b2668a2c20b16307071caa9622a66da1c8accb23a7e4abe8326664'
            'e027e683c074f4cd00c52f07c81be9d64d4085ef5b0a570b1fb80b8f4668c100'
            '2d1176728fc04308ab97cb9f2b628fd00038b2fb4162e84de339164fed9cc48b'
            '40307cb73788cfc600006a124d98067c1a92aee35ad96b5821f2e836fd3c492b'
            '19d8f7b59fb9e5d70d612af97811e4cb2b919eb451e281d07964b051bf791acf'
            'bc36426772e1471d6dd8c8aed91f288e16949e3463a9933fee6390ee0ccd3f81')
_sourcedir=$pkgname-$pkgver

prepare() {
  if [ -e "$BUILDDIR/config.h" ]
  then
    cp "$BUILDDIR/config.h" "$_sourcedir"
  elif [ ! -e "$BUILDDIR/config.def.h" ]
  then
    msg='This package can be configured in config.h. Copy the config.def.h '
    msg+='that was just placed into the package directory to config.h and '
    msg+='modify it to change the configuration. Or just leave it alone to '
    msg+='continue to use default values.'
    echo "$msg"
  fi
  cp "$_sourcedir/config.def.h" "$BUILDDIR"
  cd "$srcdir/$pkgname-$pkgver"

  # Apply custom patches
  patches="$(find $srcdir -maxdepth 1 -regextype posix-extended -regex '.*/dwm-.+\.diff.*$')"
  for patch in $patches; do
      warning "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
      warning "$patch"
      warning "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    patch --strip=1 --input="$patch";
  done
  if [[ -f "$srcdir/config.h" ]]; then
    cp -fv "$srcdir/config.h" config.h
  fi
}

build() {
  cd "$srcdir/$pkgname-$pkgver"
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11 FREETYPEINC=/usr/include/freetype2
}

package() {
  cd "$srcdir/$pkgname-$pkgver"
  make PREFIX=/usr DESTDIR="$pkgdir" install
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 README "$pkgdir/usr/share/doc/$pkgname/README"
  install -Dm644 "$srcdir/dwm.desktop" "$pkgdir/usr/share/xsessions/dwm.desktop"
}
