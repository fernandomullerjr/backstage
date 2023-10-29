
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
~~~~
