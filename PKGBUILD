# Maintainer: Artoo <artoo@artixlinux.org>
# Contributor: Christian Hesse <mail@eworm.de>
# Contributor: Dave Reisner <dreisner@archlinux.org>
# Contributor: Tom Gundersen <teg@jklm.no>

_pkgbase=systemd-stable

_alpm=1.4 # git rev-parse ${_alpm} #83961019292a041e1d2c07389d639065632e3f1f

pkgbase=udev
pkgname=('udev' 'libudev' 'esysusers' 'etmpfiles')
pkgdesc='Userspace device file manager'
_tag='d357815dc8213b736ac7bc5eefcc28a4dbb4f64f' # git rev-parse v${_tag_name}
_tag_name=254.3
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
source=("git+https://github.com/systemd/systemd-stable#tag=${_tag}" #?signed
        "git+https://github.com/systemd/systemd#tag=v${_tag_name%.*}" #?signed
        "git+https://gitea.artixlinux.org/artix/alpm-hooks.git#tag=${_alpm}"
        initcpio-{hook,install}-udev
        0001-Use-Arch-Linux-device-access-groups.patch
        0001-artix-standalone-install.patch
)
sha512sums=('SKIP'
            'SKIP'
            'SKIP'
            '5468867509f16e1955242db235aad760672545ab2d7f74d7f5316904494b9218db1dde0d008e789fbfbed8a82b3144f20f2c927f224ca5e9f3a98becd517864f'
            '285ac182e5666317bef8c91c9559b71325a0bd42352289800e13a8724c883dd0bc903e731eef3f6238e5f261c75c3308886627221fda2d2e4170e5626e9adc1d'
            'c5845849a8c66cc1192b1f263098b379c983f779c9521771c6dddc5271e75e96672dce4db112895dcd43f129f884353ecbbab4103c704fbd046657f269a324e1'
            '3d985204cda5faadc21188874127a03d1543072e16c11eca871ea000d273c226ffba3b458d06606fdef4334326bd6fed727fe1d781c763871ff4bdaa8fb42d66')

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

    patch -Np1 -i ../0001-artix-standalone-install.patch

    # Replace cdrom/dialout/tape groups with optical/uucp/storage
    patch -Np1 -i ../0001-Use-Arch-Linux-device-access-groups.patch
}

build() {
    local _meson_options=() _targets=()

    _meson_options+=(
        -Dversion-tag="${_tag_name/-/\~}-${pkgrel}-artix"
        -Dshared-lib-tag="${pkgver}-${pkgrel}"
        -Dmode=release

        -Dstandalone-binaries=true
        -Dsysusers=true
        -Dtmpfiles=true
        -Dhwdb=true
        -Dblkid=true
        -Dkmod=true

        -Dgshadow=false

        -Dsbat-distro='artix'
        -Dsbat-distro-summary='Artix Linux'
        -Dsbat-distro-pkgname="${pkgname}"
        -Dsbat-distro-version="${pkgver}"

        -Dtests=true

        -Dlink-udev-shared=false
        -Dlink-boot-shared=false

        -Ddefault-keymap='us'

        -Dman=false
        -Dhtml=false

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
        -Dapparmor=false
        -Daudit=false
        -Dbacklight=false
        -Dbinfmt=false
        -Dbootloader=false
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

    _targets+=(
        udev:shared_library
        src/libudev/libudev.pc
        udevadm
        udev-hwdb
        src/udev/{ata_id,cdrom_id,dmi_memory_id,fido_id,iocost,mtd_probe,scsi_id,v4l_id}
        src/udev/udev.pc
        rules.d/{50-udev-default,64-btrfs}.rules
        hwdb.d/60-autosuspend-chromiumos.hwdb
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

        test-fido-id-desc
        test-udev-builtin
        test-udev-event
        test-udev-node
        test-udev-util
        test-udev-device-thread
        test-libudev
        test-libudev-sym
    )
    meson compile -C build "${_targets[@]}"
}

check() {
    local tests=()
    tests+=(
        rule-syntax-check
        test-fido-id-desc
        test-udev-builtin
        test-udev-event
        test-udev-node
        test-udev-util
        test-udev-device-thread
        test-libudev
        test-libudev-sym

        test-sysusers.standalone
        test-systemd-tmpfiles.standalone
    )
    meson test -C build --print-errorlogs "${tests[@]}"
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
    depends=('acl' 'libacl.so' 'bash' 'gcc-libs' 'glibc' 'hwdata'
            'kbd' 'kmod' 'libkmod.so' 'libcap' 'libcap.so'
            'libudev' 'util-linux' 'libblkid.so')
    provides=("udev=$pkgver")

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags udev,udev-devel

    for m in udev.conf.5 iocost.conf.5 udev.7 udevadm.8 systemd-hwdb.8; do
        _inst_man "$m"
    done

    # initcpio
    install -vD -m0644 initcpio-install-udev "${pkgdir}"/usr/lib/initcpio/install/udev
    install -vD -m0644 initcpio-hook-udev "${pkgdir}"/usr/lib/initcpio/hooks/udev

    # pacman hooks
    make -C alpm-hooks DESTDIR="${pkgdir}" install_udev
}

package_libudev() {
    pkgdesc='udev library for enumerating and introspecting local devices'
    depends=('gcc-libs' 'glibc'
            'libcap' 'libcap.so')
    provides=('libudev.so')

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags libudev,libudev-devel

    _inst_man "libudev.3"
}

package_esysusers() {
    pkgdesc='the sysusers.d binary'
    depends=('gcc-libs' 'glibc'
            'libcap' 'libcap.so'
            'libxcrypt' 'libcrypt.so')

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags sysusers

    _inst_man "sysusers.d.5"
    _inst_man "systemd-sysusers.8"

    # pacman hooks
    make -C alpm-hooks DESTDIR="${pkgdir}" install_sysusers
}

package_etmpfiles() {
    pkgdesc='the tmpfiles.d binary'
    depends=('acl' 'libacl.so'
            'gcc-libs' 'glibc'
            'libcap' 'libcap.so')

    meson install -C build --destdir "$pkgdir" --no-rebuild --tags tmpfiles

    _inst_man "tmpfiles.d.5"
    _inst_man "systemd-tmpfiles.8"

    # pacman hooks
    make -C alpm-hooks DESTDIR="${pkgdir}" install_tmpfiles
}
