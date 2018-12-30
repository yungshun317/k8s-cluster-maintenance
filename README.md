# Cluster maintenance
* Understand Kubernetes cluster upgrade process.

## Upgrade kubeadm cluster
### Determine which version to upgrade to
$ sudo apt-get update
$ sudo apt-cache policy kubeadm
kubeadm:
  Installed: 1.13.0-00
  Candidate: 1.13.3-00
  Version table:
     1.13.3-00 500
        500 https://apt.kubernetes.io kubernetes-xenial/main amd64 Packages
     1.13.2-00 500
        500 https://apt.kubernetes.io kubernetes-xenial/main amd64 Packages
     1.13.1-00 500
        500 https://apt.kubernetes.io kubernetes-xenial/main amd64 Packages
 *** 1.13.0-00 500
        500 https://apt.kubernetes.io kubernetes-xenial/main amd64 Packages
        100 /var/lib/dpkg/status

## Upgrade the control plane node
$ sudo apt-mark unhold kubeadm && sudo apt-get update && sudo apt-get install -y kubeadm=1.13.3-00 && sudo apt-mark hold kubeadm

$ kubeadm version
kubeadm version: &version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.3", GitCommit:"721bfa751924da8d1680787490c54b9179b1fed0", GitTreeState:"clean", BuildDate:"2019-02-01T20:05:53Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}

$ sudo kubeadm upgrade plan
[preflight] Running pre-flight checks.
[upgrade] Making sure the cluster is healthy:
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[upgrade] Fetching available versions to upgrade to
[upgrade/versions] Cluster version: v1.13.0
[upgrade/versions] kubeadm version: v1.13.3
[upgrade/versions] Latest stable version: v1.13.3
[upgrade/versions] Latest version in the v1.13 series: v1.13.3

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   CURRENT       AVAILABLE
Kubelet     3 x v1.13.0   v1.13.3

Upgrade to the latest version in the v1.13 series:

COMPONENT            CURRENT   AVAILABLE
API Server           v1.13.0   v1.13.3
Controller Manager   v1.13.0   v1.13.3
Scheduler            v1.13.0   v1.13.3
Kube Proxy           v1.13.0   v1.13.3
CoreDNS              1.2.6     1.2.6
Etcd                 3.2.24    3.2.24

You can now apply the upgrade by executing the following command:

	kubeadm upgrade apply v1.13.3

_____________________________________________________________________

