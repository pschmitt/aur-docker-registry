# Maintainer: Philipp Schmitt <philipp@schmitt.co>
# GitHub: https://github.com/pschmitt/aur-docker-registry
pkgname=docker-registry
pkgver=0.9.0
pkgrel=1
pkgdesc='Registry server for Docker (hosting/delivering of repositories and images)'
arch=('any')
url='https://github.com/dotcloud/docker-registry'
license=('Apache 2.0')
depends=('python2' 'python2-gunicorn' 'libevent' 'lrzip' 'xz' 'redis'
         'python2-gevent' 'python2-flask' 'python2-grequests' 'python2-rsa'
         'python2-yaml' 'python2-redis' 'python2-blinker' 'python2-sqlalchemy'
         'python2-backports.lzma' 'python2-flask-cors')
options=(!emptydirs)
source=(
  "https://github.com/dotcloud/$pkgname/archive/$pkgver.tar.gz"
  "https://raw.githubusercontent.com/pschmitt/aur-$pkgname/master/$pkgname.service"
)
sha512sums=('9ee8612c7f165a8b1b40be0e54816ccc9ead2e8caa8757d24c4112045f3791955600dae6fa8a932ffd7f7dbe0c22ae2fe9bc2dfb0661504aafef4065a4df32f8'
            'e2971c763571f2d0596262d37535f1a93d6ab0b8f24eee16de2e30eaaaafedc2635e1c533d081198c6a4103a1837a7057b5874a973ce564461926933c6a8c582')
install="$pkgname.install"

# Change this to wherever you want to put your config files
DOCKER_REGISTRY_CONFIG_DIR=/etc/$pkgname

package() {
  # Install docker-registry-core
  cd "$srcdir/$pkgname-$pkgver/depends/docker-registry-core"
  python2 setup.py install --root="$pkgdir/" --optimize=1
  # Install docker-registry
  cd "$srcdir/$pkgname-$pkgver"
  python2 setup.py install --root="$pkgdir/" --optimize=1
  # Move config to /etc/docker-registry
  # TODO Wouldn't symlinkg config.yml to /etc/docker-registry.yml be better?
  cd "$pkgdir/usr/lib/python2.7/site-packages/"
  mkdir -p $pkgdir/etc
  mv config "${pkgdir}${DOCKER_REGISTRY_CONFIG_DIR}"
  ln -s ${DOCKER_REGISTRY_CONFIG_DIR} config
  # Install systemd service file
  install -Dm644 "$srcdir/docker-registry.service" \
                 "$pkgdir/usr/lib/systemd/system/docker-registry.service"
}

# vim:set ts=2 sw=2 et cc=80 :
