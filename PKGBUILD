# Maintainer: xxxx <xxx@xxx.com>

_pkgname=yong
pkgname=${_pkgname}-git
pkgver=2.5.0
pkgrel=1
pkgdesc="A Chinese Input Method"
arch=('x86_64')
url="https://github.com/dgod/yong"
license=('GPL')
depends=('gtk2' 'gtk3' 'qt5-base' 'pango')
makedepends=('git' 'nodejs' 'gcc' 'ibus' 'libxkbcommon')
source=('git+https://github.com/dgod/yong.git' 'git+https://github.com/dgod/build.js.git')
sha512sums=('SKIP' 'SKIP')

pkgver() {
    awk '/%define +version/{print $3}' $srcdir/$_pkgname/install/yong.spec | sed 's|-|.|g'
}

prepare(){
    patch $srcdir/$_pkgname/im/qt5-im/build.txt < ../qt5-im-build.patch
    patch $srcdir/$_pkgname/install/build.txt < ../yong-install.patch
    
    cd $srcdir/$_pkgname
    mkdir -p {llib,cloud,gbk,mb,vim}/l64
    mkdir -p {im,config}/{l64-gtk3,l64-gtk2}
    mkdir -p im/gtk-im/{l64-gtk3,l64-gtk2}
    mkdir -p im/qt5-im/l64-qt5
    mkdir -p im/IMdkit/l64
}

build() {
 
    buildjs="$srcdir/build.js/build.js"
    cd $srcdir/$_pkgname
    node $buildjs l64
    node $buildjs -C install  copy64
    
}

package() {
    mkdir -p $pkgdir/usr/bin
    mkdir -p $pkgdir/opt
    mkdir -p $srcdir/$_pkgname/install/yong/l64/qt5-im
    cp -f $srcdir/$_pkgname/im/qt5-im/l64-qt5/libyongplatforminputcontextplugin.so $srcdir/$_pkgname/install/yong/l64/qt5-im/
    cp -r $srcdir/$_pkgname/install/yong $pkgdir/opt
    cd $pkgdir/opt/yong
    
    ln -sf ../../opt/yong/l64/yong-gtk3 $pkgdir/usr/bin/yong
    ln -sf ../../opt/yong/l64/yong-config-gtk3 $pkgdir/usr/bin/yong-config
    install -D locale/zh_CN.mo $pkgdir/usr/share/locale/zh_CN/LC_MESSAGES/yong.mo
    install -D l64/gtk-im/im-yong-gtk2.so $pkgdir/usr/lib/gtk-2.0/2.10.0/immodules/im-yong.so
    install -D l64/gtk-im/im-yong-gtk3.so $pkgdir/usr/lib/gtk-3.0/3.0.0/immodules/im-yong.so
    install -D l64/qt5-im/libyongplatforminputcontextplugin.so $pkgdir/usr/lib/qt/plugins/platforminputcontexts/libyongplatforminputcontextplugin.so
}

post_install() {
    gtk-query-immodules-2.0 --update-cache 
    gtk-query-immodules-3.0 --update-cache 
}

post_remove() {
  gtk-query-immodules-2.0 --update-cache 
  gtk-query-immodules-3.0 --update-cache 
}
