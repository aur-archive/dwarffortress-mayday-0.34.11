# Maintainer: Mark Pustjens <pustjens@dds.nl>
# Contributor: Patrick Chilton <chpatrick _at_ gmail _dot_ com>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Daenyth <Daenyth+Arch [AT] gmail [DOT] com>
# Contributor: djnm <nmihalich [at} gmail dott com>
pkgname=dwarffortress-mayday-0.34.11
_pkgname=dwarffortress-mayday
pkgver=0.34.11
_dfver=34_11
_dfgver=34_11
pkgrel=1
pkgdesc="A single-player fantasy game. You control a dwarven outpost or an adventurer in a randomly generated persistent world. Mayday tileset."
arch=(i686 x86_64)
# WIP Thread: http://www.bay12forums.com/smf/index.php?topic=66142.0
url="http://mayday.w.staszic.waw.pl/df.php"
install="$_pkgname.install"
license=('custom:dwarffortress')
depends=(gtk2 glu sdl_image libsndfile openal sdl_ttf glew gcc-libs)
makedepends=(git cmake unzip git)
options=('!strip')
if [[ $CARCH == 'x86_64' ]]; then
  makedepends+=(gcc-multilib)
  depends=(gcc-libs-multilib lib32-gtk2 lib32-glu lib32-sdl_image lib32-libsndfile lib32-openal
           lib32-libxdamage lib32-ncurses lib32-sdl_ttf lib32-glew)
  optdepends=('lib32-nvidia-utils: If you have nvidia graphics'
              'lib32-catalyst-utils: If you have ATI graphics'
              'lib32-alsa-lib: for alsa sound'
              'lib32-libpulse: for pulse sound')
fi
backup=('opt/df_linux/data/init/colors.txt'
        'opt/df_linux/data/init/init.txt'
        'opt/df_linux/data/init/d_init.txt'
        'opt/df_linux/data/init/interface.txt')
provides=(dwarffortress-mayday)
conflicts=(dwarffortress-mayday)
noextract=(dfg_${_dfgver}_win.zip df_${_dfver}_linux.tar.bz2)
# I made a fucking github repo with the sole purpose of unfucking df a bit
# We try to compile whatever little bit of df is open source
source=(http://www.bay12games.com/dwarves/df_${_dfver}_linux.tar.bz2
        http://mayday.w.staszic.waw.pl/~mayday/upload/DFG/dfg_${_dfgver}_win.zip
        git://github.com/svenstaro/dwarf_fortress_unfuck.git
        dwarffortress
        dwarffortress.desktop
        dwarffortress.png)
sha256sums=('5d20f22621704354d663e6f335b51f4c90956c85c336e89d18d7dad80ec9e9ed'
            'a6cd13a272e0a991819fa9df6545f7b7369be34beb167fc4d75abbc533cdb688'
            'SKIP'
            '4a7cc39dabe4fa3c28fdb9bdff83f7cdf55de0beff700fd6ebe605dda2f81688'
            'e79e3d945c6cc0da58f4ca30a210c7bf1bc3149fd10406d1262a6214eb40445a'
            '83183abc70b11944720b0d86f4efd07468f786b03fa52fe429ca8e371f708e0f')

build() {
  cd $srcdir/dwarf_fortress_unfuck

  cmake .
  make
}

package() {
  cd "${srcdir}"
  tar -f "df_${_dfver}_linux.tar.bz2" -x \
  --exclude "df_linux/raw" \
  --exclude "df_linux/data/art" \
  --exclude "df_linux/data/init/colors.txt" \
  --exclude "df_linux/data/init/init.txt"

  unzip -qod df_linux "dfg_${_dfgver}_win.zip" \
    "raw/*" \
    "data/art/*" \
    "data/init/colors.txt" \
    "data/init/init.txt"

  cd $srcdir/df_linux
  install -dm755 $pkgdir/opt/
  cp -r $srcdir/df_linux $pkgdir/opt/$_pkgname
  rm -r $pkgdir/opt/$_pkgname/df $pkgdir/opt/$_pkgname/libs/* $pkgdir/opt/$_pkgname/g_src

  find $pkgdir/opt/$_pkgname -type d -exec chmod 755 {} +
  find $pkgdir/opt/$_pkgname -type f -exec chmod 644 {} +

  install -Dm755 $srcdir/df_linux/libs/Dwarf_Fortress $pkgdir/opt/$_pkgname/libs/Dwarf_Fortress
  install -Dm755 $srcdir/dwarf_fortress_unfuck/libgraphics.so $pkgdir/opt/$_pkgname/libs/libgraphics.so
  install -Dm755 $srcdir/dwarffortress $pkgdir/usr/bin/$_pkgname

  # No idea why we need this. Really. This isn't being loaded dynamically, it's not linked and
  # in general there is no indication this is being used. However, it doesn't work without this symlink.
  [[ $CARCH == "x86_64" ]] && ln -s /usr/lib32/libpng.so $pkgdir/opt/$_pkgname/libs/libpng.so.3
  [[ $CARCH == "i686" ]] && ln -s /usr/lib/libpng.so $pkgdir/opt/$_pkgname/libs/libpng.so.3

  # Desktop launcher with icon
  install -Dm644 $srcdir/dwarffortress.desktop $pkgdir/usr/share/applications/"$_pkgname".desktop
  install -Dm644 $srcdir/dwarffortress.png     $pkgdir/usr/share/pixmaps/"$_pkgname".png

  install -Dm644 $srcdir/df_linux/readme.txt $pkgdir/usr/share/licenses/$_pkgname/readme.txt

  # Set pkgname in runscript / desktop file
  sed -i "s/^pkgname=.*/pkgname=$_pkgname/" $pkgdir/usr/bin/$_pkgname
  sed -i "s/^exec=.*/exec=$_pkgname/" $pkgdir/usr/share/applications/$_pkgname.desktop
}

# vim:set ts=2 sw=2 et:
sha256sums=('720eda6c83e72fb1212a8eead0c39989ab5387bcf42dc4028a14e8df5bdd69e2'
            'fe332692de1d1020db40948aeb4aa9dcb8988646dcb9eec0b6ce78c2678df2d0'
            'SKIP'
            '4a7cc39dabe4fa3c28fdb9bdff83f7cdf55de0beff700fd6ebe605dda2f81688'
            'e79e3d945c6cc0da58f4ca30a210c7bf1bc3149fd10406d1262a6214eb40445a'
            '83183abc70b11944720b0d86f4efd07468f786b03fa52fe429ca8e371f708e0f')
