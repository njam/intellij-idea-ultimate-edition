# Maintainer: Urs Wolfer <uwolfer @ fwo.ch>

pkgbase='intellij-idea-ultimate-edition'
pkgname=(
  'intellij-idea-ultimate-edition'
  'intellij-idea-ultimate-edition-jre-bundled'
  'intellij-idea-ultimate-edition-jre-system'
)
pkgver=2016.3.2
pkgrel=2
pkgdesc="An intelligent IDE for Java, Groovy and other programming languages with advanced refactoring features intensely focused on developer productivity."
arch=('any')
url="https://www.jetbrains.com/idea/"
license=('Commercial')
options=(!strip)
source=(
  "https://download.jetbrains.com/idea/ideaIU-${pkgver}.tar.gz"
  'jetbrains-idea.desktop'
)
sha256sums=(
  'aa636eb6ad9fe048c7ec1334ca5e23abc7004c8c12f28b531c2c89a67e49ed8e'
  '83af2ba8f9f14275a6684e79d6d4bd9b48cd852c047dacfc81324588fa2ff92b'
)

build() {
  mkdir -p "${srcdir}/vendor-package"
  mv "${srcdir}/idea-IU-"*"/jre" "${srcdir}/vendor-package/jre"
  mv "${srcdir}/idea-IU-"* "${srcdir}/vendor-package/base"
}

package_intellij-idea-ultimate-edition() {
  depends=("${pkgbase}-jre-meta" 'giflib' 'libxtst')
  backup=("usr/share/${pkgbase}/bin/idea.vmoptions" "usr/share/${pkgbase}/bin/idea64.vmoptions" "usr/share/${pkgbase}/bin/idea.properties")

  install -d -m755 "${pkgdir}/usr/share/${pkgbase}"
  cp -r "${srcdir}/vendor-package/base/." "${pkgdir}/usr/share/${pkgbase}"

  # create binary and desktop entry
  install -d -m755 "${pkgdir}/usr/bin"
  ln -s "/usr/share/${pkgbase}/bin/idea.sh" "${pkgdir}/usr/bin/${pkgbase}"
  install -D -m644 "${srcdir}/jetbrains-idea.desktop" "${pkgdir}/usr/share/applications/jetbrains-idea.desktop"
  install -D -m644 "${srcdir}/vendor-package/base/bin/idea.png" "${pkgdir}/usr/share/pixmaps/${pkgbase}.png"

  # workaround FS#40934
  sed -i 's|lcd|on|'  "${pkgdir}/usr/share/${pkgbase}/bin/"*".vmoptions"
}

package_intellij-idea-ultimate-edition-jre-bundled() {
  pkgdesc='JRE from IntelliJ IDEA, recommended instead of a system-wide installed JRE'
  arch=('i686' 'x86_64')
  provides=("${pkgbase}-jre-meta")

  install -d -m755 "${pkgdir}/usr/share/${pkgbase}"
  cp -r "${srcdir}/vendor-package/jre/." "${pkgdir}/usr/share/${pkgbase}/jre"
}

package_intellij-idea-ultimate-edition-jre-system() {
  pkgdesc="Meta package providing the default JRE for ${pkgbase}"
  depends=('java-runtime=8')
  provides=("${pkgbase}-jre-meta")
}

# vim:set ts=2 sw=2 et:
