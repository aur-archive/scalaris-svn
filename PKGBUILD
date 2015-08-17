# Maintainer: Roman Parykin <donderom at ymail dot com>

pkgname=scalaris-svn
pkgver=2800
pkgrel=2
pkgdesc="A scalable, transactional, distributed key-value store."
arch=('i686' 'x86_64')
url="http://code.google.com/p/scalaris/"
license=('Apache')
depends=('openssl' 'erlang>=R13B01')
makedepends=('subversion')
optdepends=('tokyocabinet: storage on disk')
provides=('scalaris')
conflicts=('scalaris')
backup=('etc/scalaris.cfg' 'etc/scalaris.local.cfg' 'etc/scalarisctl.conf')

_svntrunk='http://scalaris.googlecode.com/svn/trunk/'
_svnmod='scalaris'

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

  #
  # BUILD
  #
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var
  make
}

package() {
  cd "$srcdir/$_svnmod-build"
  make DESTDIR="$pkgdir/" install

  # put the LICENSE file to the licenses
  install -D -m644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # remove all the *.svn files
  rm -rf $(find "$pkgdir" -type d -name ".svn")

  # remove temp build folder
  rm -rf "$srcdir/$_svnmod-build"
}

# vim:set ts=2 sw=2 et:
