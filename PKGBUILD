# Maintainer: Artoo <artoo@artixlinux.org>
# Contributor: Christian Hesse <mail@eworm.de>
# Contributor: Dave Reisner <dreisner@archlinux.org>
# Contributor: Tom Gundersen <teg@jklm.no>

_pkgbase=systemd-stable

pkgbase=udev
pkgname=('udev' 'libudev' 'esysusers' 'etmpfiles')
pkgdesc='Userspace device file manager'
_tag='2c4171c3c4146fcb32253bfb6423b5a3ee42a553' # git rev-parse v${_tag_name}
_tag_name=254.1
pkgver="${_tag_name/-/}"
pkgrel=1
arch=('x86_64')
url='https://www.github.com/systemd/systemd'
license=('GPL2' 'LGPL2.1')
makedepends=('acl' 'libacl.so' 'gperf' 'hwdata' 'kbd' 'kmod' 'libkmod.so'
            'libcap' 'libcap.so' 'libxcrypt' 'libcrypt.so' 'util-linux' 'libblkid.so'
            'docbook-xsl' 'git' 'intltool' 'meson' 'python-jinja' 'rsync')
options=('strip')
validpgpkeys=('63CDA1E5D3FC22B998D20DD6327F26951A015CC4'  # Lennart Poettering <lennart@poettering.net>
              'A9EA9081724FFAE0484C35A1A81CEA22BC8C7E2E'  # Luca Boccassi <luca.boccassi@gmail.com>
              '9A774DB5DB996C154EBBFBFDA0099A18E29326E1'  # Yu Watanabe <watanabe.yu+github@gmail.com>
              '5C251B5FC54EB2F80F407AAAC54CA336CFEB557E') # Zbigniew Jędrzejewski-Szmek <zbyszek@in.waw.pl>
_alpm=4ace40eddd2db821ff0e410e09e4e99a98dca559
source=("git+https://github.com/systemd/systemd-stable#tag=${_tag}" #?signed
        "git+https://github.com/systemd/systemd#tag=v${_tag_name%.*}" #?signed
        '0001-Use-Arch-Linux-device-access-groups.patch'
        initcpio-{hook,install}-udev
        "git+https://gitea.artixlinux.org/artix/alpm-hooks.git#commit=${_alpm}"
#         meson-install-tags.patch
#         meson-artix.patch
#         udev-log-msg.patch
#         legacy.conf
        artix.patch
)
sha512sums=('SKIP'
            'SKIP'
            '3ccf783c28f7a1c857120abac4002ca91ae1f92205dcd5a84aff515d57e706a3f9240d75a0a67cff5085716885e06e62597baa86897f298662ec36a940cf410e'
            '32606b42856b5f3ea7f485143e532671f58986237e14c58ea5ab17383dc39a375cb6c738c8a2db9e4a8c8be88ea44a876d6bbed129cb2f5c9aa3f8228b04d927'
            '38eed28d42ac8f70bc8d1058ace35f137f7f5c972442ee14b98c2146202e0615aa584304edbd59e8608d1b6bec3cb391fc69b25393740f6eabd8fc5ad3bde64f'
            'SKIP'
            '0476c3747f388d39bf67d4d88b26158443c0f7fa9ad6d8e28587c1acbbcee9502e433894e76b4ca816448adae00f5f75a15cc113232550821ab9108b53ca26b9')

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

    patch -Np1 -i ../artix.patch

    # Replace cdrom/dialout/tape groups with optical/uucp/storage
    patch -Np1 -i ../0001-Use-Arch-Linux-device-access-groups.patch
}

build() {
    local _meson_options=()

    _meson_options+=(
        -Dversion-tag="${_tag_name/-/\~}-${pkgrel}-artix"
        -Dshared-lib-tag="${pkgver}-${pkgrel}"
        -Dmode=release

        -Dstandalone-binaries=true
        -Dsysusers=true
        -Dtmpfiles=true
        -Dhwdb=true
        -Dblkid=true
        -Dgshadow=true

        -Dsbat-distro='artix'
        -Dsbat-distro-summary='Artix Linux'
        -Dsbat-distro-pkgname="${pkgname}"
        -Dsbat-distro-version="${pkgver}"

        -Dtests=true

        -Dlink-udev-shared=false
        -Dlink-boot-shared=false
        -Dman=false

        -Ddns-servers=''
        -Dntp-servers=''
        -Defi=false

        -Dsysvinit-path=
        -Ddefault-dnssec=no

        -Dadm-group=false
        -Danalyze=false
        -Dapparmor=false
        -Daudit=false
        -Dbacklight=false
        -Dbinfmt=false
        -Dbzip2=false
        -Dcoredump=false
        -Ddbus=false
        -Delfutils=false
        -Denvironment-d=false
        -Dfdisk=false
        -Dgcrypt=false
        -Dglib=false
        -Dgnutls=false
        -Dhibernate=false
        -Dhostnamed=false
        -Didn=false
        -Dima=false
        -Dinitrd=false
        -Dfirstboot=false
        -Dkernel-install=false
        -Dldconfig=false
        -Dlibcryptsetup=false
        -Dlibcurl=false
        -Dlibfido2=false
        -Dlibidn=false
        -Dlibidn2=false
        -Dlibiptc=false
        -Dlocaled=false
        -Dlogind=false
        -Dlz4=false
        -Dmachined=false
        -Dmicrohttpd=false
        -Dnetworkd=false
        -Dnscd=false
        -Dnss-myhostname=false
        -Dnss-resolve=false
        -Dnss-systemd=false
        -Doomd=false
        -Dopenssl=false
        -Dp11kit=false
        -Dpam=false
        -Dpcre2=false
        -Dpolkit=false
        -Dportabled=false
        -Dpstore=false
        -Dpwquality=false
        -Drandomseed=false
        -Dresolve=false
        -Drfkill=false
        -Dseccomp=false
        -Dsmack=false
        -Dsysext=false
        -Dtimedated=false
        -Dtimesyncd=false
        -Dtpm=false
        -Dqrencode=false
        -Dquotacheck=false
        -Duserdb=false
        -Dutmp=false
        -Dvconsole=false
        -Dwheel-group=false
        -Dxdg-autostart=false
        -Dxkbcommon=false
        -Dxz=false
        -Dzlib=false
        -Dzstd=false
    )
    artix-meson "$_pkgbase" build "${_meson_options[@]}"

    local _targets=()
    _targets+=(
        udev:shared_library
        src/libudev/libudev.pc
        udevadm
        systemd-hwdb
        src/udev/{ata_id,cdrom_id,dmi_memory_id,fido_id,iocost,mtd_probe,scsi_id,v4l_id}
        src/udev/udev.pc
        rules.d/{50-udev-default,64-btrfs}.rules
        hwdb.d/60-autosuspend-chromiumos.hwdb
        man/{libudev.3,udev.conf.5,hwdb.7,udev.7,udevadm.8}

        systemd-{sysusers,tmpfiles}.standalone
        sysusers.d/basic.conf
        tmpfiles.d/{etc,static-nodes-permissions,var,legacy}.conf
        man/{sysusers,tmpfiles}.d.5
        man/systemd-{sysusers,tmpfiles}.8
        factory/templates/{locale,vconsole}.conf

        systemd-detect-virt
#         test/sys
#         test-udev
        test-fido-id-desc
        test-udev-builtin
        test-udev-event
        test-udev-node
        test-udev-util
        systemd-runtest.env
#         test-tmpfiles
    )
    meson compile -C build "${_targets[@]}"
}

