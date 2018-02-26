# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Contributor: Giovanni Scafora <giovanni@archlinux.org>
# Contributor: Sarah Hay <sarahhay@mb.sympatico.ca>
# Contributor: Martin Sandsmark <martin.sandsmark@kde.org>
# Contributor: Konstantin Gizdov <arch at kge dot pw>

pkgname=vlc
pkgver=3.0.0
pkgrel=2
pkgdesc='Multi-platform MPEG, VCD/DVD, and DivX player'
url='https://www.videolan.org/vlc/'
arch=('x86_64')
license=('LGPL2.1' 'GPL2')
depends=('a52dec' 'libdvbpsi' 'libxpm' 'libdca' 'libproxy' 'sdl_image' 'libdvdnav'
         'libtiger' 'lua' 'libmatroska' 'zvbi' 'taglib' 'libmpcdec' 'ffmpeg'
         'faad2' 'libupnp' 'libshout' 'libmad' 'libmpeg2' 'xcb-util-keysyms'
         'libtar' 'libxinerama' 'wayland-protocols' 'libsecret' 'libarchive'
         'qt5-base' 'qt5-x11extras' 'qt5-svg')
makedepends=('live-media' 'libnotify' 'libbluray' 'flac' 'kdelibs' 'libdc1394'
             'libavc1394' 'lirc' 'libcaca' 'gtk3' 'librsvg' 'portaudio'
             'libgme' 'xosd' 'projectm' 'twolame' 'aalib' 'libmtp' 'libdvdcss'
             'smbclient' 'libgoom2' 'vcdimager' 'opus' 'libssh2' 'mesa' 'protobuf'
             'opencv' 'libnfs' 'mpg123' 'schroedinger' 'gst-plugins-base-libs' 'libmicrodns')
optdepends=('avahi: service discovery using bonjour protocol'
            'libnotify: notification plugin'
            'gtk3: notification plugin'
            'ncurses: ncurses interface support'
            'gst-plugins-base-libs: for libgst plugins'
            'libdvdcss: decoding encrypted DVDs'
            'lirc: lirc control plugin'
            'libavc1394: devices using the 1394ta AV/C'
            'libdc1394: IEEE 1394 access plugin'
            'kdelibs: KDE Solid hardware integration'
            'libva-vdpau-driver: vdpau backend nvidia'
            'libva-intel-driver: backend intel cards'
            'libbluray: Blu-Ray video support'
            'flac: Free Lossless Audio Codec plugin'
            'portaudio: portaudio support'
            'twolame: TwoLAME mpeg2 encoder plugin'
            'projectm: ProjectM visualisation plugin'
            'libcaca: colored ASCII art video output'
            'libgme: libgme plugin'
            'librsvg: SVG plugin'
            'libgoom2: libgoom plugin'
            'vcdimager: navigate VCD with libvcdinfo'
            'aalib: ASCII art plugin'
            'libmtp: MTP devices support'
            'smbclient: SMB access plugin'
            'libcdio: audio CD playback support'
            'ttf-freefont: subtitle font '
            'ttf-dejavu: subtitle font'
            'opus: opus codec support'
            'libssh2: sftp access support'
            'opencv: opencv video support'
            'libnfs: NFS access support'
            'mpg123: mpg123 codec support'
            'schroedinger: schroedinger codec support'
            'protobuf: chromecast support'
            'lua-socket: http interface'
            'libmicrodns: Chromecast discovery')
conflicts=('vlc-plugin')
replaces=('vlc-plugin')
options=('!emptydirs')
source=(https://download.videolan.org/${pkgname}/${pkgver}/${pkgname}-${pkgver}.tar.xz{,.asc}
        update-vlc-plugin-cache.hook
        lua53_compat.patch)
sha512sums=('9bdc64e16ddd2e8d2693179f2fcac8462d7defff186262a049ba325ef00882fbd75a9d323b506ba06876a8168fd5e90319837c8dcd136b206161e67748c2a9f7'
            'SKIP'
            '80357bae69e32b353d3784932d854e294906798e14faffb87c3383c3b6f6bdc57cbabb9c6e3f3c1adf0f8ddbb24153e72104c963cf1934970c2983c96daef9df'
            '33cda373aa1fb3ee19a78748e2687f2b93c8662c9fda62ecd122a2e649df8edaceb54dda3991bc38c80737945a143a9e65baa2743a483bb737bb94cd590dc25f')
validpgpkeys=('65F7C6B4206BD057A7EB73787180713BE58D1ADC') # VideoLAN Release Signing Key

prepare() {
  cd ${pkgname}-${pkgver}
  sed -e 's:truetype/ttf-dejavu:TTF:g' -i modules/visualization/projectm.cpp
  sed -e 's|-Werror-implicit-function-declaration||g' -i configure
  patch -Np1 < "${srcdir}/lua53_compat.patch"
  sed 's|whoami|echo builduser|g' -i configure
  sed 's|hostname -f|echo arch|g' -i configure
}

build() {
  cd ${pkgname}-${pkgver}

  export CFLAGS+=" -I/usr/include/samba-4.0"
  export CPPFLAGS+=" -I/usr/include/samba-4.0"
  export CXXFLAGS+=" -std=c++11"
  export LUAC=/usr/bin/luac
  export LUA_LIBS="$(pkg-config --libs lua)"
  export RCC=/usr/bin/rcc-qt5

  ./configure --prefix=/usr \
              --sysconfdir=/etc \
              --disable-rpath \
              --enable-faad \
              --enable-nls \
              --enable-lirc \
              --enable-ncurses \
              --enable-realrtsp \
              --enable-aa \
              --enable-upnp \
              --enable-opus \
              --enable-sftp \
              --enable-wayland \
              --enable-opencv \
              --enable-chromecast \
              --enable-sout \
              --enable-microdns
  make
}

package() {
  cd "${pkgname}-${pkgver}"

  make DESTDIR="${pkgdir}" install

  for res in 16 32 48 128; do
    install -Dm 644 "${srcdir}/vlc-${pkgver}/share/icons/${res}x${res}/vlc.png" \
                     "${pkgdir}/usr/share/icons/hicolor/${res}x${res}/apps/vlc.png"
  done
  install -Dm 644 "${srcdir}/update-vlc-plugin-cache.hook" -t "${pkgdir}/usr/share/libalpm/hooks"
}

# vim: ts=2 sw=2 et:
