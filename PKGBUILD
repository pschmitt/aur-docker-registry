# Maintainer: Philipp Schmitt <philipp@schmitt.co>
# GitHub: https://github.com/pschmitt/aur-docker-registry
pkgname=docker-registry
pkgver=0.8.1
pkgrel=4
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
sha512sums=('5c84a61152c59bd71467d3820f086b400c062a87457ed54e63e0c9f9c55ac12182d9858d8ad73a79caa5690df9633504af67f387335da4a4b0da065dd23675cb'
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
