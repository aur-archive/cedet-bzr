# Contributor: Jed Brown <jed@59A2.org>

pkgname=cedet-bzr
pkgver=8450
pkgrel=1
pkgdesc="Collection of Emacs Development Enviromnent Tools (Bazaar version)"
arch=('any')
url="http://cedet.sourceforge.net/"
license=('GPL')
conflicts=('cedet' 'cedet-cvs')
provides=('cedet')
depends=('emacs')
makedepends=('bzr')
source=('cedet-bzr.install')
md5sums=('0468130bff9ea8d48fe4c7ec996f3686')
install=$pkgname.install

_bzrtrunk='bzr://cedet.bzr.sourceforge.net/bzrroot/cedet/code/trunk'
_bzrmod=cedet

build() {
  cd "$srcdir"

  msg "Connecting to BZR server...."
  if [ -d $_bzrmod/.bzr ]; then
    (cd $_bzrmod && bzr update -v)
    msg "BZR update done or server timeout"
  else
    bzr checkout --lightweight $_bzrtrunk $_bzrmod
    msg "BZR checkout done or server timeout"
  fi
  msg "Starting build ..."

  rm -rf ${_bzrmod}-build
  cp -a ${_bzrmod} ${_bzrmod}-build
  cd ${_bzrmod}-build

  unset MAKEFLAGS
  make
}

package() {
  cd "$srcdir/${_bzrmod}-build"
  install -d $pkgdir/usr/share/emacs/site-lisp/cedet
  install cedet-devel-load.el cedet-remove-builtin.el $pkgdir/usr/share/emacs/site-lisp/cedet
  cp -a lisp contrib $pkgdir/usr/share/emacs/site-lisp/cedet

  # http://sourceforge.net/tracker/index.php?func=detail&aid=3585232&group_id=17886&atid=117886
  touch $pkgdir/usr/share/emacs/site-lisp/cedet/.nosearch

  make INFODIR=$pkgdir/usr/share/info install-info
}
