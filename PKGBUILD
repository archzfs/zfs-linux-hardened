# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
# ! WARNING !
#
# The archzfs packages are kernel modules, so these PKGBUILDS will only work with the kernel package they target. In this
# case, the archzfs-linux-hardened packages will only work with the default linux-hardened package! To have a single PKGBUILD target
# many kernels would make for a cluttered PKGBUILD!
#
# If you have a custom kernel, you will need to change things in the PKGBUILDS. If you would like to have AUR or archzfs repo
# packages for your favorite kernel package built using the archzfs build tools, submit a request in the Issue tracker on the
# archzfs github page.
#
pkgbase="zfs-linux-hardened"
pkgname=("zfs-linux-hardened" "zfs-linux-hardened-headers")
_zfsver="2.1.9"
_kernelver="6.1.13.hardened1-1"
_kernelver_full="6.1.13.hardened1-1"
_extramodules="${_kernelver/.hardened/-hardened}-hardened"

pkgver="${_zfsver}_$(echo ${_kernelver} | sed s/-/./g)"
pkgrel=1
makedepends=("linux-hardened-headers=${_kernelver}")
arch=("x86_64")
url="https://openzfs.org/"
source=("https://github.com/openzfs/zfs/releases/download/zfs-${_zfsver}/zfs-${_zfsver}.tar.gz")
sha256sums=("6b172cdf2eb54e17fcd68f900fab33c1430c5c59848fa46fab83614922fe50f6")
license=("CDDL")
depends=("kmod" "zfs-utils=${_zfsver}" "linux-hardened=${_kernelver}")

build() {
    cd "${srcdir}/zfs-${_zfsver}"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --libdir=/usr/lib \
                --datadir=/usr/share --includedir=/usr/include --with-udevdir=/usr/lib/udev \
                --libexecdir=/usr/lib --with-config=kernel \
                --with-linux=/usr/lib/modules/${_extramodules}/build \
                --with-linux-obj=/usr/lib/modules/${_extramodules}/build
    make
}

package_zfs-linux-hardened() {
    pkgdesc="Kernel modules for the Zettabyte File System."
    install=zfs.install
    provides=("zfs" "spl")
    groups=("archzfs-linux-hardened")
    conflicts=("zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc" "spl-dkms" "spl-dkms-git" 'zfs-linux-hardened-git' 'spl-linux-hardened')
    replaces=("spl-linux-hardened")
    cd "${srcdir}/zfs-${_zfsver}"
    make DESTDIR="${pkgdir}" INSTALL_MOD_PATH=${pkgdir}/usr INSTALL_MOD_STRIP=1 install
    # Remove src dir
    rm -r "${pkgdir}"/usr/src
}

package_zfs-linux-hardened-headers() {
    pkgdesc="Kernel headers for the Zettabyte File System."
    provides=("zfs-headers" "spl-headers")
    conflicts=("zfs-headers" "zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc" "spl-dkms" "spl-dkms-git" "spl-headers")
    cd "${srcdir}/zfs-${_zfsver}"
    make DESTDIR="${pkgdir}" install
    rm -r "${pkgdir}/lib"
    # Remove reference to ${srcdir}
    sed -i "s+${srcdir}++" ${pkgdir}/usr/src/zfs-*/${_extramodules}/Module.symvers
}
