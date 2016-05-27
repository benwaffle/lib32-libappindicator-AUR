# Maintainer: Manuel Hüsers <manuel.huesers@uni-ol.de>
# Contributor: Jameson Pugh <imntreal@gmail.com>
# Contributor: Swift Geek < swift geek Ã¢t gmail dÃ¸t cÃ¸m>

_pkgbase='libappindicator'
pkgbase="lib32-${_pkgbase}"
pkgname=("${pkgbase}-gtk"{2,3})
pkgver=12.10.0
pkgrel=8
pkgdesc='Allow applications to export a menu into the Unity Menu bar (32-bit)'
arch=('i686' 'x86_64')
url="https://launchpad.net/${_pkgbase}"
license=('LGPL2.1' 'GPL3')
makedepends=("lib32-libdbusmenu-gtk"{2,3} "lib32-libindicator-gtk"{2,3}
             'dbus-glib' 'gobject-introspection' 'gtk-doc' 'gtk-sharp-2'
             'perl-xml-libxml' 'pygtk' 'vala' 'mono')
options=('!emptydirs')
source=("https://launchpad.net/${_pkgbase}/${pkgver%.*}/${pkgver}/+download/${_pkgbase}-${pkgver}.tar.gz"
        'configure-ac.patch'
        'makefile-am.patch')
sha256sums=('d5907c1f98084acf28fd19593cb70672caa0ca1cf82d747ba6f4830d4cc3b49f'
            'ba78e89956fdb6e2d45beb3d9cca259d226996465e1313cc82e3f548a9fd0e33'
            '787478d034443a591217b6debfe5e7b67049fe7456af10c62a69f6927c991246')

prepare() {
	cd "${srcdir}/${_pkgbase}-${pkgver}"

	patch -p1 -i '../configure-ac.patch'
	patch -p1 -i '../makefile-am.patch'

	cd "${srcdir}"
	rm -rf "${pkgbase}-gtk"{2,3} &> /dev/null
	cp -rp "${_pkgbase}-${pkgver}" "${pkgbase}-gtk2"
	mv     "${_pkgbase}-${pkgver}" "${pkgbase}-gtk3"
}

build() {
	export CC='gcc -m32'
	export CXX='g++ -m32'
	export PKG_CONFIG_PATH='/usr/lib32/pkgconfig'
	export CFLAGS="$CFLAGS -Wno-deprecated-declarations"

	cd "${srcdir}/${pkgbase}-gtk2"
	./autogen.sh --prefix='/usr' --sysconfdir='/etc' --localstatedir='/var' --libdir='/usr/lib32' \
	            --disable-{gtk-doc-html,mono-test,static,tests} --with-gtk=2
	make CSC=dmcs -j1

	cd "${srcdir}/${pkgbase}-gtk3"
	./autogen.sh --prefix='/usr' --sysconfdir='/etc' --localstatedir='/var' --libdir='/usr/lib32' \
	            --disable-{gtk-doc-html,mono-test,static,tests} --with-gtk=3
	make CSC=dmcs -j1
}

package_lib32-libappindicator-gtk2() {
	pkgdesc+=" (GTK+ 2 library)"
	depends=('lib32-libdbusmenu-gtk2' 'lib32-libindicator-gtk2')
	provides=("${pkgbase}")
	conflicts=("${pkgbase}")

	cd "${srcdir}/${pkgname}"
	make CSC=dmcs -j1 DESTDIR="${pkgdir}" install
	rm -rf "${pkgdir}"/usr/{include,share}
}

package_lib32-libappindicator-gtk3() {
	pkgdesc+=" (GTK+ 3 library)"
	depends=('lib32-libdbusmenu-gtk3' 'lib32-libindicator-gtk3')
	provides=("${pkgbase}3")
	conflicts=("${pkgbase}3")

	cd "${srcdir}/${pkgname}"
	make CSC=dmcs -j1 DESTDIR="${pkgdir}" install
	rm -rf "${pkgdir}"/usr/{include,share}
}
