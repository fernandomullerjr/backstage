

## INSTALANDO O CILIUM NO KUBEADM

https://docs.cilium.io/en/stable/installation/k8s-install-kubeadm/
<https://docs.cilium.io/en/stable/installation/k8s-install-kubeadm/>


## RESUMO

helm install cilium cilium/cilium --namespace kube-system -f /home/fernando/cursos/cka-certified-kubernetes-administrator/Secao7-Security/148-x-cilium-values.yaml
    OBS:
    1. Garantir que o disco do Node tenha espaço suficiente para evitar taints de diskpressure.
    2. O values utilizado indica a quantidade de 1 replicas do cilium operator.




## INSTALANDO O CILIUM NO KUBEADM

helm install cilium cilium/cilium --namespace kube-system



root@debian10x64:/home/fernando# helm install cilium cilium/cilium --namespace kube-system
NAME: cilium
LAST DEPLOYED: Sat Oct 28 15:27:37 2023
NAMESPACE: kube-system
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
You have successfully installed Cilium with Hubble.

Your release version is 1.14.1.

For any further help, visit https://docs.cilium.io/en/v1.14/gettinghelp
root@debian10x64:/home/fernando#
root@debian10x64:/home/fernando#








root@debian10x64:/home/fernando# helm ls -A
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
cilium  kube-system     1               2023-10-28 15:27:37.254219442 -0300 -03 deployed        cilium-1.14.1   1.14.1
root@debian10x64:/home/fernando#






- Pods do cilium e do coredns em pending:

root@debian10x64:/home/fernando# kubectl get pods -A
NAMESPACE     NAME                                  READY   STATUS    RESTARTS   AGE
kube-system   cilium-operator-788c4f69bc-g7mw4      1/1     Running   0          74s
kube-system   cilium-operator-788c4f69bc-nktj2      0/1     Pending   0          74s
kube-system   cilium-q8xc5                          0/1     Running   0          74s
kube-system   coredns-5dd5756b68-btrs6              0/1     Pending   0          5m7s
kube-system   coredns-5dd5756b68-gpwnt              0/1     Pending   0          5m7s
kube-system   etcd-debian10x64                      1/1     Running   60         5m12s
kube-system   kube-apiserver-debian10x64            1/1     Running   0          5m9s
kube-system   kube-controller-manager-debian10x64   1/1     Running   0          5m9s
kube-system   kube-proxy-272d4                      1/1     Running   0          5m7s
kube-system   kube-scheduler-debian10x64            1/1     Running   0          5m9s
root@debian10x64:/home/fernando#
root@debian10x64:/home/fernando# date
Sat 28 Oct 2023 03:28:59 PM -03
root@debian10x64:/home/fernando#













helm upgrade --install cilium cilium/cilium -n kube-system -f /home/fernando/cursos/cka-certified-kubernetes-administrator/Secao7-Security/148-x-cilium-values.yaml


root@debian10x64:/home/fernando#
root@debian10x64:/home/fernando# helm upgrade --install cilium cilium/cilium -n kube-system -f /home/fernando/cursos/cka-certified-kubernetes-administrator/Secao7-Security/148-x-cilium-values.yaml
Release "cilium" has been upgraded. Happy Helming!
NAME: cilium
LAST DEPLOYED: Sat Oct 28 15:30:02 2023
NAMESPACE: kube-system
STATUS: deployed
REVISION: 2
TEST SUITE: None
NOTES:
You have successfully installed Cilium with Hubble.

Your release version is 1.14.1.

For any further help, visit https://docs.cilium.io/en/v1.14/gettinghelp
root@debian10x64:/home/fernando#
root@debian10x64:/home/fernando#
root@debian10x64:/home/fernando# date
Sat 28 Oct 2023 03:30:07 PM -03
root@debian10x64:/home/fernando#
root@debian10x64:/home/fernando#








- Pods do coredns em pending ainda


root@debian10x64:/home/fernando# kubectl get pods -A
NAMESPACE     NAME                                  READY   STATUS    RESTARTS   AGE
kube-system   cilium-operator-788c4f69bc-g7mw4      1/1     Running   0          2m39s
kube-system   cilium-q8xc5                          1/1     Running   0          2m39s
kube-system   coredns-5dd5756b68-btrs6              0/1     Pending   0          6m32s
kube-system   coredns-5dd5756b68-gpwnt              0/1     Pending   0          6m32s
kube-system   etcd-debian10x64                      1/1     Running   60         6m37s
kube-system   kube-apiserver-debian10x64            1/1     Running   0          6m34s
kube-system   kube-controller-manager-debian10x64   1/1     Running   0          6m34s
kube-system   kube-proxy-272d4                      1/1     Running   0          6m32s
kube-system   kube-scheduler-debian10x64            1/1     Running   0          6m34s
root@debian10x64:/home/fernando#










