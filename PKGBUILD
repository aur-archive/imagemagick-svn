# Maintainer: Limao Luo <luolimao+AUR@gmail.com>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>

pkgname=imagemagick-svn
pkgver=7.0.0.0.12699
pkgrel=1
pkgdesc="An image viewing/manipulation program"
arch=(i686 x86_64)
url=https://www.imagemagick.org/
license=(custom)
depends=(djvulibre fontconfig lcms2 liblqr libtool libwebp)
makedepends=(ghostscript jasper libpng librsvg libwmf libxml2 openexr)
optdepends=('ghostscript: for Ghostscript support'
    'imagemagick-doc: full documentation'
    'jasper: for JPEG-2000 support'
    'libjpeg-turbo: for jpeg support'
    'libpng: for PNG support'
    'librsvg: for SVG support'
    'libwmf: for WMF support'
    'libxml2: for XML support'
    'openexr: for OpenEXR support')
provides=(${pkgname%-*}=$pkgver)
conflicts=(${pkgname%-*})
for i in coder colors delegates log magic mime policy thresholds type{,-dejavu,-ghostscript,-windows}; do
    backup+=("etc/ImageMagick-7/$i.xml")
done
options=(libtool !emptydirs !makeflags)
source=($pkgname::svn+$url/subversion/ImageMagick/trunk/
    libpng_mmx_patch_x86_64.patch
    perlmagick.rpath.patch)
sha256sums=('SKIP'
    '4f3ab23349fd3958a88eb09a7107e08c2c6f3953287907103ec48cfa83575e87'
    'ad2a0f18bce65bd1b7cb7f2144f891074570641d7acffb89e5354cbe6b67e751')
sha512sums=('SKIP'
    '8537c2e38cfcd1e7b14d08358c83385792e3e0ed0b9f1210d4dcfad0d416bc3c1311355661f9b020480e641a9b47761ab483dc556d24a30ff92ea557cd8e78f9'
    'dfb7a3543ed792749a74324106ca546c233a2d062c4c8c98f434916140670a9e9ee340af3bc9213f355fe5a8686456b1a4da8bba197d94cd647616829ed4e03e')

pkgver() {
    echo $(grep -o -m1 '[0-9.-]\+' $pkgname/ChangeLog | sed -n '2p' | tr '-' '.').$(svnversion "$SRCDEST"/$pkgname/ | tr -d 'a-zA-z')
}

prepare() {
    cd $pkgname/
    [[ $CARCH == "x86_64" ]] && patch -p1 -i ../libpng_mmx_patch_x86_64.patch
    patch -p1 -i ../perlmagick.rpath.patch
}

build() {
    cd $pkgname/
    ./configure \
        --prefix=/usr \
        --sysconfdir=/etc \
        --with-modules \
        --disable-static \
        --enable-openmp \
        --with-x \
        --with-wmf \
        --with-openexr \
        --with-xml \
        --with-gslib \
        --with-gs-font-dir=/usr/share/fonts/Type1 \
        --with-perl \
        --with-perl-options="INSTALLDIRS=vendor" \
        --without-gvc \
        --with-djvu \
        --without-autotrace \
        --with-jp2 \
        --with-jbig \
        --without-fpx \
        --without-dps \
        --without-fftw
    make
}

package() {
    cd $pkgname/
    make DESTDIR="$pkgdir" install
    install -d "$pkgdir"/usr/share/licenses/$pkgname/
    install -m644 LICENSE NOTICE "$pkgdir"/usr/share/licenses/$pkgname/
    rm -rf "$pkgdir"/usr/share/doc/
}
