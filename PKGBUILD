# _     _            _        _          _____
#| |__ | | __ _  ___| | _____| | ___   _|___ /
#| '_ \| |/ _` |/ __| |/ / __| |/ / | | | |_ \
#| |_) | | (_| | (__|   <\__ \   <| |_| |___) |
#|_.__/|_|\__,_|\___|_|\_\___/_|\_\\__, |____/
#                                  |___/

#Maintainer: blacksky3 <https://github.com/blacksky3>

major=5.4.2
libdrm_major=2.4.113
minor1=50402
minor2=1528701
ubuntu_ver=22.04
repo_folder_ver=5.4.2

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

# extracts a debian package
# $1: deb file to extract
extract_deb(){
  local tmpdir="$(basename "${1%.deb}")"
  rm -Rf "$tmpdir"
  mkdir "$tmpdir"
  cd "$tmpdir"
  ar x "$1"
  tar -C "${pkgdir}" -xf data.tar.xz
}

# move ubuntu specific /usr/lib/x86_64-linux-gnu to /usr/lib
# $1: debian package library dir (goes from opt/amdgpu or opt/amdgpu-pro and from x86_64 or i386)
# $2: arch package library dir (goes to usr/lib or usr/lib32)
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

# move copyright file to proper place and remove debian changelog
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
  #for i in "${pkgdir}/opt/amdgpu/bin/"*; do
  #  mv "$i" "$i"32
  #done

  # Remove bin dir
  rm -rf "${pkgdir}"/opt/amdgpu/bin
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

sha256sums=('8bfab1a5860b0c52bcc9089b24600f3d77f18fb9dac7d3955f623774f934873e'
            '3a27a205b465ba7017ec947bae5c767339dd858a0d9a2137bf6bee010877650a'
            '0ad27a95ed267b8a97075a933e4d45091599f99a16fceb8ad1768238b97093ca'
            '0f51ac8033ce3dbfd26f00cc3f0407ad06585fcf835f6505cb193678c6d115da'
            '929ceb3700838a50a1a283ea1b8f49a14c4c80b431704ec2e33073719077ae1d'
            '2f28e43606cd4aa3b3f72316da330f4ce4ab466949149fc692385a415739e0ef'
            '006b6d8d76b4cca28e783842170179c64b5cd4432160099da7be4b28e0fd4613'
            'cbad22a25787f95baffba282c05597a48f62461ac3283f81864266c5a4a89e24'
            '4bbf3dd09f57a22d997d7ec13c9bfaf3b80daec63dc60a45e6fd5671a30dbdeb'
            '8bc64eb4c483d10293c8bb2edcd756804a6180c7d023876a4d0d27601cc51899'
            'd48b12b03e9f1fcfd07507dd1b9e666bdd527ebe1e28fa1ed6812fbce4c7d1b1')
