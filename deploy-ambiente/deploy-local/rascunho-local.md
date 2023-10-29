
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
##  Git

git status
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git add -u
git reset -- manifestos-k8s/backstage-secrets.yaml
git commit -m "Backstage lab."
git push
git status




# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## COMANDOS úteis

kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

kubectl get ingress --namespace=backstage

kubectl exec --namespace=backstage -ti backstage-6c949b9bc-m7jwv -- sh
kubectl exec --namespace=backstage -ti backstage-6c949b9bc-m7jwv -- sh



# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## RESUMO E DETALHES

- As variáveis referentes a host e port estão diferentes, entre o app-config.yaml e as variáveis de ambiente injetadas no Pod.
- Dentro do pod tem o "_SERVICE_" no nome de cada uma.
- Foi ajustado o app-config.yaml
usando esta Docker Image v2, tem o app-config ajustado:
fernandomj90/backstage-local:v2


# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## COMANDOS - SUBIR BACKSTAGE

- Subindo Backstage via Kubernetes:

https://backstage.io/docs/deployment/k8s/#creating-a-namespace
<https://backstage.io/docs/deployment/k8s/#creating-a-namespace>

cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos
kubectl apply -f namespace.yaml

kubectl apply -f postgres-secrets.yaml
kubectl get secrets -n backstage

kubectl apply -f postgres-storage.yaml
kubectl get pv -n backstage
kubectl get pvc -n backstage

kubectl apply -f postgres.yaml
kubectl get pods --namespace=backstage
kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash
psql -U $POSTGRES_USER

kubectl apply -f postgres-service.yaml
kubectl get services --namespace=backstage

kubectl apply -f backstage-secrets.yaml
kubectl get secret -n backstage

kubectl apply -f backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

kubectl logs --namespace=backstage -f backstage-854df67b6c-fmvcz -c backstage

kubectl apply -f backstage-service.yaml
kubectl get services --namespace=backstage
sudo kubectl port-forward --namespace=backstage svc/backstage 80:80
sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007
sudo kubectl port-forward --namespace=backstage svc/backstage 80:80


## DESTROY

cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos


kubectl delete -f backstage-service.yaml
kubectl delete -f backstage.yaml
kubectl delete -f backstage-secrets.yaml
kubectl delete -f postgres-service.yaml
kubectl delete -f postgres.yaml
kubectl delete -f postgres-storage.yaml
kubectl delete -f postgres-secrets.yaml
kubectl delete -f namespace.yaml

kubectl get all -n backstage
kubectl get ingress -n backstage







# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## COMANDOS ÚTEIS

cd ~/cursos/idp-devportal/backstage/docker/docker-compose
docker-compose build --no-cache

docker ps
docker-compose up -d
docker ps
docker container exec -ti node_app_teste bash

npx @backstage/create-app@latest

## BUILD
- Build
cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage
DOCKER_BUILDKIT=1 docker build -t backstage-local .

- Executando container
docker run -it -p 7007:7007 backstage-local

- Push de Container
https://dev.to/pmutua/pushing-a-docker-container-image-to-docker-hub-m3p
docker build -t <hub-user>/<repo-name>[:<tag>]
docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]
docker commit <existing-container> <hub-user>/<repo-name>[:<tag>] 
docker push <hub-user>/<repo-name>:<tag>

- RESUMO
Buildando e efetuando Push ao Docker Hub
cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-local:v1 .
docker push fernandomj90/backstage-mandragora:v1







# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 22/10/2023

- Buildar imagem publica no Docker Hub, com portas para acesso local.


https://backstage.io/docs/deployment/k8s/
<https://backstage.io/docs/deployment/k8s/>

Note that app.baseUrl and backend.baseUrl in your app-config.yaml should match what we're forwarding here (port omitted in this example since we're using the default HTTP port 80):

~~~~YAML
# app-config.yaml
app:
  baseUrl: http://localhost

organization:
  name: Spotify

backend:
  baseUrl: http://localhost
  listen:
    port: 7007
  cors:
    origin: http://localhost
~~~~

- Buildar imagem publica no Docker Hub, com portas para acesso local.

- RESUMO
Buildando e efetuando Push ao Docker Hub
cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-local:v1 .
docker push fernandomj90/backstage-local:v1




