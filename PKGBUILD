# Maintainer: Artoo <artoo@artixlinux.org>
# Contributor: Christian Hesse <mail@eworm.de>
# Contributor: Dave Reisner <dreisner@archlinux.org>
# Contributor: Tom Gundersen <teg@jklm.no>

_pkgbase=systemd-stable

_alpm=1.7
_tag=255.6

pkgbase=udev
pkgname=('udev' 'libudev' 'esysusers' 'etmpfiles')
pkgdesc='Userspace device file manager'
pkgver="${_tag/~/}"
pkgrel=1
arch=('x86_64')
url='https://www.github.com/systemd/systemd'
license=(
    'GPL-2.0-or-later'
    'LGPL-2.1-or-later'
)
depends=(
    'gcc-libs'
    'glibc'
    'libcap'
)
makedepends=(
    'acl'
    'gperf'
    'hwdata'
    'kbd'
    'kmod'
    'libxcrypt'
    'util-linux'
    'docbook-xsl'
    'git'
    'intltool'
    'meson'
    'python-jinja'
    'rsync'
    'bash-completion'
)
validpgpkeys=('63CDA1E5D3FC22B998D20DD6327F26951A015CC4'  # Lennart Poettering <lennart@poettering.net>
              'A9EA9081724FFAE0484C35A1A81CEA22BC8C7E2E'  # Luca Boccassi <luca.boccassi@gmail.com>
              '9A774DB5DB996C154EBBFBFDA0099A18E29326E1'  # Yu Watanabe <watanabe.yu+github@gmail.com>
              '5C251B5FC54EB2F80F407AAAC54CA336CFEB557E') # Zbigniew Jędrzejewski-Szmek <zbyszek@in.waw.pl>
source=("git+https://github.com/systemd/systemd-stable#tag=v${_tag}" #?signed
        "git+https://github.com/systemd/systemd#tag=v${_tag%.*}" #?signed
        "git+https://gitea.artixlinux.org/artix/alpm-hooks.git#tag=${_alpm}"
        0001-Use-Arch-Linux-device-access-groups.patch
        0001-artix-standalone-install.patch
)
sha512sums=('c1de1eb0d0ef6d8da81a105cdfcb86634bed6f46ab1038de9ab786fd85f59524e7eb30fe1d02dbf2c3b3a29dc66d04a102b2274a09ad3d2c18953c380099aa0e'
            'd430427987309483c99062adb02741d25239ba5fbb97053ef817c0c5a0a935328af9c8b651de2b119b0e851dcf6623f01343859735ff81d7013ab0133e67c7ea'
            '273f53fcb219d1aa5b3d5b51b5b5b80fe32d452d8d019831a5b4340bb825f3b80177ce03ad6b797eb94585916715852a22613d400dd34ca39da2b7a3dd9ed93a'
            'c5845849a8c66cc1192b1f263098b379c983f779c9521771c6dddc5271e75e96672dce4db112895dcd43f129f884353ecbbab4103c704fbd046657f269a324e1'
            '28882b12d640938ac36b3a8a6ede1aa58185b125beb10a58c63f8ee9a18096da29b0afe31e8db04e5c37d7bd315effb75425320d96fe2b7252292b1d3e955cae')

_backports=(
)

_reverts=(
)

prepare() {
    cd "$_pkgbase"

    # add upstream repository for cherry-picking
    git remote add -f upstream ../systemd

    local _c
    for _c in "${_backports[@]}"; do
        git log --oneline -1 "${_c}"
        git cherry-pick -n "${_c}"
    done
    for _c in "${_reverts[@]}"; do
        git log --oneline -1 "${_c}"
        git revert -n "${_c}"
    done

    # Replace cdrom/dialout/tape groups with optical/uucp/storage
    patch -Np1 -i ../0001-Use-Arch-Linux-device-access-groups.patch

    patch -Np1 -i ../0001-artix-standalone-install.patch
}

build() {
    local _meson_options=() _targets=()

    local _meson_ver="${pkgver}-${pkgrel}"

    _meson_options+=(
        -Dversion-tag="${_meson_ver}-artix"
        -Dshared-lib-tag="${_meson_ver/~/}"
        -Dmode=release

        -Dstandalone-binaries=true
        -Dsysusers=true
        -Dtmpfiles=true

        -Dhwdb=true

        -Dblkid=enabled

        -Dkmod=enabled

        -Dgshadow=false

        -Dsbat-distro='artix'
        -Dsbat-distro-summary='Artix Linux'
        -Dsbat-distro-pkgname="${pkgname}"
        -Dsbat-distro-version="${pkgver}"

        -Dtests=true

        -Dlink-udev-shared=false

        -Ddefault-keymap='us'

        -Dman=enabled

        -Dhtml=disabled

        -Ddns-servers=''
        -Dntp-servers=''
        -Defi=false

        -Dsysvinit-path=
        -Dsysvrcnd-path=
        -Ddefault-dnssec=no

        -Ddefault-llmnr=no
        -Ddefault-mdns=no

        -Dadm-group=false
        -Danalyze=false

        -Dapparmor=disabled
        -Daudit=disabled
        -Dbacklight=false
        -Dbinfmt=false
        -Dbootloader=disabled
        -Dbzip2=disabled
        -Dcoredump=false
        -Ddbus=disabled
        -Delfutils=disabled
        -Denvironment-d=false
        -Dfdisk=disabled
        -Dgcrypt=disabled
        -Dglib=disabled
        -Dgnutls=disabled
        -Dhibernate=false
        -Dhostnamed=false
        -Didn=false
        -Dima=false
        -Dinitrd=false
        -Dfirstboot=false
        -Dkernel-install=false
        -Dldconfig=false
        -Dlibcryptsetup=disabled
        -Dlibcurl=disabled
        -Dlibfido2=disabled
        -Dlibidn=disabled
        -Dlibidn2=disabled
        -Dlibiptc=disabled
        -Dlocaled=false
        -Dlogind=false
        -Dlz4=disabled
        -Dmachined=false
        -Dmicrohttpd=disabled
        -Dnetworkd=false
        -Dnscd=false
        -Dnss-myhostname=false
        -Dnss-resolve=disabled
        -Dnss-systemd=false
        -Doomd=false
        -Dopenssl=disabled
        -Dp11kit=disabled
        -Dpam=disabled
        -Dpcre2=disabled
        -Dpolkit=disabled
        -Dportabled=false
        -Dpstore=false
        -Dpwquality=disabled
        -Drandomseed=false
        -Dresolve=false
        -Drfkill=false
        -Dseccomp=disabled
        -Dsmack=false
        -Dstoragetm=false
        -Dsysext=false
        -Dtimedated=false
        -Dtimesyncd=false
        -Dtpm=false
        -Dqrencode=disabled
        -Dquotacheck=false
        -Duserdb=false
        -Dutmp=false
        -Dvconsole=false
        -Dvmspawn=disabled
        -Dwheel-group=false
        -Dxdg-autostart=false
        -Dxkbcommon=disabled
        -Dxz=disabled
        -Dzlib=disabled
        -Dzstd=disabled

        -Dbpf-framework=disabled
        -Dpasswdqc=disabled
        -Dselinux=disabled
        -Dxenctrl=disabled
        -Dlibcryptsetup-plugins=disabled
        -Drepart=disabled
        -Dsysupdate=disabled
        -Dimportd=disabled
        -Dhomed=disabled
        -Dremote=disabled
        -Dnss-mymachines=disabled
        -Dukify=disabled
        -Dtpm2=disabled
    )

    artix-meson "$_pkgbase" build "${_meson_options[@]}"

    _targets+=(
        udev:shared_library
        src/libudev/libudev.pc

        udevadm
        {ata_id,cdrom_id,dmi_memory_id,fido_id,iocost,mtd_probe,scsi_id,v4l_id}
        rules.d/{50-udev-default,60-persistent-storage,64-btrfs}.rules
        udev-hwdb
        hwdb.d/60-autosuspend-chromiumos.hwdb
        src/udev/udev.pc
        man/{libudev.3,udev.conf.5,hwdb.7,udev.7,udevadm.8,iocost.conf.5,systemd-hwdb.8}

        esysusers
        etmpfiles
        sysusers.d/basic.conf
        tmpfiles.d/{etc,static-nodes-permissions,var,legacy}.conf
        man/{sysusers,tmpfiles}.d.5
        man/systemd-{sysusers,tmpfiles}.8
        factory/templates/{locale,vconsole}.conf

        systemd-detect-virt
        systemd-runtest.env

        test-libudev
        test-libudev-sym

        test-udev-builtin
        test-udev-device-thread
        test-udev-format
        test-udev-manager
        test-udev-node
        test-udev-rules
        test-udev-spawn
        #test-udev-util

        test-fido-id-desc
        test-link-config-tables
    )
    meson compile -C build "${_targets[@]}"
}

check() {
    local _tests=()
    _tests+=(
        udev-rules-check
        hwdb-test

        test-libudev
        test-libudev-sym

        test-udev
        test-udev-builtin
        test-udev-device-thread
        test-udev-format
        test-udev-manager
        test-udev-node
        test-udev-rules
        test-udev-spawn
        #test-udev-util

        test-fido-id-desc
        test-link-config-tables

        dmidecode_HP-Z600
        dmidecode_Lenovo-ThinkPad-X280
        dmidecode_Lenovo-Thinkcentre-m720s


        test-systemd-tmpfiles.standalone
        test-sysusers.standalone

        #test-tmpfile-util
        #test-offline-passwd
    )

    meson test -C build --print-errorlogs "${_tests[@]}"
}

_inst_man() {
    local x="$1" y=${1##*.} man
    install -d "${pkgdir}"/usr/share/man/man"$y"
    case "$x" in
        *sysusers*|*tmpfiles*) man=${x/systemd-/e} ;;
        *) man=${x/systemd/udev} ;;
    esac
    install -vm644 build/man/"$x" "${pkgdir}"/usr/share/man/man"$y/$man"
}

package_udev() {
    pkgdesc='Userspace device file manager'
    depends+=(
        'acl' 'libacl.so'
        'bash'
        'hwdata'
        'kbd'
        'kmod' 'libkmod.so'
        'libcap.so'
        'libudev'
        'util-linux' 'libblkid.so'
    )
    backup=(etc/udev/iocost.conf
            etc/udev/udev.conf)

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags udev,udev-devel

    for m in udev.conf.5 iocost.conf.5 udev.7 udevadm.8 systemd-hwdb.8; do
        _inst_man "$m"
    done

    # pacman hooks
    make -C alpm-hooks DESTDIR="${pkgdir}" install_udev
}

package_libudev() {
    pkgdesc='udev library for enumerating and introspecting local devices'
    depends+=(
        'libcap.so'
    )
    provides=('libudev.so')

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags libudev,libudev-devel

    _inst_man "libudev.3"
}

package_esysusers() {
    pkgdesc='the sysusers.d binary'
    depends+=(
        'libcap.so'
        'libxcrypt' 'libcrypt.so'
    )

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags esysusers

    _inst_man "sysusers.d.5"
    _inst_man "systemd-sysusers.8"

    # pacman hooks
    make -C alpm-hooks DESTDIR="${pkgdir}" install_sysusers
}

package_etmpfiles() {
    pkgdesc='the tmpfiles.d binary'
    depends+=(
        'acl' 'libacl.so'
        'libcap.so'
    )

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags etmpfiles

    _inst_man "tmpfiles.d.5"
    _inst_man "systemd-tmpfiles.8"

    # pacman hooks
    make -C alpm-hooks DESTDIR="${pkgdir}" install_tmpfiles
}