$ sudo kubeadm upgrade apply v1.13.3
[preflight] Running pre-flight checks.
[upgrade] Making sure the cluster is healthy:
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[upgrade/apply] Respecting the --cri-socket flag that is set with higher priority than the config file.
[upgrade/version] You have chosen to change the cluster version to "v1.13.3"
[upgrade/versions] Cluster version: v1.13.0
[upgrade/versions] kubeadm version: v1.13.3
[upgrade/confirm] Are you sure you want to proceed with the upgrade? [y/N]: y
[upgrade/prepull] Will prepull images for components [kube-apiserver kube-controller-manager kube-scheduler etcd]
[upgrade/prepull] Prepulling image for component etcd.
[upgrade/prepull] Prepulling image for component kube-apiserver.
[upgrade/prepull] Prepulling image for component kube-controller-manager.
[upgrade/prepull] Prepulling image for component kube-scheduler.
[apiclient] Found 0 Pods for label selector k8s-app=upgrade-prepull-kube-scheduler
[apiclient] Found 0 Pods for label selector k8s-app=upgrade-prepull-kube-controller-manager
[apiclient] Found 0 Pods for label selector k8s-app=upgrade-prepull-kube-apiserver
[apiclient] Found 0 Pods for label selector k8s-app=upgrade-prepull-etcd
[apiclient] Found 1 Pods for label selector k8s-app=upgrade-prepull-kube-controller-manager
[apiclient] Found 1 Pods for label selector k8s-app=upgrade-prepull-kube-apiserver
[apiclient] Found 1 Pods for label selector k8s-app=upgrade-prepull-kube-scheduler
[apiclient] Found 1 Pods for label selector k8s-app=upgrade-prepull-etcd
[upgrade/prepull] Prepulled image for component etcd.
[upgrade/prepull] Prepulled image for component kube-apiserver.
[upgrade/prepull] Prepulled image for component kube-controller-manager.
[upgrade/prepull] Prepulled image for component kube-scheduler.
[upgrade/prepull] Successfully prepulled the images for all the control plane components
[upgrade/apply] Upgrading your Static Pod-hosted control plane to version "v1.13.3"...
Static pod: kube-apiserver-k8s-master hash: a4a4c5d65acd45ef331b3e80062c0e1d
Static pod: kube-controller-manager-k8s-master hash: 031b2ae76163df7ba750bbd6478a767f
Static pod: kube-scheduler-k8s-master hash: 69aa2b9af9c518ac6265f1e8dce289a0
[upgrade/staticpods] Writing new Static Pod manifests to "/etc/kubernetes/tmp/kubeadm-upgraded-manifests231576307"
[upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/kube-apiserver.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2019-02-17-17-38-05/kube-apiserver.yaml"
[upgrade/staticpods] Waiting for the kubelet to restart the component
[upgrade/staticpods] This might take a minute or longer depending on the component/version gap (timeout 5m0s)
Static pod: kube-apiserver-k8s-master hash: a4a4c5d65acd45ef331b3e80062c0e1d
Static pod: kube-apiserver-k8s-master hash: a4a4c5d65acd45ef331b3e80062c0e1d
Static pod: kube-apiserver-k8s-master hash: a4a4c5d65acd45ef331b3e80062c0e1d
Static pod: kube-apiserver-k8s-master hash: a4a4c5d65acd45ef331b3e80062c0e1d
Static pod: kube-apiserver-k8s-master hash: 1ee55e9a8c1cc393c6c3997acfacb46c
[apiclient] Found 1 Pods for label selector component=kube-apiserver
[upgrade/staticpods] Component "kube-apiserver" upgraded successfully!
[upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/kube-controller-manager.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2019-02-17-17-38-05/kube-controller-manager.yaml"
[upgrade/staticpods] Waiting for the kubelet to restart the component
[upgrade/staticpods] This might take a minute or longer depending on the component/version gap (timeout 5m0s)
Static pod: kube-controller-manager-k8s-master hash: 031b2ae76163df7ba750bbd6478a767f
Static pod: kube-controller-manager-k8s-master hash: 529d392e19e25d6fcd767d2f5215c961
[apiclient] Found 1 Pods for label selector component=kube-controller-manager
[upgrade/staticpods] Component "kube-controller-manager" upgraded successfully!
[upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/kube-scheduler.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2019-02-17-17-38-05/kube-scheduler.yaml"
[upgrade/staticpods] Waiting for the kubelet to restart the component
[upgrade/staticpods] This might take a minute or longer depending on the component/version gap (timeout 5m0s)
Static pod: kube-scheduler-k8s-master hash: 69aa2b9af9c518ac6265f1e8dce289a0
Static pod: kube-scheduler-k8s-master hash: b734fcc86501dde5579ce80285c0bf0c
[apiclient] Found 1 Pods for label selector component=kube-scheduler
[upgrade/staticpods] Component "kube-scheduler" upgraded successfully!
[uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.13" in namespace kube-system with the configuration for the kubelets in the cluster
[kubelet] Downloading configuration for the kubelet from the "kubelet-config-1.13" ConfigMap in the kube-system namespace
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "k8s-master" as an annotation
[bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

[upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.13.3". Enjoy!

[upgrade/kubelet] Now that your control plane is upgraded, please proceed with upgrading your kubelets if you haven't already done so.

$ sudo apt-mark unhold kubelet && sudo apt-get update && sudo apt-get install -y kubelet=1.13.3-00 && sudo apt-mark hold kubelet

### Upgrade kubectl on all nodes
$ sudo apt-mark unhold kubectl && sudo apt-get update && sudo apt-get install -y kubectl=1.13.3-00 && sudo apt-mark hold kubectl 

### Drain control plane and worker nodes
$ kubectl get no
NAME           STATUS   ROLES    AGE   VERSION
k8s-master     Ready    master   2d    v1.13.3
k8s-worker-1   Ready    <none>   2d    v1.13.0
k8s-worker-2   Ready    <none>   2d    v1.13.0

$ kubectl drain k8s-worker-1
node/k8s-worker-1 cordoned
error: unable to drain node "k8s-worker-1", aborting command...

There are pending nodes to be drained:
 k8s-worker-1
error: DaemonSet-managed pods (use --ignore-daemonsets to ignore): kube-flannel-ds-amd64-hhbx5, kube-proxy-zbt2p

$ kubectl drain k8s-worker-1 --ignore-daemonsets
node/k8s-worker-1 already cordoned
WARNING: Ignoring DaemonSet-managed pods: kube-flannel-ds-amd64-hhbx5, kube-proxy-zbt2p
pod/coredns-86c58d9df4-fkhz5 evicted
pod/coredns-86c58d9df4-952x2 evicted
pod/nginx-deployment-779fcd779f-j97dd evicted
pod/nginx-deployment-779fcd779f-cvqhg evicted
pod/kube-state-metrics-5dc69d8d89-9dslz evicted
node/k8s-worker-1 evicted

$ kubectl drain k8s-worker-2 --ignore-daemonsets
node/k8s-worker-2 cordoned
error: unable to drain node "k8s-worker-2", aborting command...

There are pending nodes to be drained:
 k8s-worker-2
error: pods with local storage (use --delete-local-data to override): metrics-server-76db6db868-qbczr; pods not managed by ReplicationController, ReplicaSet, Job, DaemonSet or StatefulSet (use --force to override): no-annotation

$ kubectl drain k8s-worker-2 --ignore-daemonsets --delete-local-data --force
node/k8s-worker-2 already cordoned
WARNING: Ignoring DaemonSet-managed pods: kube-flannel-ds-amd64-jvwmk, kube-proxy-5dfm9; Deleting pods with local storage: metrics-server-76db6db868-qbczr; Deleting pods not managed by ReplicationController, ReplicaSet, Job, DaemonSet or StatefulSet: no-annotation
pod/nginx-deployment-779fcd779f-9ddhw evicted
pod/nginx-deployment-779fcd779f-vr6hp evicted
pod/coredns-86c58d9df4-pg9qc evicted
pod/kube-state-metrics-5dc69d8d89-94qc6 evicted
pod/no-annotation evicted
pod/nginx-deployment-779fcd779f-c8mpj evicted
pod/nginx-deployment-779fcd779f-sbkbj evicted
pod/metrics-server-76db6db868-qbczr evicted
pod/nginx-deployment-779fcd779f-p525f evicted
node/k8s-worker-2 evicted

$ kubectl drain k8s-master --ignore-daemonsets
node/k8s-master cordoned
WARNING: Ignoring DaemonSet-managed pods: kube-flannel-ds-amd64-hrncf, kube-proxy-hlmcn
pod/coredns-86c58d9df4-wtwft evicted
pod/coredns-86c58d9df4-jv4p7 evicted
node/k8s-master evicted

### Upgrade the kubelet config on worker nodes
$ sudo kubeadm upgrade node config --kubelet-version v1.13.3
[kubelet] Downloading configuration for the kubelet from the "kubelet-config-1.13" ConfigMap in the kube-system namespace
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[upgrade] The configuration for this node was successfully updated!
[upgrade] Now you should go ahead and upgrade the kubelet package using your package manager.

### Upgrade kubeadm and kubelet on worker nodes
$ sudo apt-mark unhold kubeadm kubelet && sudo apt-get update && sudo apt-get install -y kubelet=1.13.3-00 kubeadm=1.13.3-00 && sudo apt-mark hold kubeadm kubelet

### Restart the kubelet for all nodes
$ sudo systemctl restart kubelet
$ sudo systemctl status kubelet

$ kubectl uncordon k8s-master
node/k8s-master uncordoned

$ kubectl get nodes
NAME           STATUS                     ROLES    AGE   VERSION
k8s-master     Ready                      master   2d    v1.13.3
k8s-worker-1   Ready,SchedulingDisabled   <none>   2d    v1.13.3
k8s-worker-2   Ready,SchedulingDisabled   <none>   2d    v1.13.3

$ kubectl uncordon k8s-worker-1
node/k8s-worker-1 uncordoned
$ kubectl uncordon k8s-worker-2
node/k8s-worker-2 uncordoned

$ kubectl get nodes
NAME           STATUS   ROLES    AGE   VERSION
k8s-master     Ready    master   2d    v1.13.3
k8s-worker-1   Ready    <none>   2d    v1.13.3
k8s-worker-2   Ready    <none>   2d    v1.13.3