- Efetuando push

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$ docker push fernandomj90/backstage-mandragora:v1
The push refers to repository [docker.io/fernandomj90/backstage-mandragora]
An image does not exist locally with the tag: fernandomj90/backstage-mandragora
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$ docker image ls
REPOSITORY                     TAG       IMAGE ID       CREATED              SIZE
fernandomj90/backstage-local   v1        1c063b4716e1   About a minute ago   1.03GB
gcr.io/k8s-minikube/kicbase    v0.0.27   9fa1cc16ad6d   2 years ago          1.08GB
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$ docker push fernandomj90/backstage-local:v1
The push refers to repository [docker.io/fernandomj90/backstage-local]
212ef6207fb4: Pushed
ddc1f2099c41: Pushed
250ec92deefa: Pushed
a5473fc70d17: Pushed
6956edde6008: Pushed
93ef9f10d919: Pushed
239b4be40545: Mounted from library/node
b39d8975f2b5: Mounted from library/node
e8c1966637e9: Mounted from library/node
36f368d2835f: Mounted from library/node
8ce178ff9f34: Mounted from library/node
v1: digest: sha256:a547a2c00a3628f3f3631a1c96c5a3fd9b1036a634238c3d0dc13254fd3d5aa9 size: 2630
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage$

~~~~






- Subindo Backstage via Kubernetes:

https://backstage.io/docs/deployment/k8s/#creating-a-namespace
<https://backstage.io/docs/deployment/k8s/#creating-a-namespace>

cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos
kubectl apply -f namespace.yaml

kubectl apply -f postgres-secrets.yaml
kubectl get secrets -n backstage

kubectl apply -f postgres-storage.yaml
kubectl get pv -n backstage
kubectl get pvc -n backstage

kubectl apply -f postgres.yaml
kubectl get pods --namespace=backstage
kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash
psql -U $POSTGRES_USER

kubectl apply -f postgres-service.yaml
kubectl get services --namespace=backstage

kubectl apply -f backstage-secrets.yaml
kubectl get secret -n backstage

kubectl apply -f backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

kubectl logs --namespace=backstage -f backstage-854df67b6c-fmvcz -c backstage

kubectl apply -f backstage-service.yaml
kubectl get services --namespace=backstage
sudo kubectl port-forward --namespace=backstage svc/backstage 80:80
sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007
sudo kubectl port-forward --namespace=backstage svc/backstage 80:80



- Aplicado:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos$ kubectl get all -n backstage
NAME                             READY   STATUS    RESTARTS   AGE
pod/backstage-5d7f9695d9-42n49   0/1     Pending   0          15s
pod/postgres-667978b84d-c4zxm    0/1     Pending   0          6m18s

NAME                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/backstage   NodePort    10.108.129.168   <none>        80:31030/TCP   5s
service/postgres    ClusterIP   10.98.37.123     <none>        5432/TCP       60s

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/backstage   0/1     1            0           15s
deployment.apps/postgres    0/1     1            0           6m18s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/backstage-5d7f9695d9   1         1         0       15s
replicaset.apps/postgres-667978b84d    1         1         0       6m18s
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos$ date
Sun 22 Oct 2023 11:58:05 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos$

~~~~





- Node do Kubeadm não tá subindo Pods:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos$ kubectl get pods -n backstage
NAME                        READY   STATUS    RESTARTS   AGE
postgres-667978b84d-c4zxm   0/1     Pending   0          4m26s
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos$

fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos$ kubectl describe pod postgres-667978b84d-c4zxm -n backstage
Name:           postgres-667978b84d-c4zxm
Namespace:      backstage

Events:
  Type     Reason            Age    From               Message
  ----     ------            ----   ----               -------
  Warning  FailedScheduling  2m17s  default-scheduler  0/1 nodes are available: 1 node(s) had untolerated taint {node.kubernetes.io/disk-pressure: }. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling..
fernando@debian10x64:~/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos$
~~~~





# ####################################################################################################################################################
# ####################################################################################################################################################
## pendente

- TSHOOT, erro "0/1 nodes are available: 1 node(s) had untolerated taint {node.kubernetes.io/disk-pressure: }. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling..", Pods parando em "Pending".
- Efetuar deploy do Backstage no Kubernetes Local.















# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 28/10/2023

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






- Efetuar deploy do Backstage no Kubernetes Local.




- Subindo Backstage via Kubernetes:

https://backstage.io/docs/deployment/k8s/#creating-a-namespace
<https://backstage.io/docs/deployment/k8s/#creating-a-namespace>

cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos
kubectl apply -f namespace.yaml

kubectl apply -f postgres-secrets.yaml
kubectl get secrets -n backstage

kubectl apply -f postgres-storage.yaml
kubectl get pv -n backstage
kubectl get pvc -n backstage

kubectl apply -f postgres.yaml
kubectl get pods --namespace=backstage
kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash
psql -U $POSTGRES_USER

kubectl apply -f postgres-service.yaml
kubectl get services --namespace=backstage

kubectl apply -f backstage-secrets.yaml
kubectl get secret -n backstage

kubectl apply -f backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

kubectl logs --namespace=backstage -f backstage-854df67b6c-fmvcz -c backstage

