# Maintainer: Ryan Davies <ryan@ryandavies.co.nz>
# Contributor: Greg Sutcliffe <greg.sutcliffe@gmail.com>>

pkgname=foreman-proxy
pkgver=1.6.0
pkgrel=0.3
_version=1.6.0
pkgdesc="Manages DNS, DHCP, TFTP and Puppet though a HTTP Restful API. Used by foreman"
arch=('any')
url="http://theforeman.org"
license=('GPL3')
depends=('iputils' 'ruby1.9' 'ruby1.9-bundler')
conflicts=('foreman-proxy-git')
backup=(
	"etc/foreman-proxy/settings.yml"
	"etc/foreman-proxy/settings.d/bmc.yml"
	"etc/foreman-proxy/settings.d/chef.yml"
	"etc/foreman-proxy/settings.d/dhcp.yml"
	"etc/foreman-proxy/settings.d/dns.yml"
	"etc/foreman-proxy/settings.d/puppet.yml"
	"etc/foreman-proxy/settings.d/puppetca.yml"
	"etc/foreman-proxy/settings.d/realm.yml"
	"etc/foreman-proxy/settings.d/tftp.yml"
)
options=(emptydirs)
install="foreman-proxy.install"
source=(https://github.com/theforeman/smart-proxy/archive/${_version}.tar.gz
        foreman-proxy.systemd
        foreman-proxy.tmpfiles.conf)
noextract=()
md5sums=('0d20aabd393cf38e24346187b3ad2034'
         '5ceb8e3a2a35ac714f0016ed317cbf46'
         'f6d26c35bf3b9a7c71105e72053785a1')

package() {
  cd $srcdir/smart-proxy-${_version}

  mkdir -p ${pkgdir}/etc/foreman-proxy

  # Main codebase
  install -d -m0755 ${pkgdir}/usr/share/foreman-proxy/
  cp -r ./ ${pkgdir}/usr/share/foreman-proxy/

  # Move settings.d to /etc/foreman-proxy
  mv ${pkgdir}/usr/share/foreman-proxy/config/settings.d ${pkgdir}/etc/foreman-proxy/settings.d

  rename ".yml.example" ".yml" ${pkgdir}/etc/foreman-proxy/settings.d/*

  # Symlink config file to etc
  install -Dp -m0644 config/settings.yml.example ${pkgdir}/etc/foreman-proxy/settings.yml
  ln -sv /etc/foreman-proxy/settings.yml ${pkgdir}/usr/share/foreman-proxy/config/settings.yml
  ln -sv /etc/foreman-proxy/settings.d ${pkgdir}/usr/share/foreman-proxy/config/settings.d

  echo 'gem "puppet"' > ${pkgdir}/usr/share/foreman-proxy/bundler.d/aur.rb

  # logdirs
  install -d -m0755 ${pkgdir}/usr/share/foreman-proxy/logs
  install -d -m0755 ${pkgdir}/var/log/foreman-proxy

  # systemd
  install -Dm 644 $srcdir/foreman-proxy.systemd       ${pkgdir}/usr/lib/systemd/system/foreman-proxy.service
  install -Dm 644 $srcdir/foreman-proxy.tmpfiles.conf ${pkgdir}/usr/lib/tmpfiles.d/foreman-proxy.conf

}

# vim:set ts=2 sw=2 et:
