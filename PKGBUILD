#_                   _ _ _  _ _____ _  _
#| | _______   ____ _| | | || |___  | || |
#| |/ / _ \ \ / / _` | | | || |_ / /| || |_
#|   <  __/\ V / (_| | | |__   _/ / |__   _|
#|_|\_\___| \_/ \__,_|_|_|  |_|/_/     |_|

#Maintainer: kevall474 <kevall474@tuta.io> <https://github.com/kevall474>
#Credits: Jan Alexander Steffens (heftig) <heftig@archlinux.org> ---> For the base PKGBUILD
#Credits: Andreas Radke <andyrtr@archlinux.org> ---> For the base PKGBUILD
#Credits: Linus Torvalds ---> For the linux kernel
#Credits: Joan Figueras <ffigue at gmail dot com> ---> For the base PKFBUILD
#Credits: Piotr Gorski <lucjan.lucjanov@gmail.com> <https://github.com/sirlucjan/kernel-patches> ---> For the patches and the base pkgbuild
#Credits: Tk-Glitch <https://github.com/Tk-Glitch> ---> For some patches. base PKGBUILD and prepare script
#Credits: Con Kolivas <kernel@kolivas.org> <http://ck.kolivas.org/> ---> For MuQSS patches
#Credits: Hamad Al Marri <https://github.com/hamadmarri/cachy-sched> ---> For CacULE CPU Scheduler patch
#Credits: Alfred Chen <https://gitlab.com/alfredchen/projectc> ---> For the BMQ/PDS CPU Scheduler patch

################# CPU Scheduler #################

#Set CPU Scheduler
#Set '1' for CacULE CPU Scheduler
#Set '2' for CacULE-RDB CPU Scheduler
#Set '3' for BMQ CPU Scheduler
#Set '4' for PDS CPU Scheduler
#Set '5' for MuQSS CPUC Scheduler
#Leave empty for no CPU Scheduler
#Default is empty. It will build with no cpu scheduler. To build with cpu shceduler just use : env _cpu_sched=(1,2,3,4 or 5) makepkg -s
if [ -z ${_cpu_sched+x} ]; then
  _cpu_sched=
fi

################################# Arch ################################

ARCH=x86

################################# GCC ################################

# Grap GCC version
# Workarround with GCC 12.0.0. Pluggins don't work, so we have to grap GCC version
# and disable CONFIG_HAVE_GCC_PLUGINS/CONFIG_GCC_PLUGINS

GCC_VERSION=$(gcc -dumpversion)

################################# CC/CXX/HOSTCC/HOSTCXX ################################

#Set compiler to build the kernel
#Set '1' to build with GCC
#Set '2' to build with CLANG and LLVM
#Default is empty. It will build with GCC. To build with different compiler just use : env _compiler=(1 or 2) makepkg -s
if [ -z ${_compiler+x} ]; then
  _compiler=
fi

if [[ "$_compiler" = "1" ]]; then
  _compiler=1
  CC=gcc
  CXX=g++
  HOSTCC=gcc
  HOSTCXX=g++
elif [[ "$_compiler" = "2" ]]; then
  _compiler=2
  CC=clang
  CXX=clang++
  HOSTCC=clang
  HOSTCXX=clang++
else
  _compiler=1
  CC=gcc
  CXX=g++
  HOSTCC=gcc
  HOSTCXX=g++
fi

###################################################################################

# This section set the pkgbase based on the cpu scheduler, so user can build different package based on the cpu scheduler.
if [[ $_cpu_sched = "1" ]]; then
  if [[ "$_compiler" = "1" ]]; then
    pkgbase=linux-kernel-cacule-gcc
  elif [[ "$_compiler" = "2" ]]; then
    pkgbase=linux-kernel-cacule-clang
  fi
elif [[ $_cpu_sched = "2" ]]; then
  if [[ "$_compiler" = "1" ]]; then
    pkgbase=linux-kernel-cacule-rdb-gcc
  elif [[ "$_compiler" = "2" ]]; then
    pkgbase=linux-kernel-cacule-rdb-clang
  fi
elif [[ $_cpu_sched = "3" ]]; then
  if [[ "$_compiler" = "1" ]]; then
    pkgbase=linux-kernel-bmq-gcc
  elif [[ "$_compiler" = "2" ]]; then
    pkgbase=linux-kernel-bmq-clang
  fi
elif [[ $_cpu_sched = "4" ]]; then
  if [[ "$_compiler" = "1" ]]; then
    pkgbase=linux-kernel-pds-gcc
  elif [[ "$_compiler" = "2" ]]; then
    pkgbase=linux-kernel-pds-clang
  fi
elif [[ $_cpu_sched = "5" ]]; then
  if [[ "$_compiler" = "1" ]]; then
    pkgbase=linux-kernel-muqss-gcc
  elif [[ "$_compiler" = "2" ]]; then
    pkgbase=linux-kernel-muqss-clang
  fi
else
  if [[ "$_compiler" = "1" ]]; then
    pkgbase=linux-kernel-gcc
  elif [[ "$_compiler" = "2" ]]; then
    pkgbase=linux-kernel-clang
  fi
fi
pkgname=("$pkgbase" "$pkgbase-headers")
for _p in "${pkgname[@]}"; do
  eval "package_$_p() {
    $(declare -f "_package${_p#$pkgbase}")
    _package${_p#$pkgbase}
  }"
done
pkgver=5.13.7
major=5.13
pkgrel=1
arch=(x86_64)
url="https://www.kernel.org/"
license=(GPL-2.0)
makedepends=("bison" "flex" "valgrind" "git" "cmake" "make" "extra-cmake-modules" "libelf" "elfutils"
             "python" "python-appdirs" "python-mako" "python-evdev" "python-sphinx_rtd_theme" "python-graphviz" "python-sphinx"
             "clang" "lib32-clang" "bc" "gcc" "gcc-libs" "lib32-gcc-libs" "glibc" "lib32-glibc" "pahole" "patch" "gtk3" "llvm" "lib32-llvm"
             "llvm-libs" "lib32-llvm-libs" "lld" "kmod" "libmikmod" "lib32-libmikmod" "xmlto" "xmltoman" "graphviz" "imagemagick" "imagemagick-doc"
             "rsync" "cpio" "inetutils" "gzip" "zstd" "xz")
patchsource=https://raw.githubusercontent.com/kevall474/kernel-patches-v2/main/$major

# base
source=("https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-$pkgver.tar.xz"
        "config-$major")
md5sums=("9bd74571c148a7753f5a237ba52f8ee5"  #linux-5.13.7.tar.xz
         "8627ed9a9c387c842039fe2bbadd18cb") #config-5.13

# acs
source+=("$patchsource/acs-patches/0001-pci-Enable-overrides-for-missing-ACS-capabilities.patch")
md5sums+=("79122a262d2270b0c91fd8bdf8b66d14") #0001-pci-Enable-overrides-for-missing-ACS-capabilities.patch)

# bbr2
source+=("$patchsource/bbr2-xanmod-patches/0001-net-tcp_bbr-broaden-app-limited-rate-sample-detectio.patch"
         "$patchsource/bbr2-xanmod-patches/0002-net-tcp_rate-consolidate-inflight-tracking-approache.patch"
         "$patchsource/bbr2-xanmod-patches/0003-net-tcp_rate-account-for-CE-marks-in-rate-sample.patch"
         "$patchsource/bbr2-xanmod-patches/0004-net-tcp_bbr-v2-shrink-delivered_mstamp-first_tx_msta.patch"
         "$patchsource/bbr2-xanmod-patches/0005-net-tcp_bbr-v2-snapshot-packets-in-flight-at-transmi.patch"
         "$patchsource/bbr2-xanmod-patches/0006-net-tcp_bbr-v2-count-packets-lost-over-TCP-rate-samp.patch"
         "$patchsource/bbr2-xanmod-patches/0007-net-tcp_bbr-v2-export-FLAG_ECE-in-rate_sample.is_ece.patch"
         "$patchsource/bbr2-xanmod-patches/0008-net-tcp_bbr-v2-introduce-ca_ops-skb_marked_lost-CC-m.patch"
         "$patchsource/bbr2-xanmod-patches/0009-net-tcp_bbr-v2-factor-out-tx.in_flight-setting-into-.patch"
         "$patchsource/bbr2-xanmod-patches/0010-net-tcp_bbr-v2-adjust-skb-tx.in_flight-upon-merge-in.patch"
         "$patchsource/bbr2-xanmod-patches/0011-net-tcp_bbr-v2-adjust-skb-tx.in_flight-upon-split-in.patch"
         "$patchsource/bbr2-xanmod-patches/0012-net-tcp_bbr-v2-set-tx.in_flight-for-skbs-in-repair-w.patch"
         "$patchsource/bbr2-xanmod-patches/0013-net-tcp-add-new-ca-opts-flag-TCP_CONG_WANTS_CE_EVENT.patch"
         "$patchsource/bbr2-xanmod-patches/0014-net-tcp-re-generalize-TSO-sizing-in-TCP-CC-module-AP.patch"
         "$patchsource/bbr2-xanmod-patches/0015-net-tcp-add-fast_ack_mode-1-skip-rwin-check-in-tcp_f.patch"
         "$patchsource/bbr2-xanmod-patches/0016-net-tcp_bbr-v2-BBRv2-bbr2-congestion-control-for-Lin.patch"
         "$patchsource/bbr2-xanmod-patches/0017-net-tcp_bbr-v2-remove-unnecessary-rs.delivered_ce-lo.patch"
         "$patchsource/bbr2-xanmod-patches/0018-net-tcp_bbr-v2-remove-field-bw_rtts-that-is-unused-i.patch"
         "$patchsource/bbr2-xanmod-patches/0019-net-tcp_bbr-v2-remove-cycle_rand-parameter-that-is-u.patch"
         "$patchsource/bbr2-xanmod-patches/0020-net-tcp_bbr-v2-don-t-assume-prior_cwnd-was-set-enter.patch"
         "$patchsource/bbr2-xanmod-patches/0021-net-tcp_bbr-v2-Fix-missing-ECT-markings-on-retransmi.patch")
md5sums+=("14cd0786e7a2fe401ecc68470c3974b2"  #0001-net-tcp_bbr-broaden-app-limited-rate-sample-detectio.patch
          "0ed691752273d3084a6e000f88b8ffd1"  #0002-net-tcp_rate-consolidate-inflight-tracking-approache.patch
          "0854a328871f1e4c3a0b88e7bade7397"  #0003-net-tcp_rate-account-for-CE-marks-in-rate-sample.patch
          "1ac05ab50687ab1616f6389f1425d742"  #0004-net-tcp_bbr-v2-shrink-delivered_mstamp-first_tx_msta.patch
          "ac62690bf791ef4e4e5980cf60c4b63d"  #0005-net-tcp_bbr-v2-snapshot-packets-in-flight-at-transmi.patch
          "47fbe9222799a3225c15092e24a88817"  #0006-net-tcp_bbr-v2-count-packets-lost-over-TCP-rate-samp.patch
          "83aba6fec39d54e30d653fee797212e4"  #0007-net-tcp_bbr-v2-export-FLAG_ECE-in-rate_sample.is_ece.patch
          "beef0323e0ab80ae8c4a31b90733a67a"  #0008-net-tcp_bbr-v2-introduce-ca_ops-skb_marked_lost-CC-m.patch
          "d520d97e01c43714a7db083be5c33899"  #0009-net-tcp_bbr-v2-factor-out-tx.in_flight-setting-into-.patch
          "7146d39cd71eac14d652c607a86805f6"  #0010-net-tcp_bbr-v2-adjust-skb-tx.in_flight-upon-merge-in.patch
          "341bbdd0d533da6a37bbcf6011d134d4"  #0011-net-tcp_bbr-v2-adjust-skb-tx.in_flight-upon-split-in.patch
          "8cb5bd2d0cc3eb34a4c477d9e1b0f766"  #0012-net-tcp_bbr-v2-set-tx.in_flight-for-skbs-in-repair-w.patch
          "390aace232c07ebdf99775e919fdd807"  #0013-net-tcp-add-new-ca-opts-flag-TCP_CONG_WANTS_CE_EVENT.patch
          "f157575c1d1fd355c9f0dda58f90e944"  #0014-net-tcp-re-generalize-TSO-sizing-in-TCP-CC-module-AP.patch
          "4de451f6a4fe99ca4af3b322e7ee1825"  #0015-net-tcp-add-fast_ack_mode-1-skip-rwin-check-in-tcp_f.patch
          "4607cb02349094c824d24162c593a651"  #0016-net-tcp_bbr-v2-BBRv2-bbr2-congestion-control-for-Lin.patch
          "c81314714563f91845d6f3d84869910d"  #0017-net-tcp_bbr-v2-remove-unnecessary-rs.delivered_ce-lo.patch
          "6a051d6dd33cce8a35450ba0dbcd4020"  #0018-net-tcp_bbr-v2-remove-field-bw_rtts-that-is-unused-i.patch
          "888aabc09bb088f131ab894c3d7af19d"  #0019-net-tcp_bbr-v2-remove-cycle_rand-parameter-that-is-u.patch
          "2d79e78d7b644c224922a4fe2d8d73c8"  #0020-net-tcp_bbr-v2-don-t-assume-prior_cwnd-was-set-enter.patch
          "1074c5c59c14a27ba21a081c2c8838b8") #0021-net-tcp_bbr-v2-Fix-missing-ECT-markings-on-retransmi.patch

# block
source+=("$patchsource/block-patches/0002-XANMOD-block-bfq-change-BLK_DEV_ZONED-depends-to-IOS.patch"
         "$patchsource/block-patches/0003-XANMOD-block-set-rq_affinity-to-force-full-multithre.patch")
md5sums+=("e58778120aca9f6cbb31915d5a3b0d9f"  #0002-XANMOD-block-bfq-change-BLK_DEV_ZONED-depends-to-IOS.patch
          "e7b7cdc158f59c6198ae271efddf3fb0") #0003-XANMOD-block-set-rq_affinity-to-force-full-multithre.patch

# clearlinux
source+=("$patchsource/clearlinux-patches/0101-i8042-decrease-debug-message-level-to-info.patch"
         "$patchsource/clearlinux-patches/0102-increase-the-ext4-default-commit-age.patch"
         "$patchsource/clearlinux-patches/0103-silence-rapl.patch"
         "$patchsource/clearlinux-patches/0104-pci-pme-wakeups.patch"
         "$patchsource/clearlinux-patches/0105-ksm-wakeups.patch"
         "$patchsource/clearlinux-patches/0106-intel_idle-tweak-cpuidle-cstates.patch"
         "$patchsource/clearlinux-patches/0108-smpboot-reuse-timer-calibration.patch"
         "$patchsource/clearlinux-patches/0109-initialize-ata-before-graphics.patch"
         "$patchsource/clearlinux-patches/0110-give-rdrand-some-credit.patch"
         "$patchsource/clearlinux-patches/0111-ipv4-tcp-allow-the-memory-tuning-for-tcp-to-go-a-lit.patch"
         "$patchsource/clearlinux-patches/0112-init-wait-for-partition-and-retry-scan.patch"
         "$patchsource/clearlinux-patches/0113-print-fsync-count-for-bootchart.patch"
         "$patchsource/clearlinux-patches/0114-add-boot-option-to-allow-unsigned-modules.patch"
         "$patchsource/clearlinux-patches/0115-enable-stateless-firmware-loading.patch"
         "$patchsource/clearlinux-patches/0116-migrate-some-systemd-defaults-to-the-kernel-defaults.patch"
         "$patchsource/clearlinux-patches/0117-xattr-allow-setting-user.-attributes-on-symlinks-by-.patch"
         "$patchsource/clearlinux-patches/0118-add-scheduler-turbo3-patch.patch"
         "$patchsource/clearlinux-patches/0119-use-lfence-instead-of-rep-and-nop.patch"
         "$patchsource/clearlinux-patches/0121-locking-rwsem-spin-faster.patch"
         "$patchsource/clearlinux-patches/0122-ata-libahci-ignore-staggered-spin-up.patch"
         "$patchsource/clearlinux-patches/0123-print-CPU-that-faults.patch"
         "$patchsource/clearlinux-patches/0124-x86-microcode-Force-update-a-uCode-even-if-the-rev-i.patch"
         "$patchsource/clearlinux-patches/0125-x86-microcode-echo-2-reload-to-force-load-ucode.patch"
         "$patchsource/clearlinux-patches/0126-fix-bug-in-ucode-force-reload-revision-check.patch"
         "$patchsource/clearlinux-patches/0127-nvme-workaround.patch"
         "$patchsource/clearlinux-patches/0128-don-t-report-an-error-if-PowerClamp-run-on-other-CPU.patch")
md5sums+=("54db9173a84480019a507b3d521e4900"  #0101-i8042-decrease-debug-message-level-to-info.patch
          "c74d43eed0b392be6186266401525e61"  #0102-increase-the-ext4-default-commit-age.patch
          "47fa6adf0d324b8f2e822bdf1e609b2b"  #0103-silence-rapl.patch
          "373e8b9f7f5a8040d7afb673af9ecbbd"  #0104-pci-pme-wakeups.patch
          "41e9bad1652b3cfc10d0909bbffa4b9d"  #0105-ksm-wakeups.patch
          "9096411a87c757be72863a221ee70087"  #0106-intel_idle-tweak-cpuidle-cstates.patch
          "a1c63a226dd5e85e7ce339bb2f1b46ce"  #0108-smpboot-reuse-timer-calibration.patch
          "05150402becb6f3966aa812c57e65d62"  #0109-initialize-ata-before-graphics.patch
          "e78fa9e6bfa9e6cc9666bdc95352e6ae"  #0110-give-rdrand-some-credit.patch
          "be65fd4bd13482a1ef0e5c4c52bd0293"  #0111-ipv4-tcp-allow-the-memory-tuning-for-tcp-to-go-a-lit.patch
          "0649545f936191d71356f104b4be9828"  #0112-init-wait-for-partition-and-retry-scan.patch
          "2a34bb57221b4bb4169003c830083f3e"  #0113-print-fsync-count-for-bootchart.patch
          "6bdc4e521cea370925058afd7d6b0fc1"  #0114-add-boot-option-to-allow-unsigned-modules.patch
          "23f08ce07b71058561076f8c62545768"  #0115-enable-stateless-firmware-loading.patch
          "b6e025b5126638324043509ba1382a01"  #0116-migrate-some-systemd-defaults-to-the-kernel-defaults.patch
          "278e2c0da12099fb055ada9c36265700"  #0117-xattr-allow-setting-user.-attributes-on-symlinks-by-.patch
          "b538e12fd5ae76a9ae5243b1b26c2ae2"  #0118-add-scheduler-turbo3-patch.patch
          "991f9a3a7380bd41e53cbf82b1c6c260"  #0119-use-lfence-instead-of-rep-and-nop.patch
          "60e18c045e49f70abd2441648f2435cd"  #0121-locking-rwsem-spin-faster.patch
          "8da8427e1dcb80252e0b0660a3c60a50"  #0122-ata-libahci-ignore-staggered-spin-up.patch
          "570f130b2eae7a67696a5577aff76077"  #0123-print-CPU-that-faults.patch
          "495f97ed7c14c97b58217e60d25f1a31"  #0124-x86-microcode-Force-update-a-uCode-even-if-the-rev-i.patch
          "bd99806d6aeac102c60feda5b72b750a"  #0125-x86-microcode-echo-2-reload-to-force-load-ucode.patch
          "f06882f502f54afef0be07e6b56dac8e"  #0126-fix-bug-in-ucode-force-reload-revision-check.patch
          "e4bfd87f310ba2b7856a5e6719da58b0"  #0127-nvme-workaround.patch
          "a33df819da6e41a640624779cc363a8c") #0128-don-t-report-an-error-if-PowerClamp-run-on-other-CPU.patch

# cpu
source+=("$patchsource/cpu-graysky2-patches/more-uarches-for-kernel-5.8+.patch"
         "$patchsource/cpu-xanmod-patches/0012-XANMOD-init-Kconfig-Enable-O3-KBUILD_CFLAGS-optimiza.patch"
         "$patchsource/cpu-xanmod-patches/0013-XANMOD-Makefile-Turn-off-loop-vectorization-for-GCC-.patch"
         "$patchsource/cpu-sirlucjan-patches/0003-init-Kconfig-add-O1-flag.patch")
md5sums+=("15bc6521aae3d5aaded3da70b95ba854"  #more-uarches-for-kernel-5.8+.patch
          "2eabdcffa17414b0dd129dd5827947bf"  #0012-XANMOD-init-Kconfig-Enable-O3-KBUILD_CFLAGS-optimiza.patch
          "468cc483703a864bf85b97939e5b7d1b"  #0013-XANMOD-Makefile-Turn-off-loop-vectorization-for-GCC-.patch
          "9ed92b6421a4829c3be67af8e4b65a04") #0003-init-Kconfig-add-O1-flag.patch

# compaction
source+=("https://raw.githubusercontent.com/kevall474/kernel-patches/main/5.12/compaction-patches/0001-compaction-patches.patch")
md5sums+=("98564f54c3f9a6da56c6156d26b3ea39") #0001-compaction-patches.patch

# elevator
source+=("$patchsource/elevator-xanmod-patches/0001-XANMOD-elevator-set-default-scheduler-to-bfq-for-blk.patch")
md5sums+=("90e5f7c17bf202b56bf4436fbc24d482") #0001-XANMOD-elevator-set-default-scheduler-to-bfq-for-blk.patch

# futex
source+=("$patchsource/futex-tkg-patches/0007-v5.13-fsync.patch"
         "$patchsource/futex-tkg-patches/0007-v5.13-futex2_interface.patch")
md5sums+=("3cd1d74ccdd581c588929cd30bc2065a"  #0007-v5.13-fsync.patch
          "2c0375b3cc9690a0f0f3d3e49df54d10") #0007-v5.13-futex2_interface.patch

# intel
source+=("$patchsource/intel-zen-patches/0010-ZEN-intel-pstate-Implement-enable-parameter.patch")
md5sums+=("a13452de360638df606dceb1031c9a45") #0010-ZEN-intel-pstate-Implement-enable-parameter.patch

# irq
source+=("$patchsource/irq-zen-patches/0011-ZEN-Add-an-option-to-make-threadirqs-the-default.patch")
md5sums+=("bc06c9355cf6b4c67714bdc6c40bcb39") #0011-ZEN-Add-an-option-to-make-threadirqs-the-default.patch

# lrng
source+=("$patchsource/lrng-patches/v41-0001-Linux-Random-Number-Generator.patch"
         "$patchsource/lrng-patches/v41-0002-LRNG-allocate-one-DRNG-instance-per-NUMA-node.patch"
         "$patchsource/lrng-patches/v41-0003-LRNG-sysctls-and-proc-interface.patch"
         "$patchsource/lrng-patches/v41-0004-LRNG-add-switchable-DRNG-support.patch"
         "$patchsource/lrng-patches/v41-0005-LRNG-add-common-generic-hash-support.patch"
         "$patchsource/lrng-patches/v41-0006-crypto-DRBG-externalize-DRBG-functions-for-LRNG.patch"
         "$patchsource/lrng-patches/v41-0007-LRNG-add-SP800-90A-DRBG-extension.patch"
         "$patchsource/lrng-patches/v41-0008-LRNG-add-kernel-crypto-API-PRNG-extension.patch"
         "$patchsource/lrng-patches/v41-0009-crypto-provide-access-to-a-static-Jitter-RNG-sta.patch"
         "$patchsource/lrng-patches/v41-0010-LRNG-add-Jitter-RNG-fast-noise-source.patch"
         "$patchsource/lrng-patches/v41-0011-LRNG-add-SP800-90B-compliant-health-tests.patch"
         "$patchsource/lrng-patches/v41-0012-LRNG-add-interface-for-gathering-of-raw-entropy.patch"
         "$patchsource/lrng-patches/v41-0013-LRNG-add-power-on-and-runtime-self-tests.patch")
md5sums+=("006b98bc15ce286c51a28cc3bbf2e84f"  #v41-0001-Linux-Random-Number-Generator.patch
          "7727d2bc6a5e4804f85e65ed4c0b0387"  #v41-0002-LRNG-allocate-one-DRNG-instance-per-NUMA-node.patch
          "857f994ebffe24b1160f47b7c66ae276"  #v41-0003-LRNG-sysctls-and-proc-interface.patch
          "4df38bd3b05cc88e539c3e43c026576b"  #v41-0004-LRNG-add-switchable-DRNG-support.patch
          "b8b9f39f01c3585f9ed6ea188fba8377"  #v41-0005-LRNG-add-common-generic-hash-support.patch
          "dbb510d6fc8ed89f7cef487d69f03720"  #v41-0006-crypto-DRBG-externalize-DRBG-functions-for-LRNG.patch
          "20f11652be999958d2620c28d089c761"  #v41-0007-LRNG-add-SP800-90A-DRBG-extension.patch
          "d2d87d82e797966aac03a64c5f66d9f4"  #v41-0008-LRNG-add-kernel-crypto-API-PRNG-extension.patch
          "f20d5865e0a5623a311c9e39cd04edcd"  #v41-0009-crypto-provide-access-to-a-static-Jitter-RNG-sta.patch
          "2ed7a06f3ab3d5608f3249b6dc996c95"  #v41-0010-LRNG-add-Jitter-RNG-fast-noise-source.patch
          "9c97d551f93ef152c7f9c382bce4e6fb"  #v41-0011-LRNG-add-SP800-90B-compliant-health-tests.patch
          "55afb3463d4d38c1dd7d321781c5d13f"  #v41-0012-LRNG-add-interface-for-gathering-of-raw-entropy.patch
          "43cd2848b6512755defaff51789e5ade") #v41-0013-LRNG-add-power-on-and-runtime-self-tests.patch

# mm
source+=("$patchsource/mm-patches/0004-mm-set-8-megabytes-for-address_space-level-file-read.patch"
         "$patchsource/mm-pf-patches/0001-mm-5.13-protect-file-mappings-under-memory-pressure.patch"
         "$patchsource/mm-pf-patches/0002-mm-5.13-protect-anonymous-mappings-under-memory-pres.patch"
         "$patchsource/mm-tkg-patches/0001-mm-Support-soft-dirty-flag-reset-for-VA-range.patch"
         "$patchsource/mm-tkg-patches/0002-mm-Support-soft-dirty-flag-read-with-reset.patch"
         "$patchsource/mm-xanmod-patches/0008-XANMOD-mm-vmscan-vm_swappiness-30-decreases-the-amou.patch"
         "$patchsource/mm-zen-patches/0002-mm-swapfile-use-percpu_ref-to-serialize-against-conc.patch"
         "$patchsource/mm-zen-patches/0003-mm-swap-remove-confusing-checking-for-non_swap_entry.patch"
         "$patchsource/mm-zen-patches/0004-Revert-swap-fixes-after-stable-reverts.patch"
         "$patchsource/mm-zen-patches/0013-ZEN-mm-Disable-watermark-boosting-by-default.patch"
         "$patchsource/mm-zen-patches/0014-ZEN-mm-Stop-kswapd-early-when-nothing-s-waiting-for-.patch"
         "$patchsource/mm-zen-patches/0015-ZEN-mm-Don-t-stop-kswapd-on-a-per-node-basis-when-th.patch"
         "$patchsource/mm-zen-patches/0024-mm-vmscan-add-sysctl-knobs-for-protecting-clean-cach.patch")
md5sums+=("26be410fdc6d7c4f4e122a74fda6f96b"  #0004-mm-set-8-megabytes-for-address_space-level-file-read.patch
          "7c0ec741ff6eb64296f41564f77bb946"  #0001-mm-5.13-protect-file-mappings-under-memory-pressure.patch
          "c7b60a51ca14e1957aeab97e2c93c672"  #0002-mm-5.13-protect-anonymous-mappings-under-memory-pres.patch
          "d6b3bcd857e74530a9d0347c6dc05c13"  #0001-mm-Support-soft-dirty-flag-reset-for-VA-range.patch
          "1e7a53eae980951494ee630853d39d98"  #0002-mm-Support-soft-dirty-flag-read-with-reset.patch
          "45e37e9feb1010271d6c537a3c2f3bf5"  #0008-XANMOD-mm-vmscan-vm_swappiness-30-decreases-the-amou.patch
          "f3437dc0536f5ba600ad98b233a556a5"  #0002-mm-swapfile-use-percpu_ref-to-serialize-against-conc.patch
          "f8a76421e812ff8e11a1fc171bf9c141"  #0003-mm-swap-remove-confusing-checking-for-non_swap_entry.patch
          "19273060b4e9459475058536c480c4c6"  #0004-Revert-swap-fixes-after-stable-reverts.patch
          "6af95507d914ebc79fd6d58643863383"  #0013-ZEN-mm-Disable-watermark-boosting-by-default.patch
          "c313937d9170be3a6fd484de937ee615"  #0014-ZEN-mm-Stop-kswapd-early-when-nothing-s-waiting-for-.patch
          "894d0c31781a3733c238cd3ba203c017"  #0015-ZEN-mm-Don-t-stop-kswapd-on-a-per-node-basis-when-th.patch
          "a1c54bae97afbe0614f5cd9c3c767e58") #0024-mm-vmscan-add-sysctl-knobs-for-protecting-clean-cach.patch

# mq-deadline
source+=("$patchsource/mq-deadline-patches/0009-ZEN-Add-CONFIG-to-rename-the-mq-deadline-scheduler-v3.patch")
md5sums+=("3234ef6a0549247cc27669b76e687b19") #0009-ZEN-Add-CONFIG-to-rename-the-mq-deadline-scheduler-v3.patch

# ntfs3
source+=("$patchsource/ntfs3-xanmod-patches/ntfs3-xanmod-patches.patch")
md5sums+=("40c4983de6192962e26d7a6f79ca1cec") #ntfs3-xanmod-patches.patch

# openrgb
source+=("$patchsource/openrgb-zen-patches/0003-ZEN-Add-OpenRGB-patches.patch")
md5sums+=("48e7ccaf1168550be498da0b6c9882b4") #0003-ZEN-Add-OpenRGB-patches.patch

# security
source+=("$patchsource/security/security-xanmod-patches.patch")
md5sums+=("8330e56c07373af22f8d9f03b8cdfc32") #security-xanmod-patches.patch

# timer
source+=("$patchsource/timer-xanmod-patches/0004-XANMOD-kconfig-add-500Hz-timer-interrupt-kernel-conf.patch")
md5sums+=("ec30ff83f107a00aa1f5943963a5291c") #0004-XANMOD-kconfig-add-500Hz-timer-interrupt-kernel-conf.patch

# v4l2loopback
source+=("$patchsource/v4l2loopback-pf-patches/0001-v4l2loopback-5.12-merge-v0.12.5.patch"
         "$patchsource/v4l2loopback-pf-patches/0002-v4l2loopback-5.13-confine-v4l2loopback_cleanup_modul.patch")
md5sums+=("7f9b0cf8f9253d0e1e931a39f83ae9e0"  #0001-v4l2loopback-5.12-merge-v0.12.5.patch
          "05f75cb594382a875170ae9632ae070b") #0002-v4l2loopback-5.13-confine-v4l2loopback_cleanup_modul.patch

# wine
source+=("$patchsource/wine-patches/0007-v5.13-winesync.patch")
md5sums+=("9573b92353399343db8a691c9b208300") #0007-v5.13-winesync.patch

# xanmod
source+=("$patchsource/xanmod-patches/0005-XANMOD-kconfig-set-PREEMPT-and-RCU_BOOST-without-del.patch"
         "$patchsource/xanmod-patches/0006-XANMOD-dcache-cache_pressure-50-decreases-the-rate-a.patch"
         "$patchsource/xanmod-patches/0007-XANMOD-sched-autogroup-Add-kernel-parameter-and-conf.patch"
         "$patchsource/xanmod-patches/0009-XANMOD-cpufreq-tunes-ondemand-and-conservative-gover.patch"
         "$patchsource/xanmod-patches/0011-XANMOD-lib-kconfig.debug-disable-default-CONFIG_SYMB.patch")
md5sums+=("SKIP"
          "SKIP"
          "SKIP"
          "SKIP"
          "SKIP")

# zen
source+=("$patchsource/zen-patches/0002-ZEN-Add-VHBA-driver.patch"
         "$patchsource/zen-patches/0006-ZEN-Disable-stack-conservation-for-GCC.patch"
         "$patchsource/zen-patches/0008-ZEN-Add-sysctl-and-CONFIG-to-disallow-unprivileged-C.patch"
         "$patchsource/zen-patches/0001-block-another-attempt-to-fix-discard-merging.patch"
         "$patchsource/zen-patches/0025-ZEN-Tune-default-CLEAN_LOW-MIN_KBYTES-values.patch"
         "$patchsource/zen-patches/0026-ZEN-Set-CLEAN_MIN_KBYTES-to-0.patch")
md5sums+=("SKIP"
          "SKIP"
          "SKIP"
          "SKIP"
          "SKIP"
          "SKIP")

# zstd
source+=("$patchsource/zstd-xanmod-patches/0001-lib-zstd-Add-kernel-specific-API.patch"
         "$patchsource/zstd-xanmod-patches/0002-lib-zstd-Add-decompress_sources.h-for-decompress_unz.patch"
         "$patchsource/zstd-xanmod-patches/0003-lib-zstd-Upgrade-to-latest-upstream-zstd-version-1.4.patch"
         "$patchsource/zstd-xanmod-patches/0004-MAINTAINERS-Add-maintainer-entry-for-zstd.patch")
md5sums+=("SKIP"
          "SKIP"
          "SKIP"
          "SKIP")

# misc
source+=("$patchsource/misc-patches/0003-sched-core-nr_migrate-256-increases-number-of-tasks-.patch"
         "$patchsource/misc-patches/0005-Disable-CPU_FREQ_GOV_SCHEDUTIL.patch"
         "$patchsource/misc-patches/vm.max_map_count.patch")
md5sums+=("SKIP"
          "SKIP"
          "SKIP")

# 0014-XANMOD-fair-Remove-all-energy-efficiency-functions-v.patch workarround with CacULE
if [[ $_cpu_sched != "1" ]] && [[ $_cpu_sched != "2" ]]; then
  source+=("$patchsource/xanmod-patches/0014-XANMOD-fair-Remove-all-energy-efficiency-functions-v.patch")
  md5sums+=("SKIP")
fi

# zen_interactive workarround with CacULE
if [[ $_cpu_sched != "1" ]] && [[ $_cpu_sched != "2" ]]; then
  source+=("$patchsource/zen-interactive-patches/ZEN_INTERACTIVE.patch"
           "$patchsource/zen-interactive-patches/0027-ZEN-Restore-dirty-background-ratios-to-mainline-defa.patch")
  md5sums+=("SKIP"
            "SKIP")
fi

# CacULE patch
if [[ $_cpu_sched = "1" ]] || [[ $_cpu_sched = "2" ]]; then
  source+=("${patchsource}/cacule-patches/cacule-$major.patch")
  md5sums+=("SKIP")
fi

# prjc patch
if [[ $_cpu_sched = "3" ]] || [[ $_cpu_sched = "4" ]]; then
  source+=("${patchsource}/prjc-patches/prjc_v$major-r1.patch")
  md5sums+=("SKIP")
fi

# MuQSS patch. 0001-MultiQueue-Skiplist-Scheduler-v0.210.patch with 5.13 kernel.
# The patch is forced because there is no release from Con Kolivas for 5.13 kernel. Bugs in the kernel may occur.
if [[ $_cpu_sched = "5" ]]; then
  source+=("${patchsource}/muqss-patches/0001-MultiQueue-Skiplist-Scheduler-v0.210.patch")
  md5sums+=("SKIP")
fi

# rdb patch
if [[ $_cpu_sched = "2" ]]; then
  source+=("${patchsource}/cacule-patches/rdb-$major.patch")
  md5sums+=("SKIP")  #rdb-5.13.patch
fi

export KBUILD_BUILD_HOST=archlinux
export KBUILD_BUILD_USER=$pkgbase
export KBUILD_BUILD_TIMESTAMP="$(date -Ru${SOURCE_DATE_EPOCH:+d @$SOURCE_DATE_EPOCH})"

prepare(){

  cd linux-$pkgver

  # Apply any patch
  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.patch ]] || continue
    msg2 "Applying patch $src..."
    patch -Np1 < "../$src"
  done

  # Apply MuQSS patch
  #if [[ $_cpu_sched = "5" ]]; then
    #msg2 "Applying patch patch-$major-ck1"
    #patch -Np1 < ../patch-$major-ck1
  #fi

  # Copy the config file first
  # Copy "${srcdir}"/config-$major to "${srcdir}"/linux-${pkgver}/.config
  msg2 "Copy "${srcdir}"/config-$major to "${srcdir}"/linux-$pkgver/.config"
  cp "${srcdir}"/config-$major .config

  plain ""
  msg2 "Disable LTO"
  scripts/config --disable CONFIG_LTO
  scripts/config --disable CONFIG_LTO_CLANG
  scripts/config --disable CONFIG_ARCH_SUPPORTS_LTO_CLANG
  scripts/config --disable CONFIG_ARCH_SUPPORTS_LTO_CLANG_THIN
  scripts/config --disable CONFIG_HAS_LTO_CLANG
  scripts/config --disable CONFIG_LTO_NONE
  scripts/config --disable CONFIG_LTO_CLANG_FULL
  scripts/config --disable CONFIG_LTO_CLANG_THIN

  sleep 2s

  # fix for GCC 12.0.0 (git version)
  # plugins don't work
  # disable plugins
  #if [[ "$GCC_VERSION" = "12.0.0" ]] && [[ "$_compiler" = "1" ]]; then
  #  plain ""
  #  msg2 "Disable CONFIG_HAVE_GCC_PLUGINS/CONFIG_GCC_PLUGINS (Quick fix for gcc 12.0.0 git version)"
  #  scripts/config --disable CONFIG_HAVE_GCC_PLUGINS
  #  scripts/config --disable CONFIG_GCC_PLUGINS
  #  plain ""
  #  sleep 2s
  #fi

  plain ""

  msg2 "Set SIG level to SHA512"
  scripts/config --undefine MODULE_SIG_FORCE
  scripts/config --disable MODULE_SIG_FORCE
  scripts/config --enable CONFIG_MODULE_SIG
  scripts/config --enable CONFIG_MODULE_SIG_ALL
  scripts/config --disable CONFIG_MODULE_SIG_SHA1
  scripts/config --disable CONFIG_MODULE_SIG_SHA224
  scripts/config --disable CONFIG_MODULE_SIG_SHA256
  scripts/config --disable CONFIG_MODULE_SIG_SHA384
  scripts/config --enable CONFIG_MODULE_SIG_SHA512
  scripts/config  --set-val CONFIG_MODULE_SIG_HASH "sha512"

  sleep 2s

  msg2 "Set module compression to ZSTD"
  scripts/config --disable CONFIG_MODULE_COMPRESS_NONE
  scripts/config --disable CONFIG_MODULE_COMPRESS_GZIP
  scripts/config --disable CONFIG_MODULE_COMPRESS_XZ
  scripts/config --enable CONFIG_MODULE_COMPRESS_ZSTD

  sleep 2s

  msg2 "Enable CONFIG_STACK_VALIDATION"
  scripts/config --enable CONFIG_STACK_VALIDATION

  sleep 2s

  msg2 "Enable IKCONFIG"
  scripts/config --enable CONFIG_IKCONFIG
  scripts/config --enable CONFIG_IKCONFIG_PROC

  sleep 2s

  msg2 "Disable NUMA"
  scripts/config --disable CONFIG_NUMA
  scripts/config --disable CONFIG_AMD_NUMA
  scripts/config --disable CONFIG_X86_64_ACPI_NUMA
  scripts/config --disable CONFIG_NODES_SPAN_OTHER_NODES
  scripts/config --disable CONFIG_NUMA_EMU
  scripts/config --disable CONFIG_NEED_MULTIPLE_NODES
  scripts/config --disable CONFIG_USE_PERCPU_NUMA_NODE_ID
  scripts/config --disable CONFIG_ACPI_NUMA
  scripts/config --disable CONFIG_ARCH_SUPPORTS_NUMA_BALANCING
  scripts/config --disable CONFIG_NODES_SHIFT
  scripts/config --undefine CONFIG_NODES_SHIFT
  scripts/config --disable CONFIG_NEED_MULTIPLE_NODES

  sleep 2s

  msg2 "Disable FUNCTION_TRACER/GRAPH_TRACER"
  scripts/config --disable CONFIG_FUNCTION_TRACER
  scripts/config --disable CONFIG_STACK_TRACER

  sleep 2s

  msg2 "Disable CONFIG_USER_NS_UNPRIVILEGED"
  scripts/config --enable CONFIG_USER_NS_UNPRIVILEGED

  sleep 2s

  msg2 "Set CPU Frequency scaling CONFIG_CPU_FREQ_DEFAULT_GOV/CONFIG_CPU_FREQ_GOV for performance"
  scripts/config --disable CONFIG_CPU_FREQ_DEFAULT_GOV_POWERSAVE
  scripts/config --disable CONFIG_CPU_FREQ_GOV_POWERSAVE
  scripts/config --disable CONFIG_CPU_FREQ_DEFAULT_GOV_USERSPACE
  scripts/config --disable CONFIG_CPU_FREQ_GOV_USERSPACE
  scripts/config --disable CONFIG_CPU_FREQ_DEFAULT_GOV_ONDEMAND
  scripts/config --disable CONFIG_CPU_FREQ_GOV_ONDEMAND
  scripts/config --disable CONFIG_CPU_FREQ_DEFAULT_GOV_CONSERVATIVE
  scripts/config --disable CONFIG_CPU_FREQ_GOV_CONSERVATIVE
  scripts/config --disable CONFIG_CPU_FREQ_DEFAULT_GOV_SCHEDUTIL
  scripts/config --disable CONFIG_CPU_FREQ_GOV_SCHEDUTIL
  scripts/config --enable CONFIG_CPU_FREQ_DEFAULT_GOV_PERFORMANCE
  scripts/config --enable CONFIG_CPU_FREQ_GOV_PERFORMANCE

  sleep 2s

  msg2 "Set CPU DEVFREQ GOV CONFIG_DEVFREQ_GOV for performance"
  scripts/config --disable CONFIG_DEVFREQ_GOV_SIMPLE_ONDEMAND
  scripts/config --undefine CONFIG_DEVFREQ_GOV_SIMPLE_ONDEMAND
  scripts/config --disable CONFIG_DEVFREQ_GOV_POWERSAVE
  scripts/config --disable CONFIG_DEVFREQ_GOV_USERSPACE
  scripts/config --disable CONFIG_DEVFREQ_GOV_PASSIVE
  scripts/config --enable CONFIG_DEVFREQ_GOV_PERFORMANCE

  sleep 2s

  msg2 "Set PCIEASPM DRIVER to performance"
  scripts/config --enable CONFIG_PCIEASPM
  scripts/config --enable CONFIG_PCIEASPM_PERFORMANCE

  sleep 2s

  msg2 "Set CONFIG_PCIE_BUS for performance"
  scripts/config --enable CONFIG_PCIE_BUS_PERFORMANCE

  sleep 2s

  msg2 "Enable CONFIG_CC_OPTIMIZE_FOR_PERFORMANCE_O3"
  scripts/config --disable CONFIG_CC_OPTIMIZE_BASAL
  scripts/config --disable CONFIG_CC_OPTIMIZE_FOR_PERFORMANCE
  scripts/config --disable CONFIG_CC_OPTIMIZE_FOR_SIZE
  scripts/config --enable CONFIG_CC_OPTIMIZE_FOR_PERFORMANCE_O3

  sleep 2s

  if [[ $_cpu_sched = "1" ]] || [[ $_cpu_sched = "2" ]]; then
    msg2 "Set timer frequency to 2000HZ"
    scripts/config --enable CONFIG_HZ_2000
    scripts/config --set-val CONFIG_HZ 2000
  else
    msg2 "Set timer frequency to 1000HZ"
    scripts/config --enable CONFIG_HZ_1000
    scripts/config --set-val CONFIG_HZ 1000
  fi

  sleep 2s

  msg2 "Enable PREEMPT"
  scripts/config --disable CONFIG_PREEMPT_NONE
  scripts/config --disable CONFIG_PREEMPT_VOLUNTARY
  scripts/config --enable CONFIG_PREEMPT
  scripts/config --enable CONFIG_PREEMPT_COUNT
  scripts/config --enable CONFIG_PREEMPTION

  sleep 2s

  msg2 "Enable CONFIG_FORCE_IRQ_THREADING"
  scripts/config --enable CONFIG_FORCE_IRQ_THREADING

  sleep 2s

  msg2 "Set to full tickless"
  scripts/config --disable CONFIG_HZ_PERIODIC
  scripts/config --disable CONFIG_NO_HZ_IDLE
  scripts/config --enable CONFIG_NO_HZ_FULL
  scripts/config --enable CONFIG_NO_HZ
  scripts/config --enable CONFIG_NO_HZ_COMMON
  #scripts/config --enable CONFIG_CONTEXT_TRACKING
  #scripts/config --disable CONFIG_CONTEXT_TRACKING_FORCE

  sleep 2s

  msg2 "Enable tristate V4L2 loopback device"
  scripts/config --module CONFIG_V4L2_LOOPBACK

  sleep 2s

  msg2 "Enable ntfs"
  scripts/config --module CONFIG_NTFS_FS
  scripts/config --enable CONFIG_NTFS_RW
  msg2 "Enable ntfs3"
  scripts/config --module CONFIG_NTFS3_FS
  scripts/config --enable CONFIG_NTFS3_64BIT_CLUSTER
  scripts/config --enable CONFIG_NTFS3_LZX_XPRESS
  scripts/config --enable CONFIG_NTFS3_FS_POSIX_ACL

  sleep 2s

  msg2 "Enable BBR/BBR2 TCP"
  scripts/config --module CONFIG_TCP_CONG_BBR
  scripts/config --module CONFIG_TCP_CONG_BBR2

  sleep 2s

  msg2 "Enable CONFIG_VHBA"
  scripts/config --module CONFIG_VHBA

  sleep 2s

  msg2 "Disabling Kyber I/O scheduler"
  scripts/config --disable CONFIG_MQ_IOSCHED_KYBER

  sleep 2s

  msg2 "Enable Deadline I/O scheduler"
  scripts/config --enable CONFIG_MQ_IOSCHED_DEADLINE

  sleep 2s

  msg2 "Enable MQ-Deadline-Nodefault I/O scheduler"
  scripts/config --enable CONFIG_MQ_IOSCHED_DEADLINE_NODEFAULT

  sleep 2s

  msg2 "Enable CONFIG_BFQ_CGROUP_DEBUG"
  scripts/config --enable CONFIG_BFQ_CGROUP_DEBUG

  sleep 2s

  msg2 "Enable CONFIG_SCHED_AUTOGROUP_DEFAULT_ENABLED"
  scripts/config --enable CONFIG_SCHED_AUTOGROUP_DEFAULT_ENABLED

  sleep 2s

  if [[ $_cpu_sched != "1" ]] && [[ $_cpu_sched != "2" ]]; then
    msg2 "Enable ZEN_INTERACTIVE"
    scripts/config --enable ZEN_INTERACTIVE

    sleep 2s
  fi

  msg2 "Enable Fsync support"
  scripts/config --enable CONFIG_FUTEX
  scripts/config --enable CONFIG_FUTEX_PI

  sleep 2s

  msg2 "Enable Futex2 support"
  scripts/config --enable CONFIG_FUTEX2

  sleep 2s

  msg2 "Enable winesync support"
  scripts/config --module CONFIG_WINESYNC

  sleep 2s

  msg2 "Enable SECURITY_FORK_BRUT"
  scripts/config --enable CONFIG_SECURITY_FORK_BRUT

  sleep 2s

  msg2 "Enable OpenRGB SMBus access"
  scripts/config --module CONFIG_I2C_NCT677

  sleep 2s

  msg2 "Enable LRNG"
  scripts/config --enable CONFIG_LRNG
  scripts/config --enable CONFIG_LRNG_OVERSAMPLE_ENTROPY_SOURCES
  scripts/config --enable CONFIG_LRNG_CONTINUOUS_COMPRESSION_ENABLED
  scripts/config --enable CONFIG_LRNG_ENABLE_CONTINUOUS_COMPRESSION
  scripts/config --enable CONFIG_LRNG_SWITCHABLE_CONTINUOUS_COMPRESSION
  scripts/config --enable CONFIG_LRNG_COLLECTION_SIZE_1024

  sleep 2s

  msg2 "Enable UNEVICTABLE_FILE"
  scripts/config --enable CONFIG_UNEVICTABLE_FILE

  sleep 2s

  #msg2 "Enable LRU"
  #scripts/config --enable CONFIG_LRU_GEN
  #scripts/config --enable CONFIG_LRU_GEN_ENABLED
  #scripts/config --enable CONFIG_LRU_GEN_STATS

  #sleep 2s

  if [[ $_cpu_sched = "1" ]]; then
    msg2 "Enable CacULE CPU scheduler"
    scripts/config --enable CONFIG_CACULE_SCHED
  elif [[ $_cpu_sched = "2" ]]; then
    msg2 "Enable CacULE CPU scheduler"
    scripts/config --enable CONFIG_CACULE_SCHED
    msg2 "Enable CacULE-RDB CPU scheduler"
    scripts/config --enable CONFIG_CACULE_RDB
  elif [[ $_cpu_sched = "3" ]]; then
    msg2 "Enable CONFIG_SCHED_ALT, this feature enable alternative CPU scheduler"
    scripts/config --enable CONFIG_SCHED_ALT
    msg2 "Enable BMQ CPU scheduler"
    scripts/config --enable CONFIG_SCHED_BMQ
    scripts/config --disable CONFIG_SCHED_PDS
  elif [[ $_cpu_sched = "4" ]]; then
    msg2 "Enable CONFIG_SCHED_ALT, this feature enable alternative CPU scheduler"
    scripts/config --enable CONFIG_SCHED_ALT
    msg2 "Enable PDS CPU scheduler"
    scripts/config --disable CONFIG_SCHED_BMQ
    scripts/config --enable CONFIG_SCHED_PDS
  elif [[ $_cpu_sched = "5" ]]; then
    msg2 "Enable MuQSS"
    scripts/config --enable CONFIG_SCHED_MC
    scripts/config --enable CONFIG_SCHED_SMT
    scripts/config --enable CONFIG_SMP
    scripts/config --enable CONFIG_SCHED_MC_PRIO
    scripts/config --enable CONFIG_SCHED_MUQSS
    msg2 "Set to RQ_MC"
    scripts/config --enable CONFIG_RQ_MC
    scripts/config --set-val CONFIG_SHARERQ 2
  else
    msg2 "Enable CFS"
    scripts/config --enable SCHED_NORMAL
    scripts/config --enable SCHED_BATCH
    scripts/config --enable SCHED_IDLE
    scripts/config --enable CONFIG_CGROUP_SCHED
    scripts/config --enable CONFIG_FAIR_GROUP_SCHED
    scripts/config --enable CONFIG_CFS_BANDWIDTH
    scripts/config --enable CONFIG_SCHED_DEBUG
  fi

  sleep 2s

  if [[ $_cpu_sched = "1" ]] || [[ $_cpu_sched = "2" ]]; then
    msg2 "Apply suggested config by Hamad Al Marri for CacULE"
    # General Setup
    echo "General Setup"
    sleep 1s
    scripts/config --disable CONFIG_EXPERT
    # Note:
    # CONFIG_NO_HZ_FULL requires you to add
    # the boot parameter "nohz_full=" in your
    # grup. For example, in case your machine
    # has 8 CPUS, "nohz_full=1-7" makes
    # all CPUs (except CPU0) adaptive ticks.
    # Without "nohz_full=1-7", no benfit of
    # selecting CONFIG_NO_HZ_FULL
    #
    # Please see the discussion here:
    # https://github.com/hamadmarri/cacule-cpu-scheduler/discussions/23#discussioncomment-711456
    #scripts/config --enable CONFIG_NO_HZ_FULL

    scripts/config --enable CONFIG_PREEMPT
    scripts/config --enable CONFIG_SCHED_AUTOGROUP
    scripts/config --disable CONFIG_BSD_PROCESS_ACCT
    scripts/config --disable CONFIG_TASK_XACCT
    scripts/config --disable CONFIG_PSI
    scripts/config --disable CONFIG_MEMCG
    scripts/config --disable CONFIG_CGROUP_CPUACCT
    scripts/config --disable CONFIG_CGROUP_DEBUG
    scripts/config --disable CONFIG_CHECKPOINT_RESTORE
    scripts/config --disable CONFIG_SLAB_MERGE_DEFAULT
    scripts/config --disable CONFIG_SLAB_FREELIST_HARDENED
    scripts/config --disable CONFIG_SLUB_CPU_PARTIAL
    scripts/config --disable CONFIG_PROFILING
    # Processor type and features
    echo "Processor type and features"
    sleep 1s
    scripts/config --disable CONFIG_RETPOLINE
    scripts/config --disable CONFIG_X86_5LEVEL
    scripts/config --disable CONFIG_KEXEC
    scripts/config --disable CONFIG_KEXEC_FILE
    scripts/config --disable CONFIG_CRASH_DUMP
    #scripts/config --set-val CONFIG_NR_CPUS $(nproc)
    # if you are not using this kernel as guest in a virtual machine,
    # then disable CONFIG_HYPERVISOR_GUEST
    #./scripts/config --disable CONFIG_HYPERVISOR_GUEST
    # General architecture-dependent options
    echo "General architecture-dependent options"
    sleep 1s
    scripts/config --disable CONFIG_KPROBES
    # Kernel hacking
    echo "Kernel hacking"
    sleep 1s
    scripts/config --disable CONFIG_FTRACE
    scripts/config --disable CONFIG_DEBUG_KERNEL
    scripts/config --disable CONFIG_PAGE_EXTENSION
    scripts/config --set-val CONFIG_RCU_CPU_STALL_TIMEOUT 4
    scripts/config --disable CONFIG_PRINTK_TIME
    scripts/config --disable CONFIG_DEBUG_INFO
    scripts/config --disable CONFIG_ENABLE_MUST_CHECK
    scripts/config --disable CONFIG_STRIP_ASM_SYMS
    scripts/config --disable CONFIG_UNUSED_SYMBOLS
    scripts/config --disable CONFIG_DEBUG_FS
    scripts/config --disable CONFIG_OPTIMIZE_INLINING
    scripts/config --disable CONFIG_DEBUG_SECTION_MISMATCH
    scripts/config --disable CONFIG_SECTION_MISMATCH_WARN_ONLY
    scripts/config --disable CONFIG_STACK_VALIDATION
    scripts/config --disable CONFIG_DEBUG_FORCE_WEAK_PER_CPU
    scripts/config --disable CONFIG_MAGIC_SYSRQ
    scripts/config --disable CONFIG_MAGIC_SYSRQ_SERIAL
    scripts/config --disable CONFIG_PAGE_EXTENSION
    scripts/config --disable CONFIG_DEBUG_PAGEALLOC
    scripts/config --disable CONFIG_PAGE_OWNER
    scripts/config --disable CONFIG_DEBUG_MEMORY_INIT
    scripts/config --disable CONFIG_HARDLOCKUP_DETECTOR
    scripts/config --disable CONFIG_SOFTLOCKUP_DETECTOR
    scripts/config --disable CONFIG_DETECT_HUNG_TASK
    scripts/config --disable CONFIG_WQ_WATCHDOG
    scripts/config --set-val CONFIG_PANIC_TIMEOUT 10
    scripts/config --disable CONFIG_SCHED_DEBUG
    scripts/config --disable CONFIG_SCHEDSTATS
    scripts/config --disable CONFIG_SCHED_STACK_END_CHECK
    scripts/config --disable CONFIG_STACKTRACE
    scripts/config --disable CONFIG_DEBUG_BUGVERBOSE
    scripts/config --set-val CONFIG_RCU_CPU_STALL_TIMEOUT 4
    scripts/config --disable CONFIG_RCU_TRACE
    scripts/config --disable CONFIG_FAULT_INJECTION
    scripts/config --disable CONFIG_LATENCYTOP
    scripts/config --disable CONFIG_PROVIDE_OHCI1394_DMA_INIT
    scripts/config --disable RUNTIME_TESTING_MENU
    scripts/config --disable CONFIG_MEMTEST
    scripts/config --disable CONFIG_KGDB
    scripts/config --disable CONFIG_EARLY_PRINTK
    scripts/config --disable CONFIG_DOUBLEFAULT

    sleep 2s
  fi

  msg2 "Set CONFIG_GENERIC_CPU"
  scripts/config --enable CONFIG_GENERIC_CPU

  sleep 2s

  # Setting localversion
  msg2 "Setting localversion..."
  scripts/setlocalversion --save-scmversion
  echo "-${pkgbase}" > localversion

  # Config
  if [[ "$_compiler" = "1" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} olddefconfig
  elif [[ "$_compiler" = "2" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} LLVM=1 LLVM_IAS=1 HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} olddefconfig
  fi

  make -s kernelrelease > version
  msg2 "Prepared $pkgbase version $(<version)"
}

