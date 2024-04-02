# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer:  Νικόλαος Κυριάκος Φυτίλης <n-fit@live.com>
# Contributor: Daniel Isenmann <daniel@archlinux.org>
# Contributor: György Balló <ballogy@freestart.hu>

pkgname=banshee
_majver=2.6
pkgver="${_majver}.2"
pkgrel=14
pkgdesc="Music management and playback for GNOME"
arch=(
  'i686'
  'x86_64'
  'arm'
  'aarch64'
)
url="http://${pkgname}.fm"
license=(
  'MIT'
)
depends=(
  libxxf86vm
  gst-plugins-base-libs
  mono-addins
  dbus-sharp-glib
  libsoup 
  taglib-sharp-git
  gconf-sharp
  libmtp
  mono-zeroconf-git
  hicolor-icon-theme
  media-player-info
  gst-plugins-bad
  mono-upnp-git
  gst-plugins-good gvfs
  libgpod
)
makedepends=(
  'intltool'
  'gnome-doc-utils'
  'gnome-common'
)
optdepends=(
  'gst-plugins-ugly: Extra media codecs'
  'gst-libav: Extra media codecs'
  'brasero: CD burning'
)
_patches=(
  Initial-port-to-GStreamer-1.0.patch
  Remove-build-time-enable-gapless-playback-option.patch
  Don-t-use-the-new-decoded-pad-signal-of-decodebin.patch
  Use-new-style-GStreamer-1.0-raw-audio-caps-in-the-WA.patch
  sqlite_fix.patch
  Remove-IDBusExportable-inheritance-from-exported-int.patch
  Use-dbus-2.patch
)
_patches_sums=(
  '16cbe2ef60e6f9b22015585bb3209648'
  '0bf7ee4241b12538779c9ecc401d142a'
  'f87534f54029794bd7be2a123ab01300'
  'd092827720e4a11549587eb3131123ae'
  '2677e6edc2e0d2ce8994adc852dda362'
  '3b28f10e167c0aae27157dcc3b828b67'
  '2c4436f7aba58fdd0c5a38d709d73e5c'
)
source=(
  "http://download.gnome.org/sources/$pkgname/2.6/${pkgname}-${pkgver}.tar.xz"
  "${_patches[@]}"
)
md5sums=(
  '12dbb8a996783f7081d538062a8589b7'
  "${_patches_sums[@]}"
)

prepare() {
  local \
    _patch
  cd \
    "$pkgname}-${pkgver}"
  for _patch in "${_patches[@]}"; do
    patch \
      -p1 \
      -i \
      "../${_patch}"
  done
  NOCONFIGURE=1 \
    ./autogen.sh
  # Fix build with mono 4.0 (Fedora)
  sed \
    -i \
    "s#mono/2.0#mono/4.5#g" \
    configure
  sed \
    -i \
    "s#Mono 2.0#Mono 4.5#g" \
    configure
  sed \
    's/CollectionExtensions/Hyena.Collections.CollectionExtensions/g'\
    -i \
    src/Core/Banshee.Services/Banshee.Preferences/Collection.cs
}

build() {
  local \
    _configure_opts=()
  _configure_opts=(
    --prefix=/usr
    --sysconfdir=/etc
    --localstatedir=/var
    --disable-docs 
    --disable-static
    --disable-scrollkeeper
    --disable-schemas-install
    --disable-boo
    --disable-daap
    --disable-youtube
    --disable-webkit
    --disable-gio
    --disable-gio_hardware
    --disable-user-help
    --with-vendor-build-id=Hip
  )
  export \
    MONO_SHARED_DIR="$srcdir/.wabi"
  mkdir \
    -p \
    "${MONO_SHARED_DIR}"
  cd \
    "${pkgname}-${pkgver}"
  MCS=/usr/bin/mcs \
  ./configure \
    "${_configure_opts[@]}"
  make
}

package() {
  export \
    MONO_SHARED_DIR="$srcdir/.wabi"
  mkdir \
    -p \
    "${MONO_SHARED_DIR}"
  cd \
    "${srcdir}/${pkgname}-${pkgver}"
  make \
    DESTDIR="${pkgdir}" \
    PREFIX="/usr" \
    install
  install \
    -Dm644 \
    COPYING \
    "${pkgdir}/usr/share/licenses/${pkgname}/COPYING"
}

# vim:set sw=2 sts=-1 et:
