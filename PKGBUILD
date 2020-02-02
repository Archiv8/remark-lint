# shellcheck shell=bash
# Maintainer: Archiv8 <archiv8@artisteducator.com>
# Contributor: Archiv8 <archiv8@artisteducator.com>

_relname="remark-lint"

# pkgbase=
pkgname="nodejs-${_relname}"
pkgver=6.0.5
pkgrel=1
# epoch=
pkgdesc="Command line interface for remark, a markdown processor powered by plugins."
arch=("any")
url="https://github.com/remarkjs/remark-lint"
license=("MIT")
# groups=()
depends=("nodejs")
# optdepends=()
makedepends=("jq" "npm")
# checkdepends=()
# provides=()
# conflicts=()
# replaces=()
# backup=()
# options=()
# install=
changelog="CHANGELOG.md"
source=(
"https://registry.npmjs.org/$_relname/-/$_relname-$pkgver.tgz"
"CC-by-SA-v4.md"
"CHANGELOG.md"
"ISSUES.md"
"LICENSE"
"LICENSE.md"
"README.md"
)
# noextract=()
# validpgpkeys=()
sha256sums=('42929e6e1d0fbd2a1cf78ffc61b9ab0f28be3dccd6fd927bcae53efaf303e76c'
            '7f7239991e69ac06287d906ab892b5b53f5e61442c3d3544a34aa1c135f9e0ce'
            '86f91538f3bcd7ebee745307ecf0e8b55cfec05c6076e1cff186a07f4a97012e'
            '65a4dc155f621ebbc7703fb3961ec07f5dadce1494d3d811917c0c2c76539e4e'
            'ed52f87c3904a045931ff47fef50c31840b2f80f3618ce7a9d188918a9f83357'
            '793d6a89bd8684b58c6d26c5212f0863ddb33c962afde7108a68b28e4388a9f8'
            'b417d1ec977ca0e9788174c68b88408a45ea63d7ccf3ab19ae42310cf0949cbd')

# prepare () {}

# build() {}

# check() {}

package() {
  # Ensure cache is set when install is run in order to avoid littering user's home directory
  printf "\e[1;32m==>\e[0m \e[38;5;248mBuilding nodejs package... \e[0m\n"
  printf "    This may take some time depending on the size of the package and number of dependencies to download... \e[0m\n"
  npm install --cache "$srcdir/npm-cache" -g --user root --prefix "$pkgdir"/usr "$srcdir"/$_relname-$pkgver.tgz

  # Fix incorrect user / group as highlighted by namcap
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible ownership issues\e[0m\n"
  while IFS= read -r fsitem; do
    chown -h root:root "$fsitem"
  done <   <(find "$pkgdir" -not -group root -not -user root)

  # Non-deterministic race in npm gives 777 permissions to random directories.
  # See https://github.com/npm/npm/issues/9359 for details.
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible permission issues on directories\e[0m\n"
  find "$pkgdir/usr" -type d -exec chmod 755 '{}' +

  # Remove references to $pkgdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$pkgdir\e[0m\n"
  find "$pkgdir" -type f -name package.json -print0 | xargs -0 sed -i "/_where/d"

  # Remove references to $srcdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$srcdir \e[0m\n"
  local tmppackage="$(mktemp)"
  local pkgjson="$pkgdir/usr/lib/node_modules/$_relname/package.json"
  jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
  mv "$tmppackage" "$pkgjson"
  chmod 644 "$pkgjson"

  # Install license
  install -Dm 644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # Create Archiv8 documentation folder
  install -dvm 755 "$pkgdir/usr/share/doc/$pkgname/packaging/"

  # Install Archiv8 Documentation
  install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CC-by-SA-v4.md"
  install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
  install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
  install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/doc/$pkgname/packaging/LICENSE.md"
  install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
  }