root@debian10x64:/home/fernando#
root@debian10x64:/home/fernando# kubectl describe pod coredns-5dd5756b68-btrs6 -n kube-system
Name:                 coredns-5dd5756b68-btrs6
Namespace:            kube-system

Events:
  Type     Reason            Age    From               Message
  ----     ------            ----   ----               -------
  Warning  FailedScheduling  7m17s  default-scheduler  0/1 nodes are available: 1 node(s) had untolerated taint {node.kubernetes.io/not-ready: }. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling..
  Warning  FailedScheduling  111s   default-scheduler  0/1 nodes are available: 1 node(s) had untolerated taint {node.kubernetes.io/disk-pressure: }. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling..
root@debian10x64:/home/fernando#



root@debian10x64:/home/fernando# kubectl get nodes
NAME          STATUS   ROLES           AGE     VERSION
debian10x64   Ready    control-plane   7m58s   v1.28.1
root@debian10x64:/home/fernando#








root@debian10x64:/home/fernando#
root@debian10x64:/home/fernando# kubectl describe node debian10x64
Name:               debian10x64
Roles:              control-plane
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=debian10x64
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/control-plane=
                    node.kubernetes.io/exclude-from-external-load-balancers=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/containerd/containerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sat, 28 Oct 2023 15:23:37 -0300
Taints:             node-role.kubernetes.io/control-plane:NoSchedule
                    node.kubernetes.io/disk-pressure:NoSchedule
Unschedulable:      false














- Efetuado resize do disco da VM de 60gb para 75gb
- Efetuado resize da sda3 de 20gb para 35gb
- Reboot da VM

~~~~bash
root@debian10x64:/home/fernando# lsblk
NAME                       MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
fd0                          2:0    1     4K  0 disk
loop0                        7:0    0 272.7M  1 loop /snap/kontena-lens/249
loop1                        7:1    0 105.8M  1 loop /snap/core/16091
loop2                        7:2    0  55.7M  1 loop /snap/core18/2790
loop3                        7:3    0  55.7M  1 loop /snap/core18/2785
loop4                        7:4    0 272.7M  1 loop /snap/kontena-lens/248
loop5                        7:5    0 116.7M  1 loop /snap/robo3t-snap/9
loop6                        7:6    0 105.8M  1 loop /snap/core/16202
sda                          8:0    0    75G  0 disk
├─sda1                       8:1    0   487M  0 part /boot
├─sda2                       8:2    0     1K  0 part
├─sda3                       8:3    0    35G  0 part
│ └─debian10x64--vg-root   254:0    0  73.6G  0 lvm  /
└─sda5                       8:5    0  39.5G  0 part
  ├─debian10x64--vg-root   254:0    0  73.6G  0 lvm  /
  └─debian10x64--vg-swap_1 254:1    0   976M  0 lvm
sr0                         11:0    1   336M  0 rom
root@debian10x64:/home/fernando#


root@debian10x64:/home/fernando# df -h
Filesystem                        Size  Used Avail Use% Mounted on
udev                              4.8G     0  4.8G   0% /dev
tmpfs                             982M   15M  968M   2% /run
/dev/mapper/debian10x64--vg-root   73G   54G   16G  78% /
~~~~





- Todos os pods em "running" agora:

~~~~bash
root@debian10x64:/home/fernando# kubectl get pods -A
NAMESPACE     NAME                                  READY   STATUS    RESTARTS        AGE
kube-system   cilium-operator-788c4f69bc-g7mw4      1/1     Running   2 (108s ago)    140m
kube-system   cilium-q8xc5                          1/1     Running   2 (108s ago)    140m
kube-system   coredns-5dd5756b68-btrs6              1/1     Running   3 (108s ago)    144m
kube-system   coredns-5dd5756b68-gpwnt              1/1     Running   3 (67s ago)     144m
kube-system   etcd-debian10x64                      1/1     Running   62 (108s ago)   144m
kube-system   kube-apiserver-debian10x64            1/1     Running   2 (108s ago)    144m
kube-system   kube-controller-manager-debian10x64   1/1     Running   2 (108s ago)    144m
kube-system   kube-proxy-272d4                      1/1     Running   2 (108s ago)    144m
kube-system   kube-scheduler-debian10x64            1/1     Running   2 (108s ago)    144m
root@debian10x64:/home/fernando#
root@debian10x64:/home/fernando# date
Sat 28 Oct 2023 05:48:02 PM -03
root@debian10x64:/home/fernando#
~~~~