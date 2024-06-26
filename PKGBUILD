# Maintainer: Chris Speck  <chris.speck(at)annalise(dot)ai>

pkgname=slack-desktop-wayland
_original_pkgname=slack-desktop
pkgver=4.36.140
pkgrel=1
pkgdesc="Slack Desktop (Beta) for Linux with Wayland Support"
arch=('x86_64')
url="https://slack.com/downloads"
license=('custom')
depends=('gtk3' 'libsecret' 'libxss' 'nss' 'pipewire' 'xdg-utils' 'xdg-desktop-portal')
optdepends=('libappindicator-gtk3: Systray indicator support'
    'org.freedesktop.secrets: Keyring password store support'
    'xdg-desktop-portal-gnome: xdg-desktop-portal support for GNOME'
    'xdg-desktop-portal-gtk: xdg-desktop-portal support for GNOME and GTK'
    'xdg-desktop-portal-kde: xdg-desktop-portal support for KDE'
    'xdg-desktop-portal-lxqt: xdg-desktop-portal support for LXQt'
    'xdg-desktop-portal-wlr: xdg-desktop-portal support for wlroots-based Wayland compositors'
)
source=("https://downloads.slack-edge.com/releases/linux/${pkgver}/prod/x64/${_original_pkgname}-${pkgver}-amd64.deb"
"${_original_pkgname}.patch")
noextract=("${_original_pkgname}-${pkgver}-amd64.deb")
b2sums=('2af0c4069d56c8dea6938034cf913414e3b865965bed0d3fce55bab5788c2bc1de8cf8824cd25979adfcd401b0132b45a6212507d7c6c04bbc6b0576de53f790'
        '556dfdffabf790b100813bb0ff34ee48e5ec0a9e40b701f52bc8dc2bcd82f1e7701877798e6764f12a611bdc33dfeca8af63915dd713d75bb7fef9e5aac053fe')
provides=('slack-desktop')
conflicts=('slack-desktop' 'slack-electron')

package() {
    bsdtar -O -xf "slack-desktop-${pkgver}"*.deb data.tar.xz | bsdtar -C "${pkgdir}" -xJf -

    # Fix hardcoded icon path in .desktop file
    patch -d "${pkgdir}" -p1 <"${_original_pkgname}".patch

    # Permission fix
    find "${pkgdir}" -type d -exec chmod 755 {} +

    # Remove all unnecessary stuff
    rm -rf "${pkgdir}/etc"
    rm -rf "${pkgdir}/usr/lib/slack/src"
    rm -rf "${pkgdir}/usr/share/lintian"
    rm -rf "${pkgdir}/usr/share/doc"

    # Move license
    install -dm755 "${pkgdir}/usr/share/licenses/${_original_pkgname}"
    mv "${pkgdir}/usr/lib/slack/LICENSE" "${pkgdir}/usr/share/licenses/${_original_pkgname}"
    ln -s "/usr/share/licenses/${_original_pkgname}/LICENSE" "${pkgdir}/usr/lib/slack/LICENSE"
}
