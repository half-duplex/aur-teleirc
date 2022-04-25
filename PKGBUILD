# Maintainer: Andrea Denisse Gómez-Martínez <aur at denisse dot dev>

pkgname=teleirc
pkgdesc='Go implementation of a Telegram <=> IRC bridge for use with any IRC channel and Telegram group.'
arch=(any)
url='https://github.com/RITlug/teleirc'
_branch='master'
pkgver=2.2.0
pkgrel=2
license=('GPL3')
makedepends=(go)
source=(
    "${pkgname}-${pkgver}.tar.gz::${url}/archive/v${pkgver}.tar.gz"
    "teleirc.sysusers"
    "teleirc.tmpfiles"
    "teleirc@.service"
)
sha256sums=('63c870e4a1f1c0625ee7afa0f4bac084429d58da73c106a32ffd74ef609e1310'
            '54943849e0f27f96d6ad189993e42fecf2f33085f9974ea55a5a0adb664ba016'
            'd7a82d9af2e9841f32c2c646b964911a92d3d9d17d50b30a12fcc24dcfe4831f'
            'd6a6caab72fbc63b5b03a77985ddf8e5412dcd1d8b36d5f5ee9a901cc9f942a5')
sha512sums=('161c7f7b19a0d4e21f5aaf33d3f99eb1bfdbf9c2a591dfda456d932825408b2cbddd630f762512eb8a27a3b8c6ed676d608afa2a8e0817cf80ad684dc080eaa9'
            'f84d502ec9a993a312195aa345b4ee400926c9ab64a5301b9525a102a1ffe0d10aefd277d197a333fc2cf6d009beec32ac8b133f692ff70c9eecccdba74db94d'
            'ab8667186c4d4265db6fadf03d9c53da4c431ade5d22fa0c8159c56e37b60d520bc59f76abcabcb48fcec628ef748b22bbd93d9ab195ac68052e1c61ff25bff4'
            'd9a03e9d6de1c9343eb271a050d086467ca3858b847d64dc9a59c8d6bbed892a452450da9a54aa40f89ede4ab7ae0e710568c1c9f8697e15e203cb09270b547b')
b2sums=('8fd9c0aa66c94eabc919093df2bc5f4d2ef1ed4b16bba5c283f2d5de22e25f9d642219ef84216bb2ba016e0145728664c15b3522452f85d7a775731df6e861c6'
        '037f6d97854abffa4bc4eb527910dc5d4bb4e40dedc66e44a956d56b1f28d9e5a66931ae6b3a3bf23e7e2b19d3c5bac96df6aa9ce8e8e7627d9e8d15c3eed03c'
        '96661e672c6497fbd3dfac46d09df803a9c8ff3da4228cac9a9cd8a8a7b8922b27aa3cabb9c1b4ea9d715545f3a78b5c34f8d517a7684ac430a2f90bb0349dff'
        '742fa1a55a24c3ea8c49ba4bb63ff8463a4b4b17aa2ba87f9d8949d7a7675468230fb8f2d21955346e2739ee249486ff8c2c9d1dad39e2f71a07884f12369741')
provides=($pkgname)
conflicts=($pkgname)

prepare() {
  mkdir -p "${pkgname}-${pkgver}/build"
}

build() {
  cd "${pkgname}-${pkgver}"

  export CGO_LDFLAGS="${LDFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export GOFLAGS="-buildmode=pie -trimpath -ldflags=-linkmode=external -mod=readonly -modcacherw"

  go build -o build/ cmd/$pkgname.go
}

check() {
  cd "${pkgname}-${pkgver}"
  go test ./...
}

package() {
  cd "${pkgname}-${pkgver}"
  install -dm550 "$pkgdir"/etc/teleirc/
  install -Dm644 "$srcdir"/teleirc@.service "$pkgdir"/usr/lib/systemd/system/teleirc@.service
  install -Dm644 "$srcdir"/teleirc.sysusers "$pkgdir"/usr/lib/sysusers.d/teleirc.conf
  install -Dm644 "$srcdir"/teleirc.tmpfiles "$pkgdir"/usr/lib/tmpfiles.d/teleirc.conf
  install -Dm644 env.example "$pkgdir"/etc/teleirc/example
  install -Dm755 build/$pkgname "$pkgdir"/usr/bin/$pkgname
}
