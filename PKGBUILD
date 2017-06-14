pkgname=kubernetes-bin
pkgver=1.6.5
_contribver=0.7.0
pkgrel=1
pkgdesc="Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications."
depends=('glibc' 'bash' 'cni')
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
sha256sums=('68809d34d3429685eaafedf398f690bba1bcc1f793cd2d8193559b90492c68b1'
            'ab36d4243baf8cd47aba94f22f4c41a2980cf2ffca51ccda28b1e7685f500282'
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
