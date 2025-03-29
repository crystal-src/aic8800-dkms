# Maintainer: Franck Duriez <franck@duriez.info>
pkgname=aic8800-dkms
pkgver=1.0.6
pkgrel=2
pkgdesc="Kernel modules for BrosTrend AX300 WiFi 6"
arch=('any')
url="https://www.brostrend.com/products/ax5l"
license=('GPLv2')
source=(
  'https://linux.brostrend.com/aic8800-dkms.deb'
  '0001-Make-CONFIG_RFTEST-n-valid.patch'
  '0002-Fix-DKMS-config.patch'
  '0003-Fix-kernel-logs.patch'
  '0004-Fix-build-on-6.12.patch'
  '0005-Fix-build-on-6.13.patch'
)
sha512sums=('b5bfb20d5ad128f66c923c5ee278acbcf29b406c17efe5cdd2b1e5838f44559f5e7e36926ef6eaa6c57df0183f5b26cbae42b9e34b62b0471193e2188e675a9c'
            'fd90733cc6d1f72215ce5e11c34ace01eab307a1e6dd062b3883dda5b533371ef6a2475c7eaced900760976ffc38823b196f17285485ae56c6cd6dbe3cb6a1f2'
            'f09455fa679989a6f2bc8de047d143e4660595550c07ec0bea221dbf8398ebe27224bc1f7eb4dcbafdcaa6f8bb63b7ff32a6ddf29df24a1fc67d08bde8f16b51'
            '74cc351b6d7f173847d8541c2fb0ec5b17cdf01450fefc39c1bdd2008ce06868764b638e5402f460f612d8b273593142e864278323d1db7bf5793b34b6f0b550'
            '76c3cc172c7502c737dfcf00b7f1a707cc4068f723561db706ffb4533a3101fef81f85838c8ad629bbfabc43482219a2d96f445ba1eded0aa1b63ba594fe8a58'
            '01b26ef55bcb407edd358ae8fcc29827132361b3aea171c28d4377fd0824f4fb19bd73ccfe98a920d8cac8ff2fccec770f562771b2a1aad2bdbc64485bcae5b2')
depends=('dkms')
makedepends=('dos2unix')

prepare() {
  cd "${srcdir}"
  tar xvzf "data.tar.gz"
  cd "usr/src/aic8800-${pkgver}"
  patch -Np1 -i ../../../../0001-Make-CONFIG_RFTEST-n-valid.patch -d .
  patch -Np1 -i ../../../../0002-Fix-DKMS-config.patch -d .
  patch -Np1 -i ../../../../0003-Fix-kernel-logs.patch -d .
  patch -Np1 -i ../../../../0004-Fix-build-on-6.12.patch -d .
  dos2unix aic_load_fw/aic_bluetooth_main.c
  patch -Np1 -i ../../../../0005-Fix-build-on-6.13.patch -d .
}

build() {
  cd "${srcdir}"
}

package() {
  # Copy udev rules
  install -dm 755 "${pkgdir}/usr/lib/udev/rules.d"
  install -m 644 "${srcdir}/lib/udev/rules.d/aic.rules" "${pkgdir}/usr/lib/udev/rules.d/"

  # Copy device firmware
  install -dm 755 "${pkgdir}/usr/lib/firmware"
  cp -dr --no-preserve=ownership "${srcdir}/lib/firmware/aic8800DC" "${pkgdir}/usr/lib/firmware"

  # Copy source and dkms config
  install -dm 755 "${pkgdir}/usr/src/"
  cp -dr --no-preserve=ownership "${srcdir}/usr/src/aic8800-$pkgver" "${pkgdir}/usr/src/aic8800-$pkgver"

  # Prune unwanted files
  rm -fr "${pkgdir}/usr/src/driverctl"
}
