# Maintainer: Urs Wolfer <uwolfer @ fwo.ch>

pkgbase='intellij-idea-ultimate-edition'
pkgname=(intellij-idea-ultimate-edition intellij-idea-ultimate-edition-jre)
pkgver=2016.3.2
pkgrel=2
pkgdesc="An intelligent IDE for Java, Groovy and other programming languages with advanced refactoring features intensely focused on developer productivity."
arch=('any')
url="https://www.jetbrains.com/idea/"
license=('Commercial')
makedepends=('rsync')
options=(!strip)
source=(https://download.jetbrains.com/idea/ideaIU-$pkgver.tar.gz \
        jetbrains-idea.desktop
)
sha256sums=('aa636eb6ad9fe048c7ec1334ca5e23abc7004c8c12f28b531c2c89a67e49ed8e'
            '83af2ba8f9f14275a6684e79d6d4bd9b48cd852c047dacfc81324588fa2ff92b'
)

build() {
  mv "${srcdir}/idea-IU-"* "${srcdir}/vendor-package"
}

package_intellij-idea-ultimate-edition() {
  depends=('giflib' 'libxtst')
  optdepends=(
    'intellij-idea-ultimate-edition-jre: JetBrains custom Java Runtime (Recommended)'
    'java-runtime: JRE - Required if intellij-idea-ultimate-edition-jre is not installed'
  )
  backup=("usr/share/${pkgbase}/bin/idea.vmoptions" "usr/share/${pkgbase}/bin/idea64.vmoptions" "usr/share/${pkgbase}/bin/idea.properties")

  install -d -m755 "${pkgdir}/usr/share/${pkgbase}"
  rsync -rtl "${srcdir}/vendor-package/" "${pkgdir}/usr/share/${pkgbase}" \
    --exclude=/jre

  # create binary and desktop entry
  install -d -m755 "${pkgdir}/usr/bin"
  ln -s "/usr/share/${pkgbase}/bin/idea.sh" "${pkgdir}/usr/bin/${pkgbase}"
  install -D -m644 "${srcdir}/jetbrains-idea.desktop" "${pkgdir}/usr/share/applications/jetbrains-idea.desktop"
  install -D -m644 "${srcdir}/vendor-package/bin/idea.png" "${pkgdir}/usr/share/pixmaps/${pkgbase}.png"

  # workaround FS#40934
  sed -i 's|lcd|on|'  "$pkgdir"/usr/share/"$pkgbase"/bin/*.vmoptions
}

package_intellij-idea-ultimate-edition-jre() {
  pkgdesc='JRE from IntelliJ IDEA, recommended instead of a system-wide installed JRE'
  arch=('i686' 'x86_64')
  depends=("${pkgbase}")

  install -d -m755 "${pkgdir}/usr/share/${pkgbase}"
  rsync -rtl "${srcdir}/vendor-package/jre/" "${pkgdir}/usr/share/${pkgbase}/jre"
}

# vim:set ts=2 sw=2 et:
