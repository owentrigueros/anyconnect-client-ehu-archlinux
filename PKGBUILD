# Maintainer: Owen Trigueros <owentrigueros@gmail.com>
pkgname=anyconnect-client-ehu
_pkgname=anyconnect-linux64
pkgver=4.8.01090
pkgrel=1
pkgdesc="Cisco AnyConnect Secure Mobility Client"
arch=('x86_64')
url="https://www.ehu.eus/es/web/ikt-tic/vpn"
license=('custom')
depends=('pangox-compat')
source=("https://www.ehu.eus/documents/1870470/8671861/anyconnect-linux64-$pkgver-predeploy-k9.tar.gz"
    "$pkgname-$pkgver-$pkgrel.patch"
    "AnyConnectLocalPolicy.xml"
    "vpnagentd.service")

md5sums=("aeaeff30b489195f469f106cbcea537f"
    "bd1c8a89e24c7f0a75ffa00f6fe5206a"
    "a9f7f0ed55d85cb43bc0746fa569b7c9" 
    "8654e46106048d5e32a4695bac98cd05")

prepare() {
    cd "$_pkgname-$pkgver"
    patch --strip 1 --input "$srcdir/$pkgname-$pkgver-$pkgrel.patch"

    # make installation relative to $pkgdir instead of /
    sed -i "s#/opt#$pkgdir/opt#g" vpn/vpn_install.sh
    sed -i "s#/etc#$pkgdir/etc#g" vpn/vpn_install.sh
    sed -i "s#/usr#$pkgdir/usr#g" vpn/vpn_install.sh
}

package() {
    mkdir -p "$pkgdir/usr/share/desktop-directories"
    mkdir -p "$pkgdir/usr/share/applications"
    mkdir -p "$pkgdir/usr/share/icons/hicolor/48x48/apps"
    mkdir -p "$pkgdir/usr/share/icons/hicolor/64x64/apps"
    mkdir -p "$pkgdir/usr/share/icons/hicolor/96x96/apps"
    mkdir -p "$pkgdir/usr/share/icons/hicolor/128x128/apps"
    mkdir -p "$pkgdir/usr/share/icons/hicolor/256x256/apps"
    mkdir -p "$pkgdir/usr/share/icons/hicolor/512x512/apps"
    mkdir -p "$pkgdir/etc/rc.d"

    cd "$_pkgname-$pkgver/vpn"
    ./vpn_install.sh

    # remove installation logs
    rm "$pkgdir/opt/cisco/anyconnect/"*.log

    # remove referencies to $src from 'vpndownloader.sh'
    sed -i "s#$pkgdir##g" "$pkgdir/opt/cisco/vpn/bin/vpndownloader.sh"

    # make symlink to libaccurl (the one that makes vpn_install is relative to $srcdir)
    ln -sf $(basename "$pkgdir/opt/cisco/anyconnect/lib/libaccurl.so.4."*) "$pkgdir/opt/cisco/anyconnect/lib/libaccurl.so.4"

    # install the service file
    mkdir -p "$pkgdir/usr/lib/systemd/system/"
    install -m 0644 "$srcdir/vpnagentd.service" "$pkgdir/usr/lib/systemd/system/"

    # install policy file
    install -m 0644 "$srcdir/AnyConnectLocalPolicy.xml" "$pkgdir/opt/cisco/anyconnect/"
}
