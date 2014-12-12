# Maintainer: Lance Chen <cyen0312+aur@gmail.com>
# Contributor: Roan Huang <inceptioninx@gmail.com>

pkgbase=openvswitch
pkgname=('openvswitch' 'openvswitch-datapath-dkms')
pkgver=2.3.1
pkgrel=1
pkgdesc="An Open Virtual Switch"
arch=('i686' 'x86_64')
url="http://openvswitch.org/"
license=('Apache')
depends=('python2')
makedepends=('python2')
source=("http://openvswitch.org/releases/openvswitch-$pkgver.tar.gz"
        "dkms.conf"
        "openvswitch.tmpfiles"
        "ovs-vswitchd.service"
        "ovsdb-server.service")
md5sums=('c008c1de0a8b6363b37afa599105d6d6'
         '6dfb9ec230994864a9da88a7d288a254'
         '0534c19ed27d2ff8c6b32d87c07bc76f'
         '280748c570711aae27732ee0c450368f'
         'a5864ab09d9c70c997e9c285f7e6a14f')

build() {
  cd "${pkgbase}-${pkgver}"

  ./boot.sh
  ./configure --prefix=/usr \
              --sysconfdir=/etc \
              --sbindir=/usr/bin \
              --localstatedir=/var \
              --with-rundir=/run/openvswitch \
              PYTHON=/usr/bin/python2
  make
}

package_openvswitch() {
  cd "${pkgbase}-${pkgver}"

  make DESTDIR="${pkgdir}" install

  install -D ${srcdir}/openvswitch.tmpfiles ${pkgdir}/usr/lib/tmpfiles.d/openvswitch.conf
  install -D ${srcdir}/ovs-vswitchd.service ${pkgdir}/usr/lib/systemd/system/ovs-vswitchd.service
  install -D ${srcdir}/ovsdb-server.service ${pkgdir}/usr/lib/systemd/system/ovsdb-server.service
  install -d -m 0770 ${pkgdir}/run/openvswitch/
}

package_openvswitch-datapath-dkms() {
  makedepends=('dkms')
  depends=('dkms' 'python2')
  install=openvswitch-datapath-dkms.install

  cd "${pkgbase}-${pkgver}"

  install -m755 -d ${pkgdir}/usr/src/${pkgbase}-${pkgver}/
  cp -R --preserve * ${pkgdir}/usr/src/${pkgbase}-${pkgver}/

  sed s/PACKAGE_VERSION\=\"\"/PACKAGE_VERSION\=\"${pkgver}\"/ \
      ${srcdir}/dkms.conf > ${pkgdir}/usr/src/${pkgbase}-${pkgver}/dkms.conf
}

# vim:set sw=2 sts=2 et
