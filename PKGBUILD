# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Maintainer: Bernhard Landauer <bernhard[at]manjaro[dot]org>
# Maintainer: Helmut Stult <helmut[at]manjaro[dot]org>

# Arch credits:
# Maintainer : Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux58
_extramodules=extramodules-5.8-MANJARO
# don't edit here
pkgver=455.28

_nver=455
# edit here for new version
_sver=28
# edit here for new build
pkgrel=1
pkgname=$_linuxprefix-nvidia-${_nver}xx
_pkgname=nvidia
_pkgver="${_nver}.${_sver}"
pkgdesc="NVIDIA drivers for linux."
arch=('x86_64')
url="http://www.nvidia.com/"
depends=("$_linuxprefix" "nvidia-455xx-utils=${_pkgver}")
makedepends=("$_linuxprefix-headers")
groups=("$_linuxprefix-extramodules")
provides=("nvidia=$pkgver")
conflicts=("$_linuxprefix-nvidia-340xx" "$_linuxprefix-nvidia-390xx" "$_linuxprefix-nvidia-418xx"
           "$_linuxprefix-nvidia-430xx" "$_linuxprefix-nvidia-435xx" "$_linuxprefix-nvidia-440xx"
           "$_linuxprefix-nvidia-450xx")
license=('custom')
install=nvidia.install
options=(!strip)
durl="http://us.download.nvidia.com/XFree86/Linux-x86"
source=("${durl}_64/${_pkgver}/NVIDIA-Linux-x86_64-${_pkgver}-no-compat32.run")
sha256sums=('SKIP')

_pkg="NVIDIA-Linux-x86_64-${_pkgver}-no-compat32"

pkgver() {
  printf '%s' "${_pkgver}"
}

prepare() {
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}"
    # patches here
}

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
    cd "${_pkg}"/kernel
    make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
        "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia-modeset.ko" \
         "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia-modeset.ko"
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia-drm.ko" \
         "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia-drm.ko"
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia-uvm.ko" \
         "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia-uvm.ko"
    gzip "${pkgdir}/usr/lib/modules/${_extramodules}/"*.ko
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
}
