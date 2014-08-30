# Maintainer: Stefan Berggren <nsg@nsg.cc>

pkgname=heimdal-krb-client
pkgver=1.6rc2
pkgrel=1
pkgdesc="The Heimdal Kerberos network authentication system client tools"
arch=('x86_64')
url="http://www.h5l.org/"
license=('custom')
depends=('sqlite3' 'libldap')
makedepends=()
provides=('heimdal-krb-client')
conflicts=('') # krb5 really, but my package lib64-krb5 provides "krb5" so I can't add this
backup=('etc/krb5.conf')
source=("http://www.h5l.org/dist/src/heimdal-${pkgver}.tar.gz"
        "http://www.h5l.org/dist/src/heimdal-${pkgver}.tar.gz.asc"
        "version-script-client.map")
md5sums=('152ca82660864acf7f712ae1a57e6795'
         'd1486e268e99cfe0ebb21f246930f2da'
         '42a2d89f813ea1f8343276ec603ed07e')
prepare() {
  tar xf heimdal-${pkgver}.tar.gz
}

build() {
  cd heimdal-${pkgver}
  ./configure --prefix=/usr \
              --sysconfdir=/etc \
              --libdir=/usr/lib/heimdal \
              --enable-shared
  cp ../version-script-client.map lib/kadm5/
  local num_cpu=$(grep -c processor /proc/cpuinfo)
  if [ -z $num_cpu ]; then
    make
  else
    make -j $num_cpu
  fi
}

x_check() {
  mkdir -p fake_python2
  echo -e "#!/bin/bash\n\nexec python2 \"\$@\"" > fake_python2/python
  chmod +x fake_python2/python
  export PATH=$PWD/fake_python2:$PATH
  cd krb5-${pkgver}/src
  msg "Using $(python --version 2>&1) from $(type python | awk '{print $NF}')"
  make check
}

package() {
  cd heimdal-${pkgver}
  make DESTDIR="${pkgdir}" install


#  install -Dm644 "${srcdir}"/krb5-${pkgver}/NOTICE "${pkgdir}"/usr/share/licenses/${pkgname}/LICENSE
#  install -dm 755 "${pkgdir}"/usr/share/aclocal
#  install -m 644 util/ac_check_krb5.m4 "${pkgdir}"/usr/share/aclocal
  if [ -z $pkgdir ]; then
    error "pkgdir is empty, abort!"
    exit 1
  fi

  # Prefere to keep the normal su, ftp, ... in PATH
  msg "Rename binaries"
  for p in su ftp login telnet rcp rsh; do
    mv -v "${pkgdir}"/usr/bin/{,k}$p
  done

  msg "Update man pages"
  for p in "${pkgdir}"/usr/share/man/*/{su,ftp,login,telnet,ftpusers,rcp,rsh}.*; do
    ( cd $(dirname $p) && mv -v {,k}$(basename $p) )
  done

  # Just because I'm so used to type kpagsh
  msg "Create symlink for kpagsh"
  ( cd "${pkgdir}"/usr/bin/ && ln -sv pagsh kpagsh )

  msg "Remove server only binaries"
  rm -rfv "${pkgdir}"/usr/libexec/
  rm -rfv "${pkgdir}"/usr/sbin/iprop-log

  msg "Remove man pages"
  for p in "${pkgdir}"/usr/share/man/*/{iprop,ftpd,hprop,hpropd,ipropd-master,ipropd-slave,kadmind,kcm,kdc,kdigest,kfd,kimpersonate,kpasswdd,kxd,popper,push,rshd,telnetd}.*; do
    rm -v $p
  done

  install -D -m644 ${srcdir}/heimdal-${pkgver}/krb5.conf ${pkgdir}/etc/krb5.conf
  install -D -m644 ${srcdir}/heimdal-${pkgver}/LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
}
