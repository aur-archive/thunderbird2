# Maintainer: linuxSEAT <--put_my_name_here--@gmail.com>
# Contributor: Alexander Baldeck <alexander@archlinux.org>
# Contributor: Dale Blount <dale@archlinux.org>
# Contributor: Anders Bostrom <anders.bostrom@home.se>
pkgname=thunderbird2
pkgver=2.0.0.24
pkgrel=1
pkgdesc="Standalone Mail/News reader (Version 2)"
arch=('i686' 'x86_64')
license=('MPL' 'GPL')
url="http://www.mozilla.org/projects/thunderbird"
provides=('mozilla-thunderbird' 'thunderbird')
conflicts=('mozilla-thunderbird' 'thunderbird')
replaces=('mozilla-thunderbird' 'thunderbird')
depends=('gtk2>=2.16.5' 'gcc-libs>=4.4' 'libidl2>=0.8.13' 'mozilla-common' 'nss>=3.12.3' 'libxt' 'shared-mime-info')
makedepends=('zip' 'pkgconfig' 'imagemagick>=6.5.3.10-1' 'libgnomeui')
options=('!makeflags')
source=(ftp://ftp.mozilla.org/pub/mozilla.org/thunderbird/releases/${pkgver}/source/thunderbird-${pkgver}-source.tar.bz2
        mozconfig
        thunderbird.desktop
        thunderbird-1.5-lang.patch
        firefox-2.0-link-layout.patch
        thunderbird-appversion.patch
        xulrunner-elif.patch
        libpng.patch)
md5sums=('6e09f74b25aac46705abb13ea4c26f67'
         '17e3015259da53f40bc445b27078f693'
         'ea4e3c3dee98e3891bef16409551eb6e'
         'bc6f10a06407faee6494acad546aabf9'
         'b933c00957ea793fe940f4d46a85e10e'
         '63dd0436e43e2de71abcf9160a1a6f44'
         '38457261a6355365079dbe5c2342ec68'
         'a0c53b831cf162a4723a59950f70572e')

build() {
  # -Wl,--as-needed seems to fail here
  unset LDFLAGS

  cd "${srcdir}/mozilla"
  patch -Np1 -i "${srcdir}/thunderbird-1.5-lang.patch" || return 1
  patch -Np1 -i "${srcdir}/firefox-2.0-link-layout.patch" || return 1
  patch -Np1 -i "${srcdir}/thunderbird-appversion.patch" || return 1
  patch -Np1 -i "${srcdir}/xulrunner-elif.patch" || return 1
  patch -Np1 -i "${srcdir}/libpng.patch" || return 1
  
  cp "${srcdir}/mozconfig" .mozconfig

  echo "ac_cv_visibility_pragma=no" >> .mozconfig

  export MOZ_PROJECT=mail
  unset CXXFLAGS
  unset CFLAGS

  make -f client.mk build || return 1
  make DESTDIR="${pkgdir}" install || return 1

  rm -rf "${pkgdir}/usr/bin/defaults"

  install -m755 -d "${pkgdir}/usr/share/applications"
  install -m755 -d "${pkgdir}/usr/share/pixmaps"
  convert "${srcdir}/mozilla/mail/app/default.xpm" \
      "${pkgdir}/usr/share/pixmaps/thunderbird.png" || return 1
  install -m644 "${srcdir}/thunderbird.desktop" \
      "${pkgdir}/usr/share/applications/" || return 1

  install -m644 "${srcdir}/mozilla/mail/app/default.xpm" \
      "${pkgdir}/usr/lib/thunderbird-2.0/icons/" || return 1

  rm -f ${pkgdir}/usr/lib/pkgconfig/thunderbird-ns{s,pr}.pc
}
