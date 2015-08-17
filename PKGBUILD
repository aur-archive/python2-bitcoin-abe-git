# Maintainer: Duncan Townsend <duncant@mit.edu>
pkgname=python2-bitcoin-abe-git
pkgver=20130317
pkgrel=1
pkgdesc="Abe: a free block chain browser for Bitcoin-based currencies."
arch=('any')
url="https://github.com/jtobey/bitcoin-abe"
license=('AGPL3')
groups=()
depends=('python2' 'python2-crypto' 'bitcoin-daemon>=0.7')
makedepends=('git' 'python2-distribute')
optdepends=('python2-psycopg2: python driver for postgresql backend'
            'mysql-python: python driver for mysql backend'
            'python2-sqlite: python driver for sqlite backend')
provides=("python2-bitcoin-abe=0.6" "python2-bitcoin-abe=0.7pre_fb1")
conflicts=('python2-bitcoin-abe')
replaces=()
backup=('etc/abe/abe.conf')
options=(!emptydirs)
install='python2-bitcoin-abe-git.install'
source=()

_gitroot='git://github.com/jtobey/bitcoin-abe.git'
_gitname='bitcoin-abe'

# change this if you'd like to build a different branch
_gitbranch='master'

build() {
    cd "$srcdir"
    msg "Connecting to GIT server...."

    if [[ -d "$_gitname" ]]; then
        cd "$_gitname" && git pull origin $_gitbranch
        msg "The local files are updated."
    else
        git clone -b $_gitbranch "$_gitroot" "$_gitname"
    fi

    msg "GIT checkout done or server timeout"
    msg "Starting build..."

    rm -rf "$srcdir/$_gitname-build"
    git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
    cd "$srcdir/$_gitname-build"
    sed -i -e 's#/usr/bin/env python#/usr/bin/env python2#' \
        contrib/ecdsa.py Abe/upgrade.py Abe/base58.py Abe/abe.py \
        Abe/firstbits.py Abe/mixup.py Abe/reconfigure.py Abe/verify.py \
        tools/namecoin_dump.py
}

package() {
    cd "$srcdir/$_gitname-build"
    python2 setup.py install --root="$pkgdir/" --optimize=1

    DOC_DIR="${pkgdir}/usr/share/doc/${pkgname%-git}"
    install -d "${DOC_DIR}"
    install -m644 doc/FAQ.html "${DOC_DIR}/"
    install -m644 README-FASTCGI.txt "${DOC_DIR}/"
    install -m644 README-FIRSTBITS.txt "${DOC_DIR}/"
    install -m644 README-MYSQL.txt "${DOC_DIR}/"
    install -m644 README-POSTGRES.txt "${DOC_DIR}/"
    install -m644 README-SQLITE.txt "${DOC_DIR}/"
    install -m644 README.md "${DOC_DIR}/"

    CONF_DIR="${pkgdir}/etc/abe"
    install -d "${CONF_DIR}"
    install -m644 abe.conf "${CONF_DIR}/"
}
