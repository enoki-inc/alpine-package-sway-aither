# Contributor: Henrik Riomar <henrik.riomar@gmail.com>
# Contributor: Antoine Fontaine <antoine.fontaine@epfl.ch>
# Contributor: Drew DeVault <sir@cmpwn.com>
# Contributor: Michał Polański <michal@polanski.me>
# Maintainer: Rohan Sukhdeo <rohan@enoki.so>
pkgname=sway-aither
pkgver=1.7
pkgrel=4
pkgdesc="i3 compatible window manager for Wayland with extended border colors"
url="https://swaywm.org"
arch="all"
license="MIT"
options="!check" # no test suite
makedepends="cairo-dev
	gdk-pixbuf-dev
	json-c-dev
	libevdev-dev
	libinput-dev
	libxkbcommon-dev
	linux-pam-dev
	meson
	ninja
	pango-dev
	pcre2-dev
	scdoc
	wayland-dev
	wayland-protocols
	wlroots-dev
	eudev-dev
	"
subpackages="
	$pkgname-dbg
	$pkgname-doc
	$pkgname-wallpapers::noarch
	$pkgname-bash-completion
	$pkgname-zsh-completion
	$pkgname-fish-completion
	grimshot::noarch
	grimshot-doc:grimshot_doc:noarch
	"
source="$pkgname-$pkgver.tar.gz::https://github.com/enoki-inc/sway-aither/archive/$pkgver.tar.gz
	sway.desktop
	remove-aports-git-version.patch
	no-werror.patch
	"

provides="sway-virtual"
provider_priority=100

build() {
	abuild-meson . output
	meson compile ${JOBS:+-j ${JOBS}} -C output
}

package() {
	DESTDIR="$pkgdir" meson install --no-rebuild -C output

	install -D -m644 "$srcdir"/sway.desktop \
		"$pkgdir"/usr/share/wayland-sessions/sway.desktop

	# move fish completion files where they are expected
	mv "$pkgdir"/usr/share/fish/vendor_completions.d "$pkgdir"/usr/share/fish/completions
}

wallpapers() {
	pkgdesc="Wallpapers for Sway"
	license="CC0-1.0"
	install_if="$pkgname=$pkgver-r$pkgrel"

	amove usr/share/backgrounds
}

grimshot() {
	pkgdesc="Script for taking screenshots with grim and slurp on Sway"
	depends="
		cmd:grim
		cmd:jq
		cmd:notify-send
		cmd:slurp
		cmd:wl-copy
		"

	install -Dm755 "$builddir"/contrib/grimshot \
		-t "$subpkgdir"/usr/bin
}

grimshot_doc() {
	install -Dm644 "$builddir"/contrib/grimshot.1 \
		-t "$subpkgdir"/usr/share/man/man1

	default_doc
	pkgdesc="Documentation for grimshot"
	install_if="docs ${subpkgname%-doc}=$pkgver-r$pkgrel"
}

sha512sums="
17139a3d6b5b66442d2114be89fba8924fbcde49ad686ee34a4efcce67fe9aa999532985c5e4f3bf3ff2593d9189850a84a2b4d6b671fa693918a380c839bd31  sway-aither-1.7.tar.gz
c9bc08fbd9d059c037ad1e3b7ab5e91bcde27dce248cc558c1f126b01c85b1d0d4ed4bb10e3f27bc818a06e60a81f19478b95529d4eeb32036e2c6ea9f29db36  sway.desktop
3081f34ff88be38889ace94489ff4dc97a3d2d8402a6f2e83e968b991db478b7d3329d1685697898d8e43761e83be0d7c348a5fee45fe41dbb77521cda7b5a72  remove-aports-git-version.patch
4118a0a9d9fd173ad337e44534f4a21d744ec99c51b5196e4be4fcd7aa0f8d4a3b107a41dc48a15856790f34701692f5e12f26e000a6cf8d2dfd28ebce03dac1  no-werror.patch
"
