# _     _            _        _          _____
#| |__ | | __ _  ___| | _____| | ___   _|___ /
#| '_ \| |/ _` |/ __| |/ / __| |/ / | | | |_ \
#| |_) | | (_| | (__|   <\__ \   <| |_| |___) |
#|_.__/|_|\__,_|\___|_|\_\___/_|\_\\__, |____/
#                                  |___/

#Maintainer: blacksky3 <https://github.com/blacksky3>

major=5.4.3
libdrm_major=2.4.113
minor1=50403
minor2=1538762
ubuntu_ver=22.04
repo_folder_ver=5.4.3

pkgbase=libdrm-amdgpu
pkgname=(libdrm-amdgpu lib32-libdrm-amdgpu)
pkgver=${major}
pkgrel=1
arch=(x86_64)
url='https://repo.radeon.com/amdgpu'
license=(MIT)
source=(https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm-amdgpu-amdgpu1_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm-amdgpu-amdgpu1_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm-amdgpu-dev_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm-amdgpu-dev_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm-amdgpu-radeon1_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm-amdgpu-radeon1_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm-amdgpu-utils_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm-amdgpu-utils_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm2-amdgpu_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu/libdrm2-amdgpu_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${repo_folder_ver}/ubuntu/pool/main/libd/libdrm-amdgpu-common/libdrm-amdgpu-common_1.0.0.${minor1}-${minor2}.${ubuntu_ver}_all.deb)

extract_deb(){
  local tmpdir="$(basename "${1%.deb}")"
  rm -Rf "$tmpdir"
  mkdir "$tmpdir"
  cd "$tmpdir"
  ar x "$1"
  tar -C "${pkgdir}" -xf data.tar.xz
}

move_libdir(){
  local deb_libdir="$1"
  local arch_libdir="$2"

  if [ -d "${pkgdir}/${deb_libdir}" ]; then
    if [ ! -d "${pkgdir}/${arch_libdir}" ]; then
      mkdir -p "${pkgdir}/${arch_libdir}"
    fi
    mv -t "${pkgdir}/${arch_libdir}/" "${pkgdir}/${deb_libdir}"/*
    find ${pkgdir} -type d -empty -delete
  fi
}

move_copyright(){
  find ${pkgdir}/usr/share/doc -name "changelog.Debian.gz" -delete
  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  find ${pkgdir}/usr/share/doc -name "copyright" -exec mv {} ${pkgdir}/usr/share/licenses/${pkgname} \;
  find ${pkgdir}/usr/share/doc -type d -empty -delete
}

package_libdrm-amdgpu(){
  pkgdesc='AMDGPU userspace interface to kernel DRM services'
  depends=('libpciaccess')

  extract_deb "${srcdir}"/libdrm-amdgpu-amdgpu1_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libdrm-amdgpu-dev_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libdrm-amdgpu-radeon1_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libdrm-amdgpu-utils_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libdrm2-amdgpu_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libdrm-amdgpu-common_1.0.0.${minor1}-${minor2}.${ubuntu_ver}_all.deb
  move_libdir "opt/amdgpu/lib/x86_64-linux-gnu" "opt/amdgpu/lib"
  move_copyright

  # Move lib/udev to usr/lib/udev
  mv "${pkgdir}"/lib "${pkgdir}"/usr

  sed -i 's|/x86_64-linux-gnu||' \
      "$pkgdir/opt/amdgpu/lib/pkgconfig/libdrm.pc" \
      "$pkgdir/opt/amdgpu/lib/pkgconfig/libdrm_amdgpu.pc" \
      "$pkgdir/opt/amdgpu/lib/pkgconfig/libdrm_radeon.pc"

  # Hide udev rules
  mv "${pkgdir}"/usr/lib/udev/rules.d/91-amdgpu-pro-modeset.rules "${pkgdir}"/usr/lib/udev/rules.d/91-amdgpu-pro-modeset.rules.hide
}

package_lib32-libdrm-amdgpu(){
  pkgdesc='AMDGPU userspace interface to kernel DRM services (32-bit)'
  depends=(lib32-libpciaccess libdrm-amdgpu=${major})

  extract_deb "${srcdir}"/libdrm-amdgpu-amdgpu1_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/libdrm-amdgpu-dev_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/libdrm-amdgpu-radeon1_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/libdrm-amdgpu-utils_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
  extract_deb "${srcdir}"/libdrm2-amdgpu_${libdrm_major}.${minor1}-${minor2}.${ubuntu_ver}_i386.deb
  move_libdir "opt/amdgpu/lib/i386-linux-gnu" "opt/amdgpu/lib32"
  move_copyright

  # append 32 at the end of binary files
  for i in "${pkgdir}/opt/amdgpu/bin/"*; do
    mv "$i" "$i"32
  done

  # Remove bin dir
  #rm -rf "${pkgdir}"/opt/amdgpu/bin
  # Remove lib/udev
  rm -rf "${pkgdir}"/lib
  # Remove include dir
  rm -rf "${pkgdir}"/opt/amdgpu/include
  # Remove share dir
  rm -rf "${pkgdir}"/opt/amdgpu/share

  sed -i 's|/i386-linux-gnu||' \
      "$pkgdir/opt/amdgpu/lib32/pkgconfig/libdrm.pc" \
      "$pkgdir/opt/amdgpu/lib32/pkgconfig/libdrm_amdgpu.pc" \
      "$pkgdir/opt/amdgpu/lib32/pkgconfig/libdrm_radeon.pc"
}

sha256sums=('00bdf8491f90e08875b7b5990a18793908acf725ea244930a3f89fd26efd0ac1'
            '323fc25f3967353b54da2a9c6461043b04913ce48586c4afbf4c0705ac660dfe'
            '1fd9df857396a2909f933989c063e32cb10fb3ee3dae8858bd8c5cbcea80c194'
            'fc099ec78562b615ae2e1b7577e44ed99d17a7bb7d302f7f3c0c671308d42bae'
            '25d48432f6128a9b8fab6b45486395429e6a228c7ae705450f3ef7137cff7a9e'
            'dc15538bfd4af9d3de3d63d0d8fcea66f24168de136bb0007517ee84dc2fc3c8'
            'c58c1f0b7458467db97530b4f956faa0eaa95868cc965a6ec1fbfac1793de28b'
            '6a3f645872b7f641c6deebe5a5389904465a80f8cb1ac4bd1390229a555150bd'
            'b0227b1e9f1e00e8d639b4da6fd916db9cbee9d0cdee75ad90579739ea8855d9'
            'deeec3c1dd9ba51a05b97de5d19e0703a2decce9630d248057446b1b64e7498f'
            '63a67c62627cbfd1bcb0acd41a9753d27a7bcc8825204a0d4e39394e0ed5e20f')
