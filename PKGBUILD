# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/cups-pdf
# 						Maintainer: Andreas Radke <andyrtr at archlinux.org>
# 						Contributor: Thomas Baechler <thomas.baechler@rwth-aachen.de>

pkgname=cups-pdf
pkgver=3.0.1
pkgrel=5
pkgdesc="PDF printer for cups"
arch=(x86_64)
depends=('cups' 'ghostscript')
install=cups-pdf.install
# broken cert setup for https
url="http://www.cups-pdf.de/welcome.shtml"
license=('GPL2')
source=(http://www.cups-pdf.de/src/cups-pdf_$pkgver.tar.gz)
backup=(etc/cups/cups-pdf.conf)
md5sums=('5071bf192b9c6eb5ada4337b6917b939')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

build() {
  cd $srcdir/${pkgname}-$pkgver/src
  [ -z "$CC" ] && CC=gcc
  $CC $CFLAGS -s cups-pdf.c -o cups-pdf -lcups 
}

package() {
  cd $srcdir/${pkgname}-$pkgver/src
  install -D -m700 cups-pdf $pkgdir/usr/lib/cups/backend/cups-pdf

  # Install Postscript Color printer
  cd ../extra
  install -D -m644 CUPS-PDF_noopt.ppd $pkgdir/usr/share/cups/model/CUPS-PDF_noopt.ppd
  install -D -m644 CUPS-PDF_opt.ppd $pkgdir/usr/share/cups/model/CUPS-PDF_opt.ppd

  # Install config file
  install -D -m644 cups-pdf.conf $pkgdir/etc/cups/cups-pdf.conf
  
  # Install docs
  install -D -m 644 ../README "$pkgdir"/usr/share/doc/$pkgname/README

  # use cups group from cups pkg FS#57311/FS#56818/FS#57313
  chgrp -R cups ${pkgdir}/etc/cups
  sed -i "s:### Default\: lp:### Default\: cups:" ${pkgdir}/etc/cups/cups-pdf.conf
  sed -i "s:#Grp lp:Grp cups:" ${pkgdir}/etc/cups/cups-pdf.conf
}
