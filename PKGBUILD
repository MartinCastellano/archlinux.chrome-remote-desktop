# Maintainer: Fredy García <frealgagu at gmail dot com>
# Contributor: klepz <felipe.junger@aluno.ufabc.edu.br>
# Contributor: Dave Blair <mail@dave-blair.de>
# Contributor: James An <james@jamesan.ca>
# Contributor: Mateus Rodrigues Costa <charles [dot] costar [at] gmail [dot] com>

pkgname=chrome-remote-desktop
pkgver=93.0.4577.16
pkgrel=1
pkgdesc="Access other computers or allow another user to access your computer securely over the Internet"
arch=("x86_64")
url="https://remotedesktop.google.com"
license=("BSD")
depends=("gtk3" "libxss" "nss" "python-psutil" "xorg-server-xvfb" "xorg-setxkbmap" "xorg-xauth" "xorg-xdpyinfo" "xorg-xrandr")
install="${pkgname}.install"
source=(
  "${pkgname}-${pkgver}.deb::https://dl.google.com/linux/${pkgname}/deb/pool/main/${pkgname:0:1}/${pkgname}/${pkgname}_${pkgver}_amd64.deb"
  "${pkgname}.service"
  "pamrule"
  "crd"
)
sha256sums=(
  "3b01306bff19cd9ccddf61bbd636b0a82aa394b25b8da8349b53c81bf825a498"
  "e5da5ae89b5bc599f72f415d1523341b25357931b0de46159fce50ab83615a4b"
  "fcc38269eb1cc902abff9688eda9377a22367e39b9f111f87c0dd8e77adb82e2"
  "27dee2d383e6bd993fe0557d5c222fa80ab6d16d43775dedff6218713c7a1c06"
)

# curl -qs https://dl.google.com/linux/chrome-remote-desktop/deb/dists/stable/main/binary-amd64/Packages | grep "^Version\|^SHA256" | awk '{print $2}'

build() {
  cd "${srcdir}"

  bsdtar -xf data.tar.xz -C .
  
  # Removing unnecessary .deb related files
  rm -R "${srcdir}/etc/cron.daily"
  rm -R "${srcdir}/etc/init.d"
  rm -R "${srcdir}/etc/pam.d"
}

package() {
  cd "${srcdir}"

  install -d "${pkgdir}/etc"
  install -d "${pkgdir}/opt"
  cp -r "${srcdir}/etc/"* "${pkgdir}/etc"
  cp -r "${srcdir}/opt/"* "${pkgdir}/opt"
  install -Dm644 "${srcdir}/usr/share/doc/${pkgname}/copyright" "${pkgdir}/usr/share/licenses/${pkgname}/copyright"
  install -Dm644 "${srcdir}/${pkgname}.service" "${pkgdir}/usr/lib/systemd/user/${pkgname}.service"
  install -Dm644 "${srcdir}/lib/systemd/system/${pkgname}@.service" "${pkgdir}/usr/lib/systemd/system/${pkgname}@.service"
  install -Dm644 "${srcdir}/pamrule" "${pkgdir}/etc/pam.d/${pkgname}"
  install -Dm755 "${srcdir}/crd" "${pkgdir}/usr/bin/crd"
  install -dm755 "${pkgdir}/etc/chromium/native-messaging-hosts"
  
  for _file in $(find "${pkgdir}/etc/opt/chrome/native-messaging-hosts" -type f); do
    local _filename=$(basename ${_file})
    if [[ ! -f "/etc/chromium/native-messaging-hosts/${_filename}" ]]; then
      ln -s "/etc/opt/chrome/native-messaging-hosts/${_filename}" "${pkgdir}/etc/chromium/native-messaging-hosts/${_filename}"
    fi
  done
  chmod u+s "${pkgdir}/opt/google/${pkgname}/user-session"
}
