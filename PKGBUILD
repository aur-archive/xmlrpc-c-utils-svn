# Maintainer: Guten Ye

pkgname="xmlrpc-c-utils-svn"
pkgver=2394
pkgrel=1
pkgdesc="contains xmlrpc command for xmlrpc-c"
url="http://xmlrpc-c.sourceforge.net"
license=("custom:xmlrpc-c")
arch=("i686" "x86_64")
depends=("curl" "libxml2" "subversion")
options=("!makeflags" "!libtool")
provides=("xmlrpc-c-utils")

_svntrunk="https://xmlrpc-c.svn.sourceforge.net/svnroot/xmlrpc-c/trunk"
_svnmod="xmlrpc-c"

build() {
  cd "$srcdir"
  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"

  [ "${CARCH}" = "x86_64" ] && export CFLAGS="${CFLAGS} -fPIC"

  ./configure --prefix=/usr \
              --mandir=/usr/share/man \
              --enable-libxml2-backend \
              --disable-cgi-server \
              --disable-abyss-server \
              --disable-libwww-client \
              --disable-wininet-client

  make CFLAGS_PERSONAL="${CFLAGS}"
  cd tools/xmlrpc
  make
}

package() {
  cd "$srcdir/$_svnmod-build"

  install -Dm755 tools/xmlrpc/xmlrpc "$pkgdir/usr/bin/xmlrpc"
}

# vim:set ts=2 sw=2 et:
md5sums=()
