# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: codedust
# Contributor: Dario Ostuni <dario.ostuni@gmail.com>
# Contributor: Clayton Craft <clayton at craftyguy dot net>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=setuptools-rust
pkgname="${_py}-${_pkg}"
pkgver=1.8.1
_commit=2aa1ca490de98631e660e87462e338023cd2c69c
pkgrel=1
_pkgdesc=(
  "Compile and distribute Python extensions"
  "written in rust as easily as if they were written in C."
)
pkgdesc="${_pkcdesc[*]}"
arch=(
  'any'
)
license=(
  'MIT'
)
_http="https://github.com"
_ns="PyO3"
url="${_http}/${_ns}/${_pkg}"
depends=(
  'rust'
  "${_py}-setuptools"
  "${_py}-semantic-version"
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-installer"
  "${_py}-wheel"
  "${_py}-setuptools-scm"
)
checkdepends=(
  "${_py}-pytest"
  "${_py}-pytest-benchmark"
  "${_py}-beautifulsoup4"
  "${_py}-lxml"
  "${_py}-cffi"
)
source=(
  "git+${url}.git#commit=$_commit"
)
sha512sums=(
  'SKIP'
)

build() {
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      build \
    -nw
}

check() {
  cd \
    "${_pkg}"
  for _dir \
    in examples/*; do
    pushd \
      "${_dir}"
    PYTHONPATH="$PWD/../.." \
    "${_py}" \
      -m build \
      -nw
    "${_py}" \
      -m \
        installer \
      -d \
        tmp_install \
      dist/*.whl
    [[ -d tests || -d test ]] && \
      PYTHONPATH="$PWD/tmp_install/usr/lib/python3.11/site-packages" \
      pytest \
        tests
    popd
  done
  pytest \
    --doctest-modules \
    setuptools_rust
}

package() {
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -Dm644 \
    LICENSE \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}

