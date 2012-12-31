# $Id: PKGBUILD 169381 2012-10-20 09:06:34Z eric $
# Maintainer: 
# Contributor: Dale Blount <dale@archlinux.org>

pkgname=net-snmp
pkgver=5.4.4
pkgrel=1
pkgdesc="A suite of applications used to implement SNMP v1, SNMP v2c and SNMP v3 using both IPv4 and IPv6"
arch=('i686' 'x86_64')
url="http://www.net-snmp.org/"
license=('BSD')
depends=('openssl' 'libnl' 'pciutils')
makedepends=('python2-distribute')
optdepends=('perl-term-readkey: for snmpcheck application' 
            'perl-tk: for snmpcheck and tkmib applications'
            'python2: for the python modules')
provides=('ucd-snmp')
backup=('etc/conf.d/snmpd')
options=('!libtool' '!emptydirs')
source=(http://downloads.sourceforge.net/${pkgname}/${pkgname}-${pkgver}.tar.gz{,.asc}
        snmpd.confd snmpd.rc snmpd.service)
sha1sums=('e113185e20eba6a4dc9a909c6311db018993a642'
          '2478e4f11d0699dcf03066abc2fac8d6d858d318'
          'cf811da9e57bbca34d8e2a3c358bb3bfc0c2b33b'
          '90600c0141eed10d6e3ca3ccc97ad8dda15c2112'
          'f59ddc049c1c689f04e68c3c7cd8b8e301df3158'
          )

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  autoreconf -f -i

  sed -i -e "s:\(install --basedir=\$\$dir\):\1 --root='${pkgdir}':" Makefile.in
  PYTHONPROG=/usr/bin/python2 ./configure --prefix=/usr \
    --sysconfdir=/etc \
    --mandir=/usr/share/man \
    --enable-ucd-snmp-compatibility \
    --enable-ipv6 \
    --with-python-modules \
    --with-default-snmp-version="3" \
    --with-sys-contact="root@localhost" \
    --with-sys-location="Unknown" \
    --with-logfile="/var/log/snmpd.log" \
    --with-mib-modules="host misc/ipfwacc ucd-snmp/diskio tunnel ucd-snmp/dlmod" \
    --with-persistent-directory="/var/net-snmp" \
    --disable-static
  make NETSNMP_DONT_CHECK_VERSION=1
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make DESTDIR="${pkgdir}" INSTALL_PREFIX="${pkgdir}" INSTALLDIRS=vendor install
  install -D -m755 "${srcdir}/snmpd.rc" "${pkgdir}/etc/rc.d/snmpd"
  install -D -m644 "${srcdir}/snmpd.confd" "${pkgdir}/etc/conf.d/snmpd"
  install -D -m644 "${srcdir}/snmpd.service" "${pkgdir}/usr/lib/systemd/system/snmpd.service"
  install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
