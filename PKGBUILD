# $Id: PKGBUILD 54702 2011-08-23 11:42:16Z spupykin $
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Maintainer: Eugen Zagorodniy e dot zagorodniy at gmail dot com
# Contributor: aunoor
# Contributor: realturner < realturner at gmail dot com > 

pkgname=notion-xft
pkgver=20110827
pkgrel=1
pkgdesc="Tabbed tiling, window manager. Fork of Ion3, with XFT patch"
url="http://sourceforge.net/projects/notion/"
arch=('i686' 'x86_64')
license=('custom:LGPL')
depends=('glib2' 'gettext' 'lua' 'libxext' 'libsm' 'libxft')
makedepends=('git' 'pkgconfig'
	     'rubber' 'latex2html' 'texlive-htmlxml' 'texlive-latexextra')
conflicts=('ion3' 'notion')
provides=('libtu' 'libextl' 'notion')

_gitroot="git://notion.git.sourceforge.net/gitroot/notion/notion"
_gitname="notion"
_gitroot2="git://notion.git.sourceforge.net/gitroot/notion/libtu"
_gitname2="libtu"
_gitroot3="git://notion.git.sourceforge.net/gitroot/notion/libextl"
_gitname3="libextl"
_gitroot4="git://notion.git.sourceforge.net/gitroot/notion/notion-doc"
_gitname4="notion-doc"

source=(doc-build-fix.patch notion-xft.patch)
md5sums=('68b348427d0531d2679a8a8d97d51c7d'
         '81e8ed3cfd272cb86a668d5571565750')

build() {
    cd ${srcdir}

    msg "Connecting to the git repository..."
    if [ -d ${srcdir}/${_gitname} ]; then
        cd ${_gitname}
        git pull origin
    else
        git clone --depth 1 ${_gitroot}
        cd ${_gitname}
    fi

    if [ -d ${srcdir}/${_gitname}/${_gitname2} ]; then
    echo
        git pull origin
    else
        git clone --depth 1 ${_gitroot2}
      echo
    fi

    if [ -d ${srcdir}/${_gitname}/${_gitname3} ]; then
    echo
        git pull origin
    else
        git clone --depth 1 ${_gitroot3}
    echo
    fi

    if [ -d ${srcdir}/${_gitname}/${_gitname4} ]; then
    echo
        git pull origin
    else
        git clone --depth 1 ${_gitroot4}
    echo
    fi

    msg "GIT checkout done or server timeout"

    rm -rf ${srcdir}/${_gitname}-build
    cp -r  ${srcdir}/${_gitname} ${srcdir}/${_gitname}-build

    cd ${srcdir}/${_gitname}-build

    msg "Starting make..."
    patch -p1 -i $srcdir/notion-xft.patch

    sed -e 's/^\(PREFIX=\).*$/\1\/usr/' \
        -e 's/^\(ETCDIR=\).*$/\1\/etc\/notion/' \
        -e 's/^\(LUA_DIR=\).*$/\1\/usr/' \
        -e 's/^\(X11_PREFIX=\).*/\1\/usr/' \
        -e 's/^#\(USE_XFT=\)/\1/' \
        -i system.mk

    make INCLUDES=-I${srcdir}/${_gitname}-build
    (cd notion-doc && patch -p1 <$srcdir/doc-build-fix.patch)
    (cd notion-doc && make -j1 TOPDIR=.. all)
}

package() {
    cd ${srcdir}/${_gitname}-build
    make PREFIX=${pkgdir}/usr ETCDIR=${pkgdir}/etc/notion install
    (cd notion-doc && make  PREFIX=${pkgdir}/usr TOPDIR=.. install)
    mkdir -p ${pkgdir}/usr/share/licenses/notion
    cp LICENSE ${pkgdir}/usr/share/licenses/notion
}
