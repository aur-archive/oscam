# Maintainer:  Joakim Hernberg <jhernberg@alchemy.lu>
# Contributor: Markus Opitz <mastero23 at gmail dot com>

pkgname=oscam
pkgver=1.10
pkgrel=3
pkgdesc="The Open Source Conditional Access Module daemon"
arch=('i686' 'x86_64')
url="http://oscam.to/"
license=('GPL3')
depends=('bash' 'openssl' 'pcsclite')
makedepends=('cmake' 'subversion')
options=(emptydirs)
install=oscam.install
source=(oscam.service)
md5sums=('9feece4aed599a1ba005305c5e23960e')

_svnrepo=http://streamboard.de.vu/svn/oscam/tags/1.10
_svnmod=oscam
_svnrev=5954

build() {
  cd "$srcdir"

  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $_svnrev)
  else
    svn co $_svnrepo --config-dir ./ -r $_svnrev $_svnmod
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_svnmod-build"
  cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"

  cmake -DCMAKE_INSTALL_PREFIX=/usr \
        -DWEBIF=1 \
        -DWITH_SSL=1 \
        "$srcdir/$_svnmod-build"
  make
}

package() {
  cd "$srcdir/$_svnmod-build"

  make DESTDIR="$pkgdir/" install

  install -m 755 -d "$pkgdir/etc/oscam"
  install -D -m 644 "$srcdir/oscam.service" "$pkgdir/usr/lib/systemd/system/oscam.service"
}

md5sums=('bd18d6326ac6c10caaef9d4e19542e01')