kubectl apply -f backstage-service.yaml
kubectl get services --namespace=backstage
sudo kubectl port-forward --namespace=backstage svc/backstage 80:80
sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007
sudo kubectl port-forward --namespace=backstage svc/backstage 80:80







~~~~bash

root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl get pods --namespace=backstage
NAME                        READY   STATUS    RESTARTS   AGE
postgres-667978b84d-6bjsl   0/1     Pending   0          64s
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#

root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl describe pod postgres-667978b84d-6bjsl --namespace=backstage
Name:           postgres-667978b84d-6bjsl
Namespace:      backstage
Priority:       0
Node:           <none>
Labels:         app=postgres
                pod-template-hash=667978b84d
Annotations:    <none>
Status:         Pending
IP:
IPs:            <none>
Controlled By:  ReplicaSet/postgres-667978b84d
Containers:
  postgres:
    Image:      postgres:13.2-alpine
    Port:       5432/TCP
    Host Port:  0/TCP
    Environment Variables from:
      postgres-secrets  Secret  Optional: false
    Environment:        <none>
    Mounts:
      /var/lib/postgresql/data from postgresdb (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-qcjch (ro)
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  postgresdb:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  postgres-storage-claim
    ReadOnly:   false
  kube-api-access-qcjch:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  92s   default-scheduler  0/1 nodes are available: pod has unbound immediate PersistentVolumeClaims. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling..
  Warning  FailedScheduling  90s   default-scheduler  0/1 nodes are available: 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling..
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#

~~~~








- Removendo taint

kubectl taint nodes debian10x64 node-role.kubernetes.io/control-plane-



- ANTES:

~~~~YAML
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl describe node debian10x64
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
Unschedulable:      false
Lease:
~~~~


- DEPOIS:

~~~~YAML
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl taint nodes debian10x64 node-role.kubernetes.io/control-plane-
node/debian10x64 untainted
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl describe node debian10x64 | head -n 55
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
Taints:             <none>
Unschedulable:      false
Lease:
~~~~




- Pod do Postgres ficou running agora:

~~~~bash

root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl get pods --namespace=backstage
NAME                        READY   STATUS    RESTARTS   AGE
postgres-667978b84d-6bjsl   1/1     Running   0          5m10s
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#

~~~~








- Aplicando demais manifestos

- Pods sendo criados "ContainerCreating":

~~~~bash
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl get all --namespace=backstage
NAME                             READY   STATUS              RESTARTS   AGE
pod/backstage-5d7f9695d9-qqngq   0/1     ContainerCreating   0          18s
pod/postgres-667978b84d-6bjsl    1/1     Running             0          6m49s

NAME                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/backstage   NodePort    10.96.136.253   <none>        80:32537/TCP   7s
service/postgres    ClusterIP   10.103.32.165   <none>        5432/TCP       6m40s

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/backstage   0/1     1            0           18s
deployment.apps/postgres    1/1     1            1           6m49s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/backstage-5d7f9695d9   1         1         0       18s
replicaset.apps/postgres-667978b84d    1         1         1       6m49s
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# date
Sat 28 Oct 2023 06:17:19 PM -03
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#

~~~~





- Pods foram criados.
- Pod com erro, falha ao tentar conectar com o database:

~~~~bash
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl get all --namespace=backstage
NAME                             READY   STATUS    RESTARTS   AGE
pod/backstage-5d7f9695d9-qqngq   1/1     Running   0          62s
pod/postgres-667978b84d-6bjsl    1/1     Running   0          7m33s

NAME                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/backstage   NodePort    10.96.136.253   <none>        80:32537/TCP   51s
service/postgres    ClusterIP   10.103.32.165   <none>        5432/TCP       7m24s

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/backstage   1/1     1            1           62s
deployment.apps/postgres    1/1     1            1           7m33s

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/backstage-5d7f9695d9   1         1         1       62s
replicaset.apps/postgres-667978b84d    1         1         1       7m33s
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl logs backstage-5d7f9695d9-qqngq --namespace=backstage
Loaded config from app-config.yaml
{"level":"info","message":"Found 2 new secrets in config that will be redacted","service":"backstage"}
{"level":"info","message":"Created UrlReader predicateMux{readers=azure{host=dev.azure.com,authed=false},bitbucketCloud{host=bitbucket.org,authed=false},github{host=github.com,authed=false},gitlab{host=gitlab.com,authed=false},awsS3{host=amazonaws.com,authed=false},fetch{}","service":"backstage"}
Backend failed to start up Error: Failed to connect to the database to make sure that 'backstage_plugin_catalog' exists, RangeError [ERR_SOCKET_BAD_PORT]: Port should be >= 0 and < 65536. Received type number (NaN).
    at /app/node_modules/@backstage/backend-common/dist/index.cjs.js:973:17
    at async CatalogBuilder.build (/app/node_modules/@backstage/plugin-catalog-backend/dist/cjs/CatalogBuilder-21da853f.cjs.js:4940:22)
    at async createPlugin$4 (/app/packages/backend/dist/index.cjs.js:82:40)
    at async main (/app/packages/backend/dist/index.cjs.js:229:29)
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# date
Sat 28 Oct 2023 06:18:18 PM -03
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#

Pod em crash agora

root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl get all --namespace=backstage
NAME                             READY   STATUS             RESTARTS       AGE
pod/backstage-5d7f9695d9-qqngq   0/1     CrashLoopBackOff   7 (3m1s ago)   4h42m
pod/postgres-667978b84d-6bjsl    1/1     Running            0              4h49m

~~~~










Conforme o tutorial, as variáveis são injetadas automaticamente:

https://backstage.io/docs/deployment/k8s/#creating-a-postgresql-deployment
<https://backstage.io/docs/deployment/k8s/#creating-a-postgresql-deployment>
There is no special wiring needed to access the PostgreSQL service. Since it's running on the same cluster, Kubernetes will inject POSTGRES_SERVICE_HOST and POSTGRES_SERVICE_PORT environment variables into our Backstage container. These can be used in the Backstage app-config.yaml along with the secrets:

backend:
  database:
    client: pg
    connection:
      host: ${POSTGRES_SERVICE_HOST}
      port: ${POSTGRES_SERVICE_PORT}
      user: ${POSTGRES_USER}
      password: ${POSTGRES_PASSWORD}









kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash

kubectl exec -it --namespace=backstage postgres-667978b84d-6bjsl -- /bin/bash

- Dentro do Pod do postgres:

~~~~bash

bash-5.1# env
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_SERVICE_PORT=443
POSTGRES_PORT_5432_TCP=tcp://10.103.32.165:5432
HOSTNAME=postgres-667978b84d-6bjsl
POSTGRES_PASSWORD=lab123456
POSTGRES_PORT_5432_TCP_ADDR=10.103.32.165
POSTGRES_PORT_5432_TCP_PROTO=tcp
PWD=/
PG_SHA256=5fd7fcd08db86f5b2aed28fcfaf9ae0aca8e9428561ac547764c2a2b0f41adfc
HOME=/root
LANG=en_US.utf8
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
POSTGRES_PORT_5432_TCP_PORT=5432
PG_MAJOR=13
PG_VERSION=13.2
TERM=xterm
POSTGRES_PORT=tcp://10.103.32.165:5432
SHLVL=1
POSTGRES_USER=backstage
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
PGDATA=/var/lib/postgresql/data
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PORT=443
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
POSTGRES_SERVICE_PORT=5432
POSTGRES_SERVICE_HOST=10.103.32.165
_=/usr/bin/env
bash-5.1#

~~~~











- Comparando

no app-config.yaml

~~~~yaml

backend:
[...]
  database:
    client: pg
    connection:
      host: ${POSTGRES_HOST}
      port: ${POSTGRES_PORT}
      user: ${POSTGRES_USER}
      password: ${POSTGRES_PASSWORD}
~~~~

- Nas variáveis de ambiente:

~~~~bash

bash-5.1# env | grep -i postgres
POSTGRES_PORT_5432_TCP=tcp://10.103.32.165:5432
HOSTNAME=postgres-667978b84d-6bjsl
POSTGRES_PASSWORD=lab123456
POSTGRES_PORT_5432_TCP_ADDR=10.103.32.165
POSTGRES_PORT_5432_TCP_PROTO=tcp
POSTGRES_PORT_5432_TCP_PORT=5432
POSTGRES_PORT=tcp://10.103.32.165:5432
POSTGRES_USER=backstage
PGDATA=/var/lib/postgresql/data
POSTGRES_SERVICE_PORT=5432
POSTGRES_SERVICE_HOST=10.103.32.165
bash-5.1#
bash-5.1#

~~~~



- As variáveis referentes a host e port estão diferentes.
- Dentro do pod tem o "_SERVICE_" no nome de cada uma.

- Necessário ajustar no app-config.yaml
ajustando
/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage/app-config.yaml

- Buildando e efetuando Push ao Docker Hub
cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/docker-multi-stage
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-local:v2 .
docker push fernandomj90/backstage-local:v2







- Ajustando Deployment do Backstage, para usar Docker image v2:

cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos
kubectl apply -f backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage









~~~~bash

root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl get pods --namespace=backstage
NAME                         READY   STATUS                   RESTARTS       AGE
backstage-5d7f9695d9-26ljz   0/1     Evicted                  0              30m
backstage-5d7f9695d9-2ww2d   0/1     Evicted                  0              30m
backstage-5d7f9695d9-2xdj8   0/1     Evicted                  0              30m
backstage-5d7f9695d9-44s7p   0/1     Evicted                  0              30m
backstage-5d7f9695d9-45vlb   0/1     Evicted                  0              31m
backstage-5d7f9695d9-4gh4v   0/1     Evicted                  0              30m
backstage-5d7f9695d9-4jj5v   0/1     Evicted                  0              30m
backstage-5d7f9695d9-569z8   0/1     Evicted                  0              31m
backstage-5d7f9695d9-5f667   0/1     Evicted                  0              31m
backstage-5d7f9695d9-5lt78   0/1     Evicted                  0              30m
backstage-5d7f9695d9-5scbm   0/1     Evicted                  0              30m
backstage-5d7f9695d9-5tw6g   0/1     Evicted                  0              30m
backstage-5d7f9695d9-5wrj5   0/1     Evicted                  0              30m
backstage-5d7f9695d9-6428m   0/1     Evicted                  0              30m
backstage-5d7f9695d9-69pzm   0/1     Evicted                  0              30m
backstage-5d7f9695d9-6wkhc   0/1     Evicted                  0              31m
backstage-5d7f9695d9-76427   0/1     Evicted                  0              30m
backstage-5d7f9695d9-7cpgj   0/1     Evicted                  0              31m
backstage-5d7f9695d9-7h2cj   0/1     Evicted                  0              30m
backstage-5d7f9695d9-7pw7p   0/1     Evicted                  0              30m
backstage-5d7f9695d9-8c8tm   0/1     Evicted                  0              30m
backstage-5d7f9695d9-8lc6q   0/1     Evicted                  0              31m
backstage-5d7f9695d9-8p8bx   0/1     Evicted                  0              31m
backstage-5d7f9695d9-8tcxq   0/1     Evicted                  0              30m
backstage-5d7f9695d9-9g572   0/1     Evicted                  0              30m
backstage-5d7f9695d9-9wtls   0/1     Evicted                  0              31m
backstage-5d7f9695d9-b5wt2   0/1     Evicted                  0              31m
backstage-5d7f9695d9-bgmdr   0/1     Evicted                  0              31m
backstage-5d7f9695d9-blc4m   0/1     Evicted                  0              30m
backstage-5d7f9695d9-ckrpx   0/1     Evicted                  0              31m
backstage-5d7f9695d9-cmm62   0/1     Evicted                  0              31m
backstage-5d7f9695d9-cnhc4   0/1     Evicted                  0              30m
backstage-5d7f9695d9-cns4p   0/1     Evicted                  0              30m
backstage-5d7f9695d9-dcdsp   0/1     Evicted                  0              30m
backstage-5d7f9695d9-f2qx6   0/1     Evicted                  0              30m
backstage-5d7f9695d9-f8v5g   0/1     Evicted                  0              30m
backstage-5d7f9695d9-fcfn2   0/1     Error                    0              30m
backstage-5d7f9695d9-fd6rs   0/1     Evicted                  0              31m
backstage-5d7f9695d9-fms75   0/1     Evicted                  0              31m
backstage-5d7f9695d9-fmwpt   0/1     Evicted                  0              31m
backstage-5d7f9695d9-fx5x6   0/1     Evicted                  0              31m
backstage-5d7f9695d9-fxgt8   0/1     Evicted                  0              30m
backstage-5d7f9695d9-hdb77   0/1     Evicted                  0              31m
backstage-5d7f9695d9-hg2mv   0/1     Evicted                  0              30m
backstage-5d7f9695d9-hrbq4   0/1     Evicted                  0              30m
backstage-5d7f9695d9-jbhn9   0/1     Evicted                  0              31m
backstage-5d7f9695d9-jt6xb   0/1     Evicted                  0              31m
backstage-5d7f9695d9-kc5lf   0/1     Evicted                  0              31m
backstage-5d7f9695d9-kfkqh   0/1     Evicted                  0              31m
backstage-5d7f9695d9-ktrkv   0/1     Evicted                  0              30m
backstage-5d7f9695d9-lf86d   0/1     Evicted                  0              31m
backstage-5d7f9695d9-lk44c   0/1     Evicted                  0              30m
backstage-5d7f9695d9-lxmlg   0/1     Evicted                  0              31m
backstage-5d7f9695d9-m5b8s   0/1     Evicted                  0              31m
backstage-5d7f9695d9-mcvrg   0/1     Evicted                  0              30m
backstage-5d7f9695d9-ms4n9   0/1     Evicted                  0              31m
backstage-5d7f9695d9-nz6vf   0/1     Evicted                  0              31m
backstage-5d7f9695d9-p2sq5   0/1     Evicted                  0              30m
backstage-5d7f9695d9-p6m6z   0/1     Evicted                  0              31m
backstage-5d7f9695d9-ptlpn   0/1     Pending                  0              3m16s
backstage-5d7f9695d9-ptvsn   0/1     Evicted                  0              30m
backstage-5d7f9695d9-q4cwr   0/1     Evicted                  0              31m
backstage-5d7f9695d9-q8hjb   0/1     ContainerStatusUnknown   1              19m
backstage-5d7f9695d9-qcfz4   0/1     Evicted                  0              31m
backstage-5d7f9695d9-qhwrl   0/1     Evicted                  0              31m
backstage-5d7f9695d9-qqngq   0/1     ContainerStatusUnknown   11 (33m ago)   5h34m
backstage-5d7f9695d9-qw924   0/1     Evicted                  0              30m
backstage-5d7f9695d9-rh7jw   0/1     Evicted                  0              30m
backstage-5d7f9695d9-rhd4l   0/1     Evicted                  0              31m
backstage-5d7f9695d9-rllx5   0/1     Evicted                  0              30m
backstage-5d7f9695d9-s7495   0/1     Evicted                  0              30m
backstage-5d7f9695d9-s94gn   0/1     Evicted                  0              30m
backstage-5d7f9695d9-sb854   0/1     Evicted                  0              31m
backstage-5d7f9695d9-svqqb   0/1     Evicted                  0              30m
backstage-5d7f9695d9-tgmkk   0/1     Evicted                  0              31m
backstage-5d7f9695d9-v5bwl   0/1     Evicted                  0              30m
backstage-5d7f9695d9-v85cg   0/1     Evicted                  0              31m
backstage-5d7f9695d9-v8cn2   0/1     Evicted                  0              30m
backstage-5d7f9695d9-vfkpp   0/1     Evicted                  0              30m
backstage-5d7f9695d9-vwd8x   0/1     Evicted                  0              30m
backstage-5d7f9695d9-wcwk9   0/1     Evicted                  0              31m
backstage-5d7f9695d9-wlj8h   0/1     Evicted                  0              31m
backstage-5d7f9695d9-wqx49   0/1     Evicted                  0              30m
backstage-5d7f9695d9-wzlgz   0/1     Evicted                  0              31m
backstage-5d7f9695d9-xb8wb   0/1     Error                    0              11m
backstage-5d7f9695d9-xcwqn   0/1     Evicted                  0              30m
backstage-5d7f9695d9-xnpp7   0/1     Evicted                  0              30m
backstage-5d7f9695d9-xq6pb   0/1     Evicted                  0              31m
backstage-5d7f9695d9-xqr7t   0/1     Evicted                  0              31m
backstage-5d7f9695d9-xtx2k   0/1     Evicted                  0              31m
backstage-5d7f9695d9-xvgbj   0/1     Evicted                  0              31m
backstage-5d7f9695d9-xxwxn   0/1     Evicted                  0              30m
backstage-5d7f9695d9-zlmx2   0/1     Evicted                  0              31m
backstage-7b9f466556-ctcxf   0/1     Pending                  0              29s
postgres-667978b84d-262fm    0/1     Evicted                  0              3m55s
postgres-667978b84d-2tqpt    0/1     Evicted                  0              3m55s
postgres-667978b84d-2w2qb    0/1     Evicted                  0              3m54s
postgres-667978b84d-4n9b4    0/1     Evicted                  0              3m56s
postgres-667978b84d-4xhhp    0/1     Evicted                  0              3m56s
postgres-667978b84d-5226n    0/1     Evicted                  0              3m58s
postgres-667978b84d-57s75    0/1     Evicted                  0              3m56s
postgres-667978b84d-5nbgw    0/1     Evicted                  0              3m59s
postgres-667978b84d-6bjsl    0/1     ContainerStatusUnknown   1              5h41m
postgres-667978b84d-6jtxb    0/1     Evicted                  0              3m55s
postgres-667978b84d-7m2qz    0/1     Evicted                  0              3m59s
postgres-667978b84d-b24k4    0/1     Evicted                  0              3m55s
postgres-667978b84d-c2bcr    0/1     Evicted                  0              3m56s
postgres-667978b84d-cwgcx    0/1     Evicted                  0              3m58s
postgres-667978b84d-dng67    0/1     Evicted                  0              3m54s
postgres-667978b84d-dpn67    0/1     Pending                  0              3m53s
postgres-667978b84d-dpxss    0/1     Evicted                  0              3m57s
postgres-667978b84d-f2tsp    0/1     Evicted                  0              3m57s
postgres-667978b84d-g24qr    0/1     Evicted                  0              3m59s
postgres-667978b84d-gs4dd    0/1     Evicted                  0              3m57s
postgres-667978b84d-gwch2    0/1     Evicted                  0              3m59s
postgres-667978b84d-hgdkn    0/1     Evicted                  0              3m55s
postgres-667978b84d-hqwdg    0/1     Evicted                  0              3m57s
postgres-667978b84d-jbw2p    0/1     Evicted                  0              3m55s
postgres-667978b84d-jjnfw    0/1     Evicted                  0              3m57s
postgres-667978b84d-kfpnm    0/1     Evicted                  0              3m59s
postgres-667978b84d-m9sf5    0/1     Evicted                  0              3m56s
postgres-667978b84d-mfc4g    0/1     Evicted                  0              3m55s
postgres-667978b84d-ncz92    0/1     Evicted                  0              3m56s
postgres-667978b84d-p8bff    0/1     Completed                0              19m
postgres-667978b84d-psh5m    0/1     Evicted                  0              3m58s
postgres-667978b84d-pwbt7    0/1     Evicted                  0              3m55s
postgres-667978b84d-pzpnt    0/1     Evicted                  0              3m58s
postgres-667978b84d-qjtkp    0/1     Evicted                  0              3m59s
postgres-667978b84d-qqp8m    0/1     Evicted                  0              3m58s
postgres-667978b84d-qtt9s    0/1     Completed                0              11m
postgres-667978b84d-rhhk6    0/1     Evicted                  0              3m59s
postgres-667978b84d-s2wqx    0/1     Evicted                  0              3m55s
postgres-667978b84d-s44ns    0/1     Evicted                  0              3m58s
postgres-667978b84d-s89nb    0/1     Completed                0              25m
postgres-667978b84d-sk5ks    0/1     Evicted                  0              3m57s
postgres-667978b84d-tqw4c    0/1     Evicted                  0              3m56s
postgres-667978b84d-v49sm    0/1     Evicted                  0              3m59s
postgres-667978b84d-xdtww    0/1     Evicted                  0              3m59s
postgres-667978b84d-z54h2    0/1     Evicted                  0              3m57s
postgres-667978b84d-z9d4g    0/1     Evicted                  0              3m59s
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#

~~~~











root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl describe node debian10x64
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
Taints:             <none>

Events:
  Type     Reason                 Age                     From     Message
  ----     ------                 ----                    ----     -------
  Warning  FreeDiskSpaceFailed    45m                     kubelet  Failed to garbage collect required amount of images. Attempted to free 3358330060 bytes, but only found 301773 bytes eligible to free.
  Warning  FreeDiskSpaceFailed    40m                     kubelet  Failed to garbage collect required amount of images. Attempted to free 3272002764 bytes, but only found 0 bytes eligible to free.
  Warning  ImageGCFailed          40m                     kubelet  Failed to garbage collect required amount of images. Attempted to free 3272002764 bytes, but only found 0 bytes eligible to free.
  Warning  FreeDiskSpaceFailed    35m                     kubelet  Failed to garbage collect required amount of images. Attempted to free 3742960844 bytes, but only found 0 bytes eligible to free.
  Warning  ImageGCFailed          35m                     kubelet  Failed to garbage collect required amount of images. Attempted to free 3742960844 bytes, but only found 0 bytes eligible to free.
  Normal   NodeHasDiskPressure    33m (x2 over 44m)       kubelet  Node debian10x64 status is now: NodeHasDiskPressure
  Warning  EvictionThresholdMet   33m (x7 over 44m)       kubelet  Attempting to reclaim ephemeral-storage
  Warning  FreeDiskSpaceFailed    30m                     kubelet  Failed to garbage collect required amount of images. Attempted to free 3743042764 bytes, but only found 0 bytes eligible to free.
  Warning  FreeDiskSpaceFailed    25m                     kubelet  Failed to garbage collect required amount of images. Attempted to free 3743251660 bytes, but only found 0 bytes eligible to free.
  Warning  FreeDiskSpaceFailed    20m                     kubelet  Failed to garbage collect required amount of images. Attempted to free 3743345868 bytes, but only found 0 bytes eligible to free.
  Warning  FreeDiskSpaceFailed    15m                     kubelet  Failed to garbage collect required amount of images. Attempted to free 5462772940 bytes, but only found 0 bytes eligible to free.
  Normal   NodeHasNoDiskPressure  9m42s (x32 over 6h18m)  kubelet  Node debian10x64 status is now: NodeHasNoDiskPressure
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#







cd /home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos

kubectl delete -f postgres.yaml
kubectl delete -f backstage.yaml

kubectl apply -f postgres.yaml
kubectl apply -f backstage.yaml

kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage







root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl get pods -A
NAMESPACE     NAME                                  READY   STATUS    RESTARTS         AGE
backstage     backstage-7b9f466556-jtwcs            1/1     Running   0                12s
backstage     postgres-667978b84d-gnfwp             1/1     Running   0                26s
kube-system   cilium-operator-788c4f69bc-g7mw4      1/1     Running   2 (6h20m ago)    8h
kube-system   cilium-q8xc5                          1/1     Running   2 (6h20m ago)    8h
kube-system   coredns-5dd5756b68-btrs6              1/1     Running   3 (6h20m ago)    8h
kube-system   coredns-5dd5756b68-gpwnt              1/1     Running   3 (6h19m ago)    8h
kube-system   etcd-debian10x64                      1/1     Running   62 (6h20m ago)   8h
kube-system   kube-apiserver-debian10x64            1/1     Running   2 (6h20m ago)    8h
kube-system   kube-controller-manager-debian10x64   1/1     Running   2 (6h20m ago)    8h
kube-system   kube-proxy-272d4                      1/1     Running   2 (6h20m ago)    8h
kube-system   kube-scheduler-debian10x64            1/1     Running   2 (6h20m ago)    8h
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# date
Sun 29 Oct 2023 12:06:23 AM -03
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#






root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl get pods -n backstage
NAME                         READY   STATUS    RESTARTS   AGE
backstage-7b9f466556-jtwcs   1/1     Running   0          62s
postgres-667978b84d-gnfwp    1/1     Running   0          76s
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# kubectl logs backstage-7b9f466556-jtwcs -n backstage
Loaded config from app-config.yaml
{"level":"info","message":"Found 2 new secrets in config that will be redacted","service":"backstage"}
{"level":"info","message":"Created UrlReader predicateMux{readers=azure{host=dev.azure.com,authed=false},bitbucketCloud{host=bitbucket.org,authed=false},github{host=github.com,authed=false},gitlab{host=gitlab.com,authed=false},awsS3{host=amazonaws.com,authed=false},fetch{}","service":"backstage"}
{"level":"info","message":"Performing database migration","plugin":"catalog","service":"backstage","type":"plugin"}
{"level":"info","message":"Configuring \"database\" as KeyStore provider","plugin":"auth","service":"backstage","type":"plugin"}
{"level":"info","message":"Creating Local publisher for TechDocs","plugin":"techdocs","service":"backstage","type":"plugin"}
{"level":"info","message":"Added DefaultCatalogCollatorFactory collator factory for type software-catalog","plugin":"search","service":"backstage","type":"plugin"}
{"level":"info","message":"Added DefaultTechDocsCollatorFactory collator factory for type techdocs","plugin":"search","service":"backstage","type":"plugin"}
{"level":"info","message":"Starting all scheduled search tasks.","plugin":"search","service":"backstage","type":"plugin"}
{"level":"info","message":"Serving static app content from /app/packages/app/dist","plugin":"app","service":"backstage","type":"plugin"}
{"level":"info","message":"Task worker starting: search_index_software_catalog, {\"version\":2,\"cadence\":\"PT10M\",\"initialDelayDuration\":\"PT3S\",\"timeoutAfterDuration\":\"PT15M\"}","service":"backstage","task":"search_index_software_catalog","type":"taskManager"}
{"level":"info","message":"Task worker starting: search_index_techdocs, {\"version\":2,\"cadence\":\"PT10M\",\"initialDelayDuration\":\"PT3S\",\"timeoutAfterDuration\":\"PT15M\"}","service":"backstage","task":"search_index_techdocs","type":"taskManager"}
{"level":"info","message":"Injecting env config into module-backstage.65602c50.js","plugin":"app","service":"backstage","type":"plugin"}
{"entity":"location:default/generated-0ca6551527608b8e42ccccd463f27d4113d35ff1","level":"warn","location":"file:/examples/org.yaml","message":"file /examples/org.yaml does not exist","plugin":"catalog","service":"backstage","type":"plugin"}
{"entity":"location:default/generated-eeed3503740b7c4b80f2aad3e417fafee7a3803d","level":"warn","location":"file:/examples/entities.yaml","message":"file /examples/entities.yaml does not exist","plugin":"catalog","service":"backstage","type":"plugin"}
{"entity":"location:default/generated-c4d4a3f82d0b7ecef1bd7d6a1991be94fded46aa","level":"warn","location":"file:/examples/template/template.yaml","message":"file /examples/template/template.yaml does not exist","plugin":"catalog","service":"backstage","type":"plugin"}
{"level":"info","message":"Storing 283 updated assets and 0 new assets","plugin":"app","service":"backstage","type":"plugin"}
{"level":"info","message":"Listening on 0.0.0.0:7007","service":"backstage"}
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#

root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos# date
Sun 29 Oct 2023 12:07:24 AM -03
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/deploy-ambiente/deploy-local/manifestos#