build(){

  cd "${srcdir}"/linux-$pkgver

  # make -j$(nproc) all
  msg2 "make -j$(nproc) all..."
  if [[ "$_compiler" = "1" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} -j$(nproc) all
  elif [[ "$_compiler" = "2" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} LLVM=1 LLVM_IAS=1 HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} -j$(nproc) all
  fi
}

_package(){
  pkgdesc="Latest stable linux kernel and modules"
  depends=("coreutils" "kmod" "initramfs" "mkinitcpio")
  optdepends=("linux-firmware: firmware images needed for some devices"
              "crda: to set the correct wireless channels of your country"
              "winesync-headers: headers file for winesync module")
  provides=("VIRTUALBOX-GUEST-MODULES" "WIREGUARD-MODULE")

  cd "${srcdir}"/linux-$pkgver

  local kernver="$(<version)"
  local modulesdir="${pkgdir}"/usr/lib/modules/${kernver}

  msg2 "Installing boot image..."
  # systemd expects to find the kernel here to allow hibernation
  # https://github.com/systemd/systemd/commit/edda44605f06a41fb86b7ab8128dcf99161d2344
  #install -Dm644 arch/${ARCH}/boot/bzImage "$modulesdir/vmlinuz"
  msg2 "install -Dm644 "$(make -s image_name)" "$modulesdir/vmlinuz""
  install -Dm644 "$(make -s image_name)" "$modulesdir/vmlinuz"

  # Used by mkinitcpio to name the kernel
  msg2 "echo "$pkgbase" | install -Dm644 /dev/stdin "$modulesdir/pkgbase""
  echo "$pkgbase" | install -Dm644 /dev/stdin "$modulesdir/pkgbase"

  msg2 "Installing modules..."
  if [[ "$_compiler" = "1" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} INSTALL_MOD_PATH="${pkgdir}"/usr INSTALL_MOD_STRIP=1 -j$(nproc) modules_install
  elif [[ "$_compiler" = "2" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} LLVM=1 LLVM_IAS=1 HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} INSTALL_MOD_PATH="${pkgdir}"/usr INSTALL_MOD_STRIP=1 -j$(nproc) modules_install
  fi

  # remove build and source links
  msg2 "Remove build dir and source dir..."
  rm -rf "$modulesdir"/{source,build}
}

_package-headers(){
  pkgdesc="Headers and scripts for building modules for the $pkgbase package"
  depends=("${pkgbase}" "pahole")

  cd "${srcdir}"/linux-$pkgver

  local builddir="$pkgdir"/usr/lib/modules/"$(<version)"/build

  msg2 "Installing build files..."
  install -Dt "$builddir" -m644 .config Makefile Module.symvers System.map localversion version vmlinux
  install -Dt "$builddir/kernel" -m644 kernel/Makefile
  install -Dt "$builddir/arch/x86" -m644 arch/x86/Makefile
  cp -t "$builddir" -a scripts

  # add objtool for external module building and enabled VALIDATION_STACK option
  install -Dt "$builddir/tools/objtool" tools/objtool/objtool

  # add xfs and shmem for aufs building
  mkdir -p "$builddir"/{fs/xfs,mm}

  msg2 "Installing headers..."
  cp -t "$builddir" -a include
  cp -t "$builddir/arch/x86" -a arch/x86/include
  install -Dt "$builddir/arch/x86/kernel" -m644 arch/x86/kernel/asm-offsets.s

  install -Dt "$builddir/drivers/md" -m644 drivers/md/*.h
  install -Dt "$builddir/net/mac80211" -m644 net/mac80211/*.h

  # https://bugs.archlinux.org/task/13146
  install -Dt "$builddir/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # https://bugs.archlinux.org/task/20402
  install -Dt "$builddir/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "$builddir/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "$builddir/drivers/media/tuners" -m644 drivers/media/tuners/*.h

  # https://bugs.archlinux.org/task/71392
  install -Dt "$builddir/drivers/iio/common/hid-sensors" -m644 drivers/iio/common/hid-sensors/*.h

  msg2 "Installing KConfig files..."
  find . -name 'Kconfig*' -exec install -Dm644 {} "$builddir/{}" \;

  msg2 "Removing unneeded architectures..."
  local arch
  for arch in "$builddir"/arch/*/; do
    [[ $arch = */x86/ ]] && continue
    msg2 "Removing $(basename "$arch")"
    rm -r "$arch"
  done

  msg2 "Removing documentation..."
  rm -r "$builddir/Documentation"

  msg2 "Removing broken symlinks..."
  find -L "$builddir" -type l -printf 'Removing %P\n' -delete

  msg2 "Removing loose objects..."
  find "$builddir" -type f -name '*.o' -printf 'Removing %P\n' -delete

  msg2 "Stripping build tools..."
  local file
  while read -rd '' file; do
    case "$(file -bi "$file")" in
      application/x-sharedlib\;*)      # Libraries (.so)
        strip -v $STRIP_SHARED "$file" ;;
      application/x-archive\;*)        # Libraries (.a)
        strip -v $STRIP_STATIC "$file" ;;
      application/x-executable\;*)     # Binaries
        strip -v $STRIP_BINARIES "$file" ;;
      application/x-pie-executable\;*) # Relocatable binaries
        strip -v $STRIP_SHARED "$file" ;;
    esac
  done < <(find "$builddir" -type f -perm -u+x ! -name vmlinux -print0)

  msg2 "Stripping vmlinux..."
  strip -v $STRIP_STATIC "$builddir/vmlinux"

  msg2 "Adding symlink..."
  mkdir -p "$pkgdir/usr/src"
  ln -sr "$builddir" "$pkgdir/usr/src/$pkgbase"
}
