# Maintainer: Daniel Kirchner <daniel at ekpyron dot org>
pkgname=mingw-w64-yaml-cpp-hg
pkgver=553
pkgrel=3
pkgdesc="YAML parser and emitter in C++, written around the YAML 1.2 spec (mingw-w64)"
license=('MIT')
arch=('any')
url="http://code.google.com/p/yaml-cpp/"
depends=('mingw-w64-boost' 'mingw-w64-crt')
makedepends=('cmake' 'mingw-w64-gcc' 'mercurial')
conflicts=('mingw-w64-yaml-cpp')
_hgroot="https://code.google.com/p"
_hgrepo="yaml-cpp"
options=(!strip !buildflags)

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  unset LDFLAGS

  cd "${srcdir}"
  msg "Connecting to Mercurial server...."

  if [[ -d "$_hgrepo" ]]; then
    cd "$_hgrepo"
    hg pull -u
    msg "The local files are updated."
  else
    hg clone "$_hgroot/$_hgrepo"
  fi
  
  msg "Starting build..."
  
  for _arch in ${_architectures}; do
    rm -rf "${srcdir}/build-${_arch}"
    mkdir "${srcdir}/build-${_arch}"
    cd "${srcdir}/build-${_arch}"
    
    echo "SET(CMAKE_SYSTEM_NAME Windows)" > win32.cmake
    echo "SET(CMAKE_C_COMPILER ${_arch}-gcc)" >> win32.cmake
    echo "SET(CMAKE_CXX_COMPILER ${_arch}-g++)" >> win32.cmake
    echo "SET(CMAKE_RC_COMPILER ${_arch}-windres)" >> win32.cmake
    echo "SET(CMAKE_FIND_ROOT_PATH /usr/${_arch})" >> win32.cmake
    echo "SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)" >> win32.cmake
    echo "SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)" >> win32.cmake
    echo "SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)" >> win32.cmake
    echo "SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)" >> win32.cmake
    cmake ../$_hgrepo \
      -DCMAKE_TOOLCHAIN_FILE=win32.cmake \
      -DCMAKE_INSTALL_PREFIX="/usr/${_arch}"
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/build-${_arch}"
    make install DESTDIR="${pkgdir}"
    rm -rf "${pkgdir}/usr/${_arch}/bin"
	${_arch}-strip -g "${pkgdir}/usr/${_arch}/lib/"*.a
  done
}
