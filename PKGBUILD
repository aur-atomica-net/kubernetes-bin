pkgname=kubernetes-bin
pkgver=1.6.0
_contribver=0.7.0
pkgrel=1
pkgdesc="Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications."
depends=('glibc' 'bash')
makedepends=()
optdepends=('etcd: etcd cluster required to run Kubernetes')
arch=('x86_64')
source=("https://dl.k8s.io/v${pkgver}/kubernetes-server-linux-amd64.tar.gz"
	"https://github.com/kubernetes/contrib/archive/$_contribver.tar.gz"
    "kubernetes.install")
url="http://kubernetes.io/"
license=("APACHE")
provides=('kubernetes')
conflicts=('kubernetes')
backup=('etc/kubernetes/apiserver'
	'etc/kubernetes/config'
	'etc/kubernetes/controller-manager'
	'etc/kubernetes/kubelet'
	'etc/kubernetes/proxy'
	'etc/kubernetes/scheduler')
install=kubernetes.install
sha256sums=('3625b63d573aa4d28eaa30b291017f775f2ddc0523f40d25023ad1da9c30390e'
            '1d4e651ea59ea0d2b440e290fda5e166a21847891abca2907b8a1683c2252b8d'
            'f40b4b14a71f8138de69021e967d993e8b14db2cebe66eee20c7e66839ad1fde')

package() {
    cd "$srcdir/kubernetes"
    binaries=(cloud-controller-manager hyperkube kube-aggregator kube-apiserver kube-controller-manager kube-proxy kube-scheduler kubeadm kubectl kubefed kubelet)
    for bin in "${binaries[@]}"; do
        install -Dm755 server/bin/$bin "$pkgdir/usr/bin/$bin"
    done

    # install the place the kubelet defaults to put volumes
    install -d $pkgdir/var/lib/kubelet

    cd $srcdir/contrib-$_contribver

    # install config files
    install -dm 755 $pkgdir/etc/kubernetes/
    install -m 644 -t $pkgdir/etc/kubernetes/ init/systemd/environ/*

    # install service files
    install -dm 755 $pkgdir/usr/lib/systemd/system
    install -m 644 -t $pkgdir/usr/lib/systemd/system init/systemd/*.service

    install -dm 755 $pkgdir/usr/lib/tmpfiles.d
    install -m 644 -t $pkgdir/usr/lib/tmpfiles.d init/systemd/tmpfiles.d/*.conf
}
