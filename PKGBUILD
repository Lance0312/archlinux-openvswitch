# Maintainer: Lance Chen <cyen0312+aur@gmail.com>

pkgbase=openvswitch
pkgname=('openvswitch-git' 'openvswitch-datapath-git-dkms')
pkgver=0.0.0
pkgrel=1
pkgdesc="An Open Virtual Switch"
arch=('i686' 'x86_64')
url="http://openvswitch.org/"
license=('Apache')
depends=('python2')
makedepends=('python2')
source=("${pkgbase}::git+http://git.openvswitch.org/git/openvswitch"
        "dkms.conf"
        "openvswitch.tmpfiles"
        "ovs-vswitchd.service"
        "ovsdb-server.service")
md5sums=('SKIP'
         '6dfb9ec230994864a9da88a7d288a254'
         '0534c19ed27d2ff8c6b32d87c07bc76f'
         '280748c570711aae27732ee0c450368f'
         'a5864ab09d9c70c997e9c285f7e6a14f')

pkgver() {
  cd "${pkgbase}"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "${pkgbase}"

  ./boot.sh
  ./configure --prefix=/usr \
              --sysconfdir=/etc \
              --sbindir=/usr/bin \
              --localstatedir=/var \
              --with-rundir=/run/openvswitch \
              PYTHON=/usr/bin/python2
  make
}

package_openvswitch-git() {
  cd "${pkgbase}"

  make DESTDIR="${pkgdir}" install

  install -D ${srcdir}/openvswitch.tmpfiles ${pkgdir}/usr/lib/tmpfiles.d/openvswitch.conf
  install -D ${srcdir}/ovs-vswitchd.service ${pkgdir}/usr/lib/systemd/system/ovs-vswitchd.service
  install -D ${srcdir}/ovsdb-server.service ${pkgdir}/usr/lib/systemd/system/ovsdb-server.service
  install -d -m 0770 ${pkgdir}/run/openvswitch/
}

package_openvswitch-datapath-git-dkms() {
  makedepends=('dkms')
  depends=('dkms' 'python2')
  install=openvswitch-datapath-dkms.install

  cd "${pkgbase}"

  install -m755 -d ${pkgdir}/usr/src/${pkgbase}-${pkgver}/
  cp -R --preserve * ${pkgdir}/usr/src/${pkgbase}-${pkgver}/

  sed s/PACKAGE_VERSION\=\"\"/PACKAGE_VERSION\=\"${pkgver}\"/ \
      ${srcdir}/dkms.conf > ${pkgdir}/usr/src/${pkgbase}-${pkgver}/dkms.conf
}

# vim:set sw=2 sts=2 et