check() {
    local tests=()
    tests+=(
        test-sysusers.standalone
        test-systemd-tmpfiles.standalone
#         test-tmpfiles
        rule-syntax-check
        test-fido-id-desc
        test-udev-builtin
        test-udev-event
        test-udev-node
        test-udev-util
#         udev-test
        test-libudev
        test-libudev-sym
        test-udev-device-thread
    )
    meson test -C build --print-errorlogs "${tests[@]}"
}

_inst_doc(){
    install -d "${pkgdir}"/usr/share/doc/"${pkgname}"
    install -vm644 "$_pkgbase"/LICENSE.* "${pkgdir}"/usr/share/doc/"${pkgname}"
}

_inst_man_udev() {
    local x="$1" y=${1##*.}
    install -d "${pkgdir}"/usr/share/man/man"$y"
    install -vm644 build/man/"$x" "${pkgdir}"/usr/share/man/man"$y"
}

_inst_man_utils() {
    local u="$1"
    install -d "${pkgdir}"/usr/share/man/man{5,8}
    install -vm644 build/man/"$u".d.5 "${pkgdir}"/usr/share/man/man5
    install -vm644 build/man/systemd-"$u".8 "${pkgdir}"/usr/share/man/man8/"$u".8
}

package_udev() {
    pkgdesc='Userspace device file manager'
    depends=('glibc' 'gcc-libs' 'acl' 'libacl.so' 'bash' 'hwdata' 'kbd' 'kmod' 'libkmod.so'
            'libcap' 'libcap.so' 'libudev' 'util-linux' 'libblkid.so')
    provides=("udev=$pkgver")

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags udev,udev-devel

    mv -v  "${pkgdir}"/usr/bin/systemd-hwdb "${pkgdir}"/usr/bin/udev-hwdb

    for m in udev.conf.5 udev.7 udevadm.8; do
        _inst_man_udev "$m"
    done
    _inst_doc

    # initcpio
    install -vD -m0644 initcpio-install-udev "${pkgdir}"/usr/lib/initcpio/install/udev
    install -vD -m0644 initcpio-hook-udev "${pkgdir}"/usr/lib/initcpio/hooks/udev

    # pacman hooks
    make -C alpm-hooks DESTDIR="${pkgdir}" install_udev
}

package_libudev() {
    pkgdesc='udev library for enumerating and introspecting local devices'
    depends=('gcc-libs' 'glibc' 'libcap' 'libcap.so')
    provides=('libudev.so')

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags libudev,libudev-devel

    _inst_man_udev "libudev.3"
}

package_esysusers() {
    pkgdesc='the sysusers.d binary'
    depends=('glibc' 'gcc-libs' 'libxcrypt' 'libcrypt.so' 'libcap' 'libcap.so')

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags sysusers

    mv -v  "${pkgdir}"/usr/bin/systemd-sysusers.standalone "${pkgdir}"/usr/bin/sysusers

    _inst_man_utils sysusers
    _inst_doc

    # pacman hooks
    make -C alpm-hooks DESTDIR="${pkgdir}" install_sysusers
}

package_etmpfiles() {
    pkgdesc='the tmpfiles.d binary'
    depends=('glibc' 'gcc-libs' 'acl' 'libacl.so' 'libcap' 'libcap.so')

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags tmpfiles

    mv -v "${pkgdir}"/usr/bin/systemd-tmpfiles.standalone "${pkgdir}"/usr/bin/tmpfiles

    _inst_man_utils tmpfiles
    _inst_doc

    # pacman hooks
    make -C alpm-hooks DESTDIR="${pkgdir}" install_tmpfiles
}
