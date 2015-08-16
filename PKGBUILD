# $Id$
# Maintainer: Colin Keenan <colinnkeenan at gmail dot com>
# Contributor: Jan de Groot <jgc@archlinux.org>

# The difference between this PKGBUILD and the one for extra/gvfs is this is for gvfs only and libbluray has been removed from dependencies by adding "-disable-bluray" to the ./configure line of build(). Also removed dependency on gnome-online-accounts.
# This package eliminates an error some people see when gvfs aparently treats a drive as bluray

pkgbase=gvfs-no-bluray
pkgname=$pkgbase
pkgver=1.24.1
pkgrel=1
arch=('i686' 'x86_64')
license=('LGPL')
makedepends=('avahi' 'dbus' 'fuse' 'intltool' 'libarchive' 'libcdio-paranoia' 'libgphoto2' 'libimobiledevice' 'libsoup' 'smbclient' 'udisks2' 'libsecret' 'docbook-xsl' 'gtk3' 'libmtp')
url="http://www.gnome.org"
source=(http://ftp.gnome.org/pub/gnome/sources/gvfs/${pkgver:0:4}/gvfs-$pkgver.tar.xz)
sha256sums=('d38367ce189415c36fd19dca478bc9b80694b495c3458e74fb0f13d1ac9df1f9')

build() {
  cd "gvfs-$pkgver"
  autoreconf -fi
  ./configure --prefix=/usr --sysconfdir=/etc \
      --localstatedir=/var --disable-static \
      --libexecdir=/usr/lib/gvfs \
      --with-bash-completion-dir=/usr/share/bash-completion/completions \
      --disable-obexftp \
      --disable-bluray
  make
}

package() {
  pkgdesc="Disables bluray to fix error about not being able to open ...BDMV/index.bdmv."
  provides=("gvfs")
  depends=('avahi' 'dconf' 'fuse' 'libarchive' 'libcdio-paranoia' 'libsoup' 'udisks2' 'libsecret')
  conflicts=('gvfs')
  replaces=('gvfs-obexftp')
  optdepends=('gvfs-afc: AFC (mobile devices) support'
              'gvfs-smb: SMB/CIFS (Windows client) support'
              'gvfs-gphoto2: gphoto2 (PTP camera/MTP media player) support'
              'gvfs-afp: Apple Filing Protocol (AFP) support'
              'gvfs-mtp: MTP device support'
              'gvfs-goa: gnome-online-accounts support'
              'gtk3: Recent files support')
  install=gvfs.install

  cd "gvfs-$pkgver"
  sed -e 's/^am__append_4/#am__append_4/' \
      -e 's/^am__append_5/#am__append_5/' \
      -e 's/^am__append_6/#am__append_6/' \
      -e 's/^am__append_7/#am__append_7/' \
      -i monitor/Makefile
  make DESTDIR="$pkgdir" install

  cd "$pkgdir"
  rm usr/lib/gvfs/gvfsd-{smb,smb-browse,afc,afp,afp-browse,gphoto2,mtp}
  rm usr/share/gvfs/mounts/{smb,smb-browse,afc,afp,afp-browse,gphoto2,mtp}.mount
  rm usr/share/glib-2.0/schemas/org.gnome.system.smb.gschema.xml
  rm usr/share/GConf/gsettings/gvfs-smb.convert
}

