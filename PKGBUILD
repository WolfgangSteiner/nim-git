# Maintainer: Dan Beste  <dan.ray.beste+aur@gmail.com>
# Contributor: Andy Weidenbaum <archbaum@gmail.com>
# Contributor: Alexander F Rødseth <xyproto@archlinux.org>
# Contributor: Dominik Picheta <morfeusz8@gmail.com>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Jesus Alvarez <jeezusjr@gmail.com>
# Contributor: Zion Nimchuk <zionnimchuk@gmail.com>

pkgbase='nim'
pkgname=('nim' 'nimble' 'nimsuggest' 'nimpretty')
pkgdesc='Nim is a compiled, garbage-collected systems programming language with a design that focuses on efficiency, expressiveness, and elegance (in that order of priority).'
epoch=1
pkgver=1.0.0
pkgrel=1
arch=('i686' 'x86_64')
groups=('nim')
makedepends=('git')
source=(
  "https://github.com/nim-lang/Nim/archive/v$pkgver.tar.gz"
)
sha256sums=(
  '6d93d25da6b5ef4e0223acb1f1abadf06be1019a8137491ddc7c6fa030e638c3'
)

build() {
  cd Nim-$pkgver
  sh build_all.sh
  cd -
}

package_nim() {
  url="https://nim-lang.org/"
  license=('MIT')
  options=('!emptydirs')
  provides=('nim')
  conflicts=('nim')

  cd Nim-$pkgver

  # License
  install -dm 755 "${pkgdir}/usr/share/licenses/nim"
  install -m 644 "copying.txt" "${pkgdir}/usr/share/licenses/nim/LICENSE"

  # Docs
  install -dm 755 "${pkgdir}/usr/share/doc/nim"
  cp -dpr --no-preserve=ownership \
    examples doc/*                \
    -t "${pkgdir}/usr/share/doc/nim"

  # Nim
  ./koch install "${pkgdir}"
  install -Dm 755 bin/{nim,nimgrep} -t "$pkgdir/usr/bin"

  # Bash completion
  install -Dm 644          \
    tools/nim.bash-completion \
    "${pkgdir}/usr/share/bash-completion/completions/nim"


  cd "${pkgdir}/nim"
  install -dm 755 "${pkgdir}"/{etc/nim,usr/lib/nim}
  find lib -mindepth 1 -maxdepth 1 -exec \
    cp -dpr --no-preserve=ownership '{}' -t "$pkgdir/usr/lib/nim" \;
  find config -mindepth 1 -maxdepth 1 -exec \
    cp -dpr --no-preserve=ownership '{}' -t "$pkgdir/etc/nim" \;
  cp -dpr --no-preserve=ownership \
    "$srcdir/Nim-$pkgver/lib/packages"    \
    -t "$pkgdir/usr/lib/nim"

  # Workaround Nim's nonstandard header file placement
  # (https://bugs.archlinux.org/task/50252):
  install -dm 755 "${pkgdir}/usr/include"
  for _header in cycle nimbase; do
    ln -s "/usr/lib/nim/${_header}.h" "$pkgdir/usr/include/${_header}.h"
  done

  # Fix unusual placement of shared object files:
  ln -s "/usr/lib/nim/libnimrtl.so" "$pkgdir/usr/lib/libnimrtl.so"

  # Clean up $pkgdir
  rm -rf "$pkgdir/nim"
  find "$pkgdir" -type d -name .git -exec rm -r '{}' +
  find "$pkgdir" -type f -name .gitignore -exec rm -r '{}' +

}

package_nimble() {
  pkgdesc="Package manager for the Nim programming language"
  url="https://github.com/nim-lang/nim"
  license=('BSD')
  provides=('nimble')
  conflicts=('nimble')

  cd Nim-$pkgver

  # License
  install -dm 755 "${pkgdir}/usr/share/licenses/nimble"
  install -Dm 644 dist/nimble/license.txt "${pkgdir}/usr/share/licenses/nimble/LICENSE"

  # Docs
  install -Dm 644 dist/nimble/*.markdown -t "$pkgdir/usr/share/doc/nimble"

  # Nimble
  install -Dm 755 "bin/nimble" -t "$pkgdir/usr/bin"

  # Nimble looks for nimscriptapi.nim in /usr/bin/nimblepkg/, of all places.
  install -dm 755 "$pkgdir/usr/share/nimble"
  cp -dpr --no-preserve=ownership dist/nimble/src/nimblepkg "$pkgdir/usr/share/nimble"
  ln -s "/usr/share/nimble" "$pkgdir/usr/bin/nimblepkg"

  # Bash completion
  install -Dm 644          \
    dist/nimble/nimble.bash-completion \
    "${pkgdir}/usr/share/bash-completion/completions/nimble"
}

package_nimsuggest() {
  pkgdesc="Nimsuggest is a tool that helps to give editors IDE like capabilities."
  url="https://github.com/nim-lang/nim"
  license=('MIT')
  provides=('nimsuggest')
  conflicts=('nimsuggest')

  install -Dm 755 "Nim-$pkgver/bin/nimsuggest" -t "${pkgdir}/usr/bin"
}

package_nimpretty() {
  pkgdesc="Standard tool for pretty printing."
  url="https://github.com/nim-lang/nim"
  license=('MIT')
  provides=('nimpretty')
  conflicts=('nimpretty')

  install -Dm 755 "Nim-$pkgver/bin/nimpretty" -t "${pkgdir}/usr/bin"
}

# vim: sw=2 ts=2 et:
