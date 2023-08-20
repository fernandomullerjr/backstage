



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

cd /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s
kubectl apply -f namespace.yaml

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-secrets.yaml
kubectl get secrets -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml
kubectl get pv -n backstage
kubectl get pvc -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml
kubectl get pods --namespace=backstage
kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash
psql -U $POSTGRES_USER

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml
kubectl get services --namespace=backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml
kubectl get secret -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

kubectl logs --namespace=backstage -f backstage-854df67b6c-fmvcz -c backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-service.yaml
kubectl get services --namespace=backstage
sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007

- Anexando policy AWS Managed abaixo na role "arn:aws:iam::552925778543:role/eks-lab-aws-load-balancer-controller-sa-irsa":
ElasticLoadBalancingFullAccess

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
kubectl get ingress --namespace=backstage

- Buildar Docker Image com URL atualizada do ALB.

docker push fernandomj90/backstage-mandragora:latest


## DESTROY

cd /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s

kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-service.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-secrets.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/namespace.yaml

kubectl get all -n backstage
kubectl get ingress -n backstage

















# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 24/06/2023

- Iniciando repositório

https://github.com/fernandomullerjr/backstage
<https://github.com/fernandomullerjr/backstage>

eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
echo "# backstage" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:fernandomullerjr/backstage.git
git push -u origin main





## Deploy do Backstage via Kubernetes

https://backstage.io/docs/deployment/k8s/
<https://backstage.io/docs/deployment/k8s/>


### Creating a namespace

Deployments in Kubernetes are commonly assigned to their own namespace to isolate services in a multi-tenant environment.

This can be done through kubectl directly:

~~~~bash
$ kubectl create namespace backstage
namespace/backstage created
~~~~

Alternatively, create and apply a Namespace definition:

~~~~yaml
# kubernetes/namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: backstage
~~~~


~~~~bash
$ kubectl apply -f kubernetes/namespace.yaml
namespace/backstage created
~~~~


- Editado
/home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/namespace.yaml

- Aplicando
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/namespace.yaml

~~~~bash

fernando@debian10x64:~$ kubectl get ns
NAME                    STATUS   AGE
backstage               Active   4s
default                 Active   39m
kube-node-lease         Active   39m
kube-prometheus-stack   Active   30m
kube-public             Active   39m
kube-system             Active   39m
fernando@debian10x64:~$

~~~~






## Creating the PostgreSQL database

Backstage in production uses PostgreSQL as a database. To isolate the database from Backstage app deployments, we can create a separate Kubernetes deployment for PostgreSQL.
Creating a PostgreSQL secret

First, create a Kubernetes Secret for the PostgreSQL username and password. This will be used by both the PostgreSQL database and Backstage deployments:

~~~~yaml
# kubernetes/postgres-secrets.yaml
apiVersion: v1
kind: Secret
metadata:
  name: postgres-secrets
  namespace: backstage
type: Opaque
data:
  POSTGRES_USER: YmFja3N0YWdl
  POSTGRES_PASSWORD: aHVudGVyMg==
~~~~

The data in Kubernetes secrets are base64-encoded. The values can be generated on the command line:

~~~~bash
$ echo -n "backstage" | base64
YmFja3N0YWdl
~~~~

    Note: Secrets are base64-encoded, but not encrypted. Be sure to enable Encryption at Rest for the cluster. For storing secrets in Git, consider SealedSecrets or other solutions.

The secrets can now be applied to the Kubernetes cluster:

~~~~bash
$ kubectl apply -f kubernetes/postgres-secrets.yaml
secret/postgres-secrets created
~~~~


- Criando senha personalizada

~~~~bash
fernando@debian10x64:~$ echo -n "lab123456" | base64
bGFiMTIzNDU2
fernando@debian10x64:~$

~~~~


- Editado.
- Aplicando:
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-secrets.yaml

~~~~bash

fernando@debian10x64:~$ kubectl get secrets -n backstage
NAME                  TYPE                                  DATA   AGE
default-token-phxh8   kubernetes.io/service-account-token   3      14m
postgres-secrets      Opaque                                2      25s
fernando@debian10x64:~$

~~~~






## Creating a PostgreSQL persistent volume

PostgreSQL needs a persistent volume to store data; we'll create one along with a PersistentVolumeClaim. In this case, we're claiming the whole volume - but claims can ask for only part of a volume as well.

~~~~yaml
# kubernetes/postgres-storage.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: postgres-storage
  namespace: backstage
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 2G
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: '/mnt/data'
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-storage-claim
  namespace: backstage
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2G
~~~~

This file contains definitions for two different kinds, separated by a line with a triple dash. This syntax is helpful if you want to consolidate related Kubernetes definitions in a single file and apply them at the same time.

Note the volume type: local; this creates a volume using local disk on Kubernetes nodes. More likely in a production scenario, you'd want to use a more highly available type of PersistentVolume.

Apply the storage volume and claim to the Kubernetes cluster:

~~~~bash
$ kubectl apply -f kubernetes/postgres-storage.yaml
persistentvolume/postgres-storage created
persistentvolumeclaim/postgres-storage-claim created
~~~~



- Editado.
- Aplicando
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml

~~~~bash

fernando@debian10x64:~$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml
persistentvolume/postgres-storage created
persistentvolumeclaim/postgres-storage-claim created
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get pv -n backstage
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                                                                                                STORAGECLASS   REASON   AGE
postgres-storage                           2G         RWO            Retain           Bound    backstage/postgres-storage-claim                                                                                     manual                  19s
pvc-2c87a09e-1f3d-44e9-995b-dcf417b59a01   20Gi       RWO            Delete           Bound    kube-prometheus-stack/prometheus-kube-prometheus-stack-prometheus-db-prometheus-kube-prometheus-stack-prometheus-0   gp2                     44m
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get pvc -n backstage
NAME                     STATUS   VOLUME             CAPACITY   ACCESS MODES   STORAGECLASS   AGE
postgres-storage-claim   Bound    postgres-storage   2G         RWO            manual         26s
fernando@debian10x64:~$

~~~~









## Creating a PostgreSQL deployment

Now we can create a Kubernetes Deployment descriptor for the PostgreSQL database deployment itself:

~~~~yaml
# kubernetes/postgres.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
  namespace: backstage
spec:
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:13.2-alpine
          imagePullPolicy: 'IfNotPresent'
          ports:
            - containerPort: 5432
          envFrom:
            - secretRef:
                name: postgres-secrets
          volumeMounts:
            - mountPath: /var/lib/postgresql/data
              name: postgresdb
      volumes:
        - name: postgresdb
          persistentVolumeClaim:
            claimName: postgres-storage-claim
~~~~

If you're not used to Kubernetes, this is a lot to take in. We're describing a Deployment (one or more instances of an application) that we'd like Kubernetes to know about in the metadata block.

The spec block describes the desired state. Here we've requested Kubernetes create 1 replica (running instance of PostgreSQL), and to create the replica with the given pod template, which again contains Kubernetes metadata and a desired state. The template spec shows one container, created from the published postgres:13.2-alpine Docker image.

Note the envFrom and secretRef - this tells Kubernetes to fill environment variables in the container with values from the Secret we created. We've also referenced the volume created for the deployment, and given it the mount path expected by PostgreSQL.

Apply the PostgreSQL deployment to the Kubernetes cluster:

~~~~bash
$ kubectl apply -f kubernetes/postgres.yaml
deployment.apps/postgres created

$ kubectl get pods --namespace=backstage
NAME                        READY   STATUS    RESTARTS   AGE
postgres-56c86b8bbc-66pt2   1/1     Running   0          21s
~~~~

Verify the deployment by connecting to the pod:

~~~~bash
$ kubectl exec -it --namespace=backstage postgres-56c86b8bbc-66pt2 -- /bin/bash
bash-5.1# psql -U $POSTGRES_USER
psql (13.2)
backstage=# \q
bash-5.1# exit
~~~~



- Editado.
- Aplicando
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml
deployment.apps/postgres created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pods --namespace=backstage
NAME                        READY   STATUS              RESTARTS   AGE
postgres-77f59b67df-wxggr   0/1     ContainerCreating   0          5s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pods --namespace=backstage
NAME                        READY   STATUS    RESTARTS   AGE
postgres-77f59b67df-wxggr   1/1     Running   0          29s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$


kubectl exec -it --namespace=backstage postgres-77f59b67df-7wffm -- /bin/bash

psql -U $POSTGRES_USER

fernando@debian10x64:~$ kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash
bash-5.1#
bash-5.1#
bash-5.1# psql -U $POSTGRES_USER
psql (13.2)
Type "help" for help.

backstage=#


~~~~








## Creating a PostgreSQL service

The database pod is running, but how does another pod connect to it?

Kubernetes pods are transient - they can be stopped, restarted, or created dynamically. Therefore we don't want to try to connect to pods directly, but rather create a Kubernetes Service. Services keep track of pods and direct traffic to the right place.

The final step for our database is to create the service descriptor:

~~~~yaml
# kubernetes/postgres-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres
  namespace: backstage
spec:
  selector:
    app: postgres
  ports:
    - port: 5432
~~~~


Apply the service to the Kubernetes cluster:

~~~~bash
$ kubectl apply -f kubernetes/postgres-service.yaml
service/postgres created

$ kubectl get services --namespace=backstage
NAME       TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)    AGE
postgres   ClusterIP   10.96.5.103   <none>        5432/TCP   29s

~~~~


- Editado.
- Aplicando:
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml

~~~~bash

fernando@debian10x64:~$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml
service/postgres created
fernando@debian10x64:~$ kubectl^C
fernando@debian10x64:~$ kubectl get services --namespace=backstage
NAME       TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)    AGE
postgres   ClusterIP   172.20.66.5   <none>        5432/TCP   12s
fernando@debian10x64:~$

~~~~








Creating the Backstage instance

Now that we have PostgreSQL up and ready to store data, we can create the Backstage instance. This follows similar steps as the PostgreSQL deployment.
Creating a Backstage secret

For any Backstage configuration secrets, such as authorization tokens, we can create a similar Kubernetes Secret as we did for PostgreSQL, remembering to base64 encode the values:

~~~~yaml
# kubernetes/backstage-secrets.yaml
apiVersion: v1
kind: Secret
metadata:
  name: backstage-secrets
  namespace: backstage
type: Opaque
data:
  GITHUB_TOKEN: VG9rZW5Ub2tlblRva2VuVG9rZW5NYWxrb3ZpY2hUb2tlbg==
~~~~

Apply the secret to the Kubernetes cluster:

~~~~bash
$ kubectl apply -f kubernetes/backstage-secrets.yaml
secret/backstage-secrets created
~~~~

- Criando token no Github
backstage-lab
- Efetuado base64

- Aplicando
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml

~~~~bash

fernando@debian10x64:~$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml
secret/backstage-secrets created
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get secret -n backstage
NAME                  TYPE                                  DATA   AGE
backstage-secrets     Opaque                                1      10s
default-token-phxh8   kubernetes.io/service-account-token   3      44m
postgres-secrets      Opaque                                2      30m
fernando@debian10x64:~$

~~~~









## Creating a Backstage deployment

To create the Backstage deployment, first create a Docker image. We'll use this image to create a Kubernetes deployment. For this example, we'll use the standard host build with the frontend bundled and served from the backend.

First, create a Kubernetes Deployment descriptor:

~~~~yaml
# kubernetes/backstage.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backstage
  namespace: backstage
spec:
  replicas: 1
  selector:
    matchLabels:
      app: backstage
  template:
    metadata:
      labels:
        app: backstage
    spec:
      containers:
        - name: backstage
          image: backstage:1.0.0
          imagePullPolicy: IfNotPresent
          ports:
            - name: http
              containerPort: 7007
          envFrom:
            - secretRef:
                name: postgres-secrets
            - secretRef:
                name: backstage-secrets
# Uncomment if health checks are enabled in your app:
# https://backstage.io/docs/plugins/observability#health-checks
#          readinessProbe:
#            httpGet:
#              port: 7007
#              path: /healthcheck
#          livenessProbe:
#            httpGet:
#              port: 7007
#              path: /healthcheck
~~~~

For production deployments, the image reference will usually be a full URL to a repository on a container registry (for example, ECR on AWS).

For testing locally with minikube, you can point the local Docker daemon to the minikube internal Docker registry and then rebuild the image to install it:

~~~~bash
$ eval $(minikube docker-env)
$ yarn build-image --tag backstage:1.0.0
~~~~

There is no special wiring needed to access the PostgreSQL service. Since it's running on the same cluster, Kubernetes will inject POSTGRES_SERVICE_HOST and POSTGRES_SERVICE_PORT environment variables into our Backstage container. These can be used in the Backstage app-config.yaml along with the secrets:

~~~~yaml
backend:
  database:
    client: pg
    connection:
      host: ${POSTGRES_SERVICE_HOST}
      port: ${POSTGRES_SERVICE_PORT}
      user: ${POSTGRES_USER}
      password: ${POSTGRES_PASSWORD}
~~~~

Make sure to rebuild the Docker image after applying app-config.yaml changes.

Apply this Deployment to the Kubernetes cluster:

~~~~bash
$ kubectl apply -f kubernetes/backstage.yaml
deployment.apps/backstage created

$ kubectl get deployments --namespace=backstage
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
backstage   1/1     1            1           1m
postgres    1/1     1            1           10m

$ kubectl get pods --namespace=backstage
NAME                                 READY   STATUS    RESTARTS   AGE
backstage-54bfcd6476-n2jkm           1/1     Running   0          58s
postgres-56c86b8bbc-66pt2            1/1     Running   0          9m
~~~~

Beautiful! 🎉 The deployment and pod are running in the cluster. If you run into any trouble, check the container logs from the pod:

~~~~bash
# -f to tail, <pod> -c <container>
$ kubectl logs --namespace=backstage -f backstage-54bfcd6476-n2jkm -c backstage
~~~~






## Git

git status
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git add -u
git reset -- manifestos-k8s/backstage-secrets.yaml
git commit -m "Backstage lab."
git push
git status





- Editado deployment do Backstage.
- Aplicando:
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

- Erro no Pod do Backstage:

~~~~bash

fernando@debian10x64:~$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
deployment.apps/backstage created
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get deployments --namespace=backstage
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
backstage   0/1     1            0           6s
postgres    1/1     1            1           51m
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get pods --namespace=backstage
NAME                         READY   STATUS         RESTARTS   AGE
backstage-764dcdfc4b-vnftj   0/1     ErrImagePull   0          12s
postgres-77f59b67df-7wffm    1/1     Running        0          51m
fernando@debian10x64:~$

~~~~





## Docker image

https://backstage.io/docs/deployment/docker/

- Criado o Dockerfile:
/home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/dockerfile-multi

- Buildando:
cd /home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/
docker image build -t backstage . --file dockerfile-multi


- ERRO:

~~~~BASH
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage$ docker image build -t backstage . --file dockerfile-multi
Sending build context to Docker daemon  6.144kB
Step 1/27 : FROM node:16-bullseye-slim AS packages
16-bullseye-slim: Pulling from library/node
759700526b78: Pull complete
243faaa2ae94: Pull complete
8f814c8c0197: Pull complete
6c659989516e: Pull complete
59b07c4c9d28: Pull complete
Digest: sha256:84cff75662479f26f7baedf15832ea42003b1762da8e3ed7475fad2730c6042d
Status: Downloaded newer image for node:16-bullseye-slim
 ---> ac00507796e5
Step 2/27 : WORKDIR /app
 ---> Running in fd4df9fcf0d1
Removing intermediate container fd4df9fcf0d1
 ---> c5fe2fbc10ee
Step 3/27 : COPY package.json yarn.lock ./
COPY failed: file not found in build context or excluded by .dockerignore: stat package.json: file does not exist
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage$

~~~~







- Tratar erro no Deployment do Backstage, 
- Erros

~~~~bash
Events:
  Type     Reason     Age                 From               Message
  ----     ------     ----                ----               -------
  Normal   Scheduled  15m                 default-scheduler  Successfully assigned backstage/backstage-764dcdfc4b-vnftj to ip-10-0-1-24.ec2.internal
  Normal   Pulling    14m (x4 over 15m)   kubelet            Pulling image "backstage:1.0.0"
  Warning  Failed     14m (x4 over 15m)   kubelet            Failed to pull image "backstage:1.0.0": rpc error: code = Unknown desc = Error response from daemon: pull access denied for backstage, repository does not exist or may require 'docker login': denied: requested access to the resource is denied
  Warning  Failed     14m (x4 over 15m)   kubelet            Error: ErrImagePull
  Warning  Failed     13m (x6 over 15m)   kubelet            Error: ImagePullBackOff
  Normal   BackOff    24s (x67 over 15m)  kubelet            Back-off pulling image "backstage:1.0.0"
fernando@debian10x64:~$

~~~~



- Ajustando imagem no Deployment do Backstage, para esta:
image: socialchorus/backstage:latest

- FONTE:
https://hub.docker.com/layers/socialchorus/backstage/latest/images/sha256-449892f5da326734009967bfe2759a631f71c1c2b02da6ae0424601b09075641?context=explore
<https://hub.docker.com/layers/socialchorus/backstage/latest/images/sha256-449892f5da326734009967bfe2759a631f71c1c2b02da6ae0424601b09075641?context=explore>

- Ajustando
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml

~~~~bash

fernando@debian10x64:~$ kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
deployment.apps "backstage" deleted
fernando@debian10x64:~$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
deployment.apps/backstage created
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get pods --namespace=backstage
NAME                         READY   STATUS              RESTARTS   AGE
backstage-7894c44d6b-j8c8h   0/1     ContainerCreating   0          6s
postgres-77f59b67df-7wffm    1/1     Running             0          75m
fernando@debian10x64:~$
fernando@debian10x64:~$

~~~~




- Pod do Backstage subiu agora:

~~~~bash
fernando@debian10x64:~$ kubectl get pods --namespace=backstage
NAME                         READY   STATUS    RESTARTS   AGE
backstage-7894c44d6b-j8c8h   1/1     Running   0          72s
postgres-77f59b67df-7wffm    1/1     Running   0          76m
fernando@debian10x64:~$ date
Sat 24 Jun 2023 10:17:17 PM -03
fernando@debian10x64:~$
~~~~



- Porém o Pod usa uma porta diferente no "app-config.yaml":

~~~~bash

fernando@debian10x64:~$ kubectl exec -ti backstage-7894c44d6b-j8c8h -n backstage sh
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
#
#
#
#
# pwd
/app
# ls
app-config.yaml  node_modules  package.json  packages  yarn.lock
# cat app
cat: app: No such file or directory
# cat app-config.yaml
app:
  title: FirstUp Backstage App
  baseUrl: http://localhost:3000
  datadogRum:
    clientToken: pub75b925cfabe46b0dc92edcd5b0d88a43
    applicationId: 935994e9-cbc0-4cba-94be-eb672d67a566
    site: datadoghq.com


~~~~




# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- Efetuar commit e push da Docker image para o Docker Hub
- Tratar erro no Deployment do Backstage, com imagem Dockerhub publica foi(pod ficou running), mas service não vai ficar OK, porque o "app-config.yaml" usa porta diferente do Container no k8s.
- Buildar imagem Docker personalizada, contendo um "app-config.yaml" personalizado, usando a porta do container do Deployment do Backstage(7007).
- Ver sobre build de imagem Docker personalizada para o Backstage
    https://backstage.io/docs/deployment/docker/
    avaliar se o build da imagem Docker vai ser via:
    Host Build, ou Multi-stage Build, ou Separate Frontend
- Tratar erros do build da imagem Docker, arquivos locais.
- Criar passo-a-passo, para subir o projeto do Backstage em Kubernetes, a ordem dos manifestos e etc.
- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.









# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 05/08/2023

- Testando deployment via Kubernetes no cluster EKS

https://backstage.io/docs/deployment/k8s/#creating-a-namespace
<https://backstage.io/docs/deployment/k8s/#creating-a-namespace>


cd /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s
kubectl apply -f namespace.yaml


- Criando senha personalizada

~~~~bash
fernando@debian10x64:~$ echo -n "lab123456" | base64
bGFiMTIzNDU2
fernando@debian10x64:~$

~~~~


- Editado.
- Aplicando:
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-secrets.yaml

~~~~bash


fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-secrets.yaml
secret/postgres-secrets created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get secrets -n backstage
NAME                  TYPE                                  DATA   AGE
default-token-4qcc6   kubernetes.io/service-account-token   3      76s
postgres-secrets      Opaque                                2      5s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$


~~~~



- Aplicando
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml
persistentvolume/postgres-storage created
persistentvolumeclaim/postgres-storage-claim created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pv -n backstage
NAME               CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                              STORAGECLASS   REASON   AGE
postgres-storage   2G         RWO            Retain           Bound    backstage/postgres-storage-claim   manual                  14s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pvc -n backstage
NAME                     STATUS   VOLUME             CAPACITY   ACCESS MODES   STORAGECLASS   AGE
postgres-storage-claim   Bound    postgres-storage   2G         RWO            manual         19s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

~~~~




- Aplicando
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml

~~~~bash
fernando@debian10x64:~$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml
deployment.apps/postgres created
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get pods --namespace=backstage
NAME                        READY   STATUS    RESTARTS   AGE
postgres-77f59b67df-7wffm   1/1     Running   0          9s
fernando@debian10x64:~$


kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash

psql -U $POSTGRES_USER

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash
bash-5.1# psql -U $POSTGRES_USER
psql (13.2)
Type "help" for help.

backstage=#



~~~~






- Aplicando:
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml

~~~~bash


fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml
service/postgres created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get services --namespace=backstage
NAME       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
postgres   ClusterIP   172.20.63.248   <none>        5432/TCP   4s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$


~~~~




- Aplicando
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml
secret/backstage-secrets created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get secret -n backstage
NAME                  TYPE                                  DATA   AGE
backstage-secrets     Opaque                                1      6s
default-token-4qcc6   kubernetes.io/service-account-token   3      8m44s
postgres-secrets      Opaque                                2      7m33s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$


~~~~




- Editado deployment do Backstage.
- Aplicando:
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
deployment.apps/backstage created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get deployments --namespace=backstage
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
backstage   0/1     1            0           4s
postgres    1/1     1            1           4m25s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pods --namespace=backstage
NAME                         READY   STATUS              RESTARTS   AGE
backstage-854df67b6c-fmvcz   0/1     ContainerCreating   0          9s
postgres-77f59b67df-wxggr    1/1     Running             0          4m30s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pods --namespace=backstage
NAME                         READY   STATUS              RESTARTS   AGE
backstage-854df67b6c-fmvcz   0/1     ContainerCreating   0          16s
postgres-77f59b67df-wxggr    1/1     Running             0          4m37s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pods --namespace=backstage
NAME                         READY   STATUS    RESTARTS   AGE
backstage-854df67b6c-fmvcz   1/1     Running   0          4m56s
postgres-77f59b67df-wxggr    1/1     Running   0          9m17s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ date
Sat 05 Aug 2023 05:09:44 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

~~~~







~~~~YAML
# -f to tail, <pod> -c <container>
$ kubectl logs --namespace=backstage -f backstage-54bfcd6476-n2jkm -c backstage
~~~~


- Editado:

~~~~BASH

# -f to tail, <pod> -c <container>
kubectl logs --namespace=backstage -f backstage-854df67b6c-fmvcz -c backstage

~~~~




## Creating a Backstage service

Like the PostgreSQL service above, we need to create a Kubernetes Service for Backstage to handle connecting requests to the correct pods.

Create the Kubernetes Service descriptor:

~~~~YAML
# kubernetes/backstage-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: backstage
  namespace: backstage
spec:
  selector:
    app: backstage
  ports:
    - name: http
      port: 80
      targetPort: http
~~~~

The selector here is telling the Service which pods to target, and the port mapping translates normal HTTP port 80 to the backend http port (7007) on the pod.

Apply this Service to the Kubernetes cluster:

$ kubectl apply -f kubernetes/backstage-service.yaml
service/backstage created

Now we have a fully operational Backstage deployment! 🎉 For a grand reveal, you can forward a local port to the service:

$ sudo kubectl port-forward --namespace=backstage svc/backstage 80:80
Forwarding from 127.0.0.1:80 -> 7007

This shows port 7007 since port-forward doesn't really support services, so it cheats by looking up the first pod for a service and connecting to the mapped pod port.

Note that app.baseUrl and backend.baseUrl in your app-config.yaml should match what we're forwarding here (port omitted in this example since we're using the default HTTP port 80):

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

If you're using an auth provider, it should also have this address configured for the authentication pop-up to work properly.

Now you can open a browser on your machine to localhost and browse your Kubernetes-deployed Backstage instance. 





kubectl apply -f backstage-service.yaml
sudo kubectl port-forward --namespace=backstage svc/backstage 80:80


sudo kubectl port-forward --namespace=backstage svc/backstage 8282:8282



- ERRO

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f backstage-service.yaml
service/backstage created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl port-forward --namespace=backstage svc/backstage 80:80
Unable to listen on port 80: Listeners failed to create with the following errors: [unable to create listener: Error listen tcp4 127.0.0.1:80: bind: permission denied unable to create listener: Error listen tcp6 [::1]:80: bind: permission denied]
error: unable to listen on any of the requested ports: [{80 7007}]
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ sudo kubectl port-forward --namespace=backstage svc/backstage 80:80
[sudo] password for fernando:
The connection to the server localhost:8080 was refused - did you specify the right host or port?
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ sudo kubectl port-forward --namespace=backstage svc/backstage 8282:8282
The connection to the server localhost:8080 was refused - did you specify the right host or port?
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   172.20.0.1   <none>        443/TCP   51m
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get svc -A
NAMESPACE     NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
backstage     backstage    ClusterIP   172.20.34.247   <none>        80/TCP          63s
backstage     postgres     ClusterIP   172.20.63.248   <none>        5432/TCP        13m
default       kubernetes   ClusterIP   172.20.0.1      <none>        443/TCP         51m
kube-system   kube-dns     ClusterIP   172.20.0.10     <none>        53/UDP,53/TCP   51m
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$


fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ date
Sat 05 Aug 2023 05:17:31 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ sudo kubectl port-forward --namespace=backstage svc/backstage 80:80
The connection to the server localhost:8080 was refused - did you specify the right host or port?
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007
The connection to the server localhost:8080 was refused - did you specify the right host or port?





# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- Erro ao tentar expor Backstage via "kubectl port-forward". 
      ERRO:
      "The connection to the server localhost:8080 was refused - did you specify the right host or port?"
- Tratar erros na console do EKS, erro de IAM, devido mudança de conta AWS. Detalhe, tem conta IAM e IAM-Identity-Center, ver como lidar.
      A principio, resolvido, validar. lab7
      Ajustar lab9
- Ler:
      https://medium.com/rahasak/deploy-spotify-backstage-with-kubernetes-b769e755e402
- Ver como expor Backstage via ingress, Load Balancer, etc
- Verificar sobre PVC avançado.
- Criar passo-a-passo, para subir o projeto do Backstage em Kubernetes, a ordem dos manifestos e etc.
- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.











- Ajustando o Service do Backstage
porta
80
PARA:
7007

DE:

~~~~YAML
# kubernetes/backstage-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: backstage
  namespace: backstage
spec:
  selector:
    app: backstage
  ports:
    - name: http
      port: 80
      targetPort: http
~~~~


PARA:

~~~~YAML
# kubernetes/backstage-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: backstage
  namespace: backstage
spec:
  selector:
    app: backstage
  ports:
    - name: http
      port: 7007
      targetPort: http
~~~~


kubectl apply -f backstage-service.yaml







sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007


fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f backstage-service.yaml
service/backstage configured
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get svc -A
NAMESPACE     NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
backstage     backstage    ClusterIP   172.20.34.247   <none>        7007/TCP        11m
backstage     postgres     ClusterIP   172.20.63.248   <none>        5432/TCP        23m
default       kubernetes   ClusterIP   172.20.0.1      <none>        443/TCP         61m
kube-system   kube-dns     ClusterIP   172.20.0.10     <none>        53/UDP,53/TCP   61m
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007
The connection to the server localhost:8080 was refused - did you specify the right host or port?
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$








# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 06/08/2023


aws eks --region us-east-1 update-kubeconfig --name eks-lab

Apply complete! Resources: 8 added, 1 changed, 7 destroyed.

Outputs:

configure_kubectl = "aws eks --region us-east-1 update-kubeconfig --name eks-lab"
vpc_id = "vpc-0a9f90de740c75f4f"
fernando@debian10x64:~/cursos/terraform/eks-via-terraform-github-actions/09-eks-blueprint$ aws eks --region us-east-1 update-kubeconfig --name eks-lab
Updated context arn:aws:eks:us-east-1:552925778543:cluster/eks-lab in /home/fernando/.kube/config
fernando@debian10x64:~/cursos/terraform/eks-via-terraform-github-actions/09-eks-blueprint$


- Testando deployment via Kubernetes no cluster EKS

https://backstage.io/docs/deployment/k8s/#creating-a-namespace
<https://backstage.io/docs/deployment/k8s/#creating-a-namespace>


cd /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s
kubectl apply -f namespace.yaml

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-secrets.yaml
kubectl get secrets -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml
kubectl get pv -n backstage
kubectl get pvc -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml
kubectl get pods --namespace=backstage
kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash
psql -U $POSTGRES_USER

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml
kubectl get services --namespace=backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml
kubectl get secret -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

kubectl logs --namespace=backstage -f backstage-854df67b6c-bcb4m -c backstage

kubectl apply -f backstage-service.yaml
sudo kubectl port-forward --namespace=backstage svc/backstage 80:80
kubectl get services --namespace=backstage

sudo kubectl port-forward --namespace=backstage svc/backstage 8282:8282
kubectl get services --namespace=backstage


port-f

sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007


- ERRO

~~~~BASH
fernando@debian10x64:~$ sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007
[sudo] password for fernando:
The connection to the server localhost:8080 was refused - did you specify the right host or port?
fernando@debian10x64:~$
~~~~



- Ver sobre ingress-controller antes.
- Subir ALB na AWS.
- Ajustado o Service.
- Ver sobre ingress-controller antes.

- Ajustado o manifesto do Service, para NodePort e o nome dele:

~~~~YAML
# kubernetes/backstage-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: backstage
  namespace: backstage
spec:
  selector:
    app: backstage
  type: NodePort
  ports:
    - name: backstage-port
      #port: 80
      port: 7007
      targetPort: 7007
~~~~


Claro! Aqui está um exemplo de manifesto em YAML para criar um Ingress no Kubernetes e configurar um Application Load Balancer (ALB) na AWS:

~~~~yaml

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: meu-ingress
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/scheme: internet-facing
spec:
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: meu-app-service
                port:
                  number: 80
~~~~

Neste exemplo, você precisa substituir algumas partes para se adequar ao seu ambiente:

    metadata.name: Escolha um nome adequado para o seu Ingress.
    annotations: Essas são as anotações específicas do ALB para configurar o Ingress com o Load Balancer correto.
    backend.service.name: Substitua por o nome do serviço Kubernetes que você deseja expor.
    backend.service.port.number: Substitua pelo número da porta do serviço que você deseja expor.

Certifique-se de que você já tenha configurado um serviço Kubernetes para o aplicativo que você deseja expor antes de criar o Ingress. Lembre-se também de que, para utilizar o Ingress com o ALB na AWS, você precisará ter o controlador do ALB instalado e configurado no seu cluster Kubernetes.

Após criar esse manifesto, você pode aplicá-lo ao cluster Kubernetes executando o seguinte comando:

bash

kubectl apply -f nome-do-arquivo.yaml

Isso criará o Ingress e, por consequência, configurará o Application Load Balancer na AWS para direcionar o tráfego para o seu serviço Kubernetes. Certifique-se de ter as permissões corretas na AWS para criar e gerenciar recursos do ALB.

- Editado:
backstage/manifestos-k8s/backstage-ingress.yaml

~~~~YAML
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: backstage-ingress
  namespace: backstage
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/scheme: internet-facing
spec:
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: backstage
                port:
                  number: 7007
~~~~

- Aplicando:
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

kubectl get ingress -n backstage

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
ingress.networking.k8s.io/backstage-ingress created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get ingress -n backstage
NAME                CLASS    HOSTS   ADDRESS   PORTS   AGE
backstage-ingress   <none>   *                 80      13s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

~~~~

- Erros:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
ingress.networking.k8s.io/backstage-ingress created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get ingress -n backstage
NAME                CLASS    HOSTS   ADDRESS   PORTS   AGE
backstage-ingress   <none>   *                 80      13s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get ingress -n backstage
NAME                CLASS    HOSTS   ADDRESS   PORTS   AGE
backstage-ingress   <none>   *                 80      44s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get ingress -n backstage backstage-ingress
NAME                CLASS    HOSTS   ADDRESS   PORTS   AGE
backstage-ingress   <none>   *                 80      78s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl describe ingress -n backstage backstage-ingress
Name:             backstage-ingress
Labels:           <none>
Namespace:        backstage
Address:
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /   backstage:7007 (10.0.2.56:7007)
Annotations:  alb.ingress.kubernetes.io/scheme: internet-facing
              alb.ingress.kubernetes.io/target-type: ip
              kubernetes.io/ingress.class: alb
Events:
  Type     Reason             Age   From     Message
  ----     ------             ----  ----     -------
  Warning  FailedDeployModel  84s   ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: dd7f673b-1c1f-46c8-8eab-dec938cbba60
  Warning  FailedDeployModel  83s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: 4d34b2a6-0d33-46ad-ba2f-0317de532b50
  Warning  FailedDeployModel  83s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: aa3c56a3-3ea6-4bde-b24e-b6eb62967d60
  Warning  FailedDeployModel  82s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: 650a17a0-837c-4beb-9e1e-3d235a322a3f
  Warning  FailedDeployModel  82s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: 9666467d-c036-44e3-988b-04ebba359f07
  Warning  FailedDeployModel  82s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: ac35e626-16c3-4ca8-aed6-cc54b7075ca3
  Warning  FailedDeployModel  81s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: b25c7c30-c80f-4ed0-948e-7cad43139f8c
  Warning  FailedDeployModel  81s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: 4ebba1f9-e079-44a8-86db-b9246566c173
  Warning  FailedDeployModel  80s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: b4a3b290-11c6-443c-9acb-829e5faadd0d
  Warning  FailedDeployModel  39s (x5 over 78s)  ingress  (combined from similar events): Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691364252014129032 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:targetgroup/k8s-backstag-backstag-16938504f4/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: 34f50e9f-5d53-44e2-b7ff-6a030c0263f1
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

~~~~


- Verificando o tf state:
/home/fernando/cursos/terraform/eks-via-terraform-github-actions/09-eks-blueprint/terraform.tfstate

existe a role mencionada no erro

~~~~JSON

          "attributes": {
            "arn": "arn:aws:iam::552925778543:role/eks-lab-aws-load-balancer-controller-sa-irsa",
            "assume_role_policy": "{\"Statement\":[{\"Action\":\"sts:AssumeRoleWithWebIdentity\",\"Condition\":{\"StringLike\":{\"oidc.eks.us-east-1.amazonaws.com/id/FD1565A379BE9415D1118FCA12B29A53:aud\":\"sts.amazonaws.com\",\"oidc.eks.us-east-1.amazonaws.com/id/FD1565A379BE9415D1118FCA12B29A53:sub\":\"system:serviceaccount:kube-system:aws-load-balancer-controller-sa\"}},\"Effect\":\"Allow\",\"Principal\":{\"Federated\":\"arn:aws:iam::552925778543:oidc-provider/oidc.eks.us-east-1.amazonaws.com/id/FD1565A379BE9415D1118FCA12B29A53\"}}],\"Version\":\"2012-10-17\"}",
            "create_date": "2023-08-06T21:12:09Z",
            "description": "AWS IAM Role for the Kubernetes service account aws-load-balancer-controller-sa.",
            "force_detach_policies": true,
            "id": "eks-lab-aws-load-balancer-controller-sa-irsa",
            "inline_policy": [],
            "managed_policy_arns": [],
            "max_session_duration": 3600,
            "name": "eks-lab-aws-load-balancer-controller-sa-irsa",
            "name_prefix": "",
            "path": "/",
            "permissions_boundary": "",
            "tags": null,
            "tags_all": {},
            "unique_id": "AROAYBPHR2JX6L3SWV4DM"
          },
~~~~


- Anexando policy
ElasticLoadBalancingFullAccess

- Testando
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

kubectl get ingress -n backstage
kubectl describe ingress -n backstage backstage-ingress

- OK, agora parece estar tudo OK com o ingress:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
ingress.networking.k8s.io "backstage-ingress" deleted
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
ingress.networking.k8s.io/backstage-ingress created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get ingress -n backstage
NAME                CLASS    HOSTS   ADDRESS                                                                   PORTS   AGE
backstage-ingress   <none>   *       k8s-backstag-backstag-c3e6f62e16-1248054694.us-east-1.elb.amazonaws.com   80      6s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl describe ingress -n backstage backstage-ingress
Name:             backstage-ingress
Labels:           <none>
Namespace:        backstage
Address:          k8s-backstag-backstag-c3e6f62e16-1248054694.us-east-1.elb.amazonaws.com
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /   backstage:7007 (10.0.2.56:7007)
Annotations:  alb.ingress.kubernetes.io/scheme: internet-facing
              alb.ingress.kubernetes.io/target-type: ip
              kubernetes.io/ingress.class: alb
Events:
  Type    Reason                  Age   From     Message
  ----    ------                  ----  ----     -------
  Normal  SuccessfullyReconciled  21s   ingress  Successfully reconciled
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ date
Sun 06 Aug 2023 08:31:54 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

~~~~



- ALB
k8s-backstag-backstag-c3e6f62e16-1248054694.us-east-1.elb.amazonaws.com

k8s-backstag-backstag-c3e6f62e16-1248054694.us-east-1.elb.amazonaws.com

PÁGINA FICA CARREGANDO, tem titulo, mas nao mostra nada

- Logs do Pod do Backstage:

~~~~bash

{"level":"info","message":"10.0.0.234 - - [06/Aug/2023:23:33:57 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.1.120 - - [06/Aug/2023:23:34:09 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.2.170 - - [06/Aug/2023:23:34:09 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.0.234 - - [06/Aug/2023:23:34:12 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.1.120 - - [06/Aug/2023:23:34:24 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.2.170 - - [06/Aug/2023:23:34:24 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.0.234 - - [06/Aug/2023:23:34:27 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"entity":"location:default/generated-eeed3503740b7c4b80f2aad3e417fafee7a3803d","level":"warn","location":"file:/examples/entities.yaml","message":"file /examples/entities.yaml does not exist","plugin":"catalog","service":"backstage","type":"plugin"}
{"entity":"location:default/generated-c4d4a3f82d0b7ecef1bd7d6a1991be94fded46aa","level":"warn","location":"file:/examples/template/template.yaml","message":"file /examples/template/template.yaml does not exist","plugin":"catalog","service":"backstage","type":"plugin"}
{"entity":"location:default/generated-0ca6551527608b8e42ccccd463f27d4113d35ff1","level":"warn","location":"file:/examples/org.yaml","message":"file /examples/org.yaml does not exist","plugin":"catalog","service":"backstage","type":"plugin"}
{"level":"info","message":"10.0.1.120 - - [06/Aug/2023:23:34:39 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.2.170 - - [06/Aug/2023:23:34:39 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.0.234 - - [06/Aug/2023:23:34:42 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.0.234 - - [06/Aug/2023:23:34:48 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/116.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.1.120 - - [06/Aug/2023:23:34:54 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.2.170 - - [06/Aug/2023:23:34:54 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"ELB-HealthChecker/2.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"10.0.0.234 - - [06/Aug/2023:23:34:57 +0000] \"GET 
~~~~



git status
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git add -u
git reset -- manifestos-k8s/backstage-secrets.yaml
git commit -m "Backstage lab. Aplicação no K8S, ALB ON, mas página não abre ainda."
git push
git status





- Comentando o trecho sobre ip no alb
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

- ERRO

~~~~BASH

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl describe ingress -n backstage backstage-ingress
Name:             backstage-ingress
Labels:           <none>
Namespace:        backstage
Address:          k8s-backstag-backstag-c3e6f62e16-1248054694.us-east-1.elb.amazonaws.com
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /   backstage:7007 (10.0.2.56:7007)
Annotations:  alb.ingress.kubernetes.io/scheme: internet-facing
              kubernetes.io/ingress.class: alb
Events:
  Type     Reason                  Age                    From     Message
  ----     ------                  ----                   ----     -------
  Normal   SuccessfullyReconciled  5m51s (x2 over 9m37s)  ingress  Successfully reconciled
  Warning  FailedDeployModel       2s (x13 over 27s)      ingress  Failed deploy model due to InvalidParameter: 1 validation error(s) found.
- minimum field value of 1, CreateTargetGroupInput.Port.
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

~~~~


- Efetuando delete e aplicando novamente:
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
kubectl get ingress -n backstage backstage-ingress

kubectl describe ingress -n backstage backstage-ingress

- Segue com erro:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

ingress.networking.k8s.io "backstage-ingress" deleted
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
ingress.networking.k8s.io/backstage-ingress created
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get ingress -n backstage backstage-ingress
NAME                CLASS    HOSTS   ADDRESS   PORTS   AGE
backstage-ingress   <none>   *                 80      8s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl describe ingress -n backstage backstage-ingress
Name:             backstage-ingress
Labels:           <none>
Namespace:        backstage
Address:
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /   backstage:7007 (10.0.2.56:7007)
Annotations:  alb.ingress.kubernetes.io/scheme: internet-facing
              kubernetes.io/ingress.class: alb
Events:
  Type     Reason             Age                From     Message
  ----     ------             ----               ----     -------
  Warning  FailedDeployModel  5s (x11 over 13s)  ingress  Failed deploy model due to InvalidParameter: 1 validation error(s) found.
- minimum field value of 1, CreateTargetGroupInput.Port.
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

~~~~


- Removendo comentário no parametro "alb.ingress.kubernetes.io/target-type: ip" do ingress.
- Efetuando delete e aplicando novamente:
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
kubectl get ingress -n backstage backstage-ingress

kubectl describe ingress -n backstage backstage-ingress


Provisioning

kubectl exec -ti backstage-854df67b6c-bcb4m -n backstage -- sh


image: fernandomj90/backstage-mandragora:v3	
Imagem buildada que funciona no local da VM do Debian o acesso via navegador. Não funciona via ALB, devido o valor do "baseUrl" ser localhost.







# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- Ingress OK, porém a página do Backstage não abre, fica em branco, só consta o titulo da página.
      Verificar como montar uma pipeline para buildar imagem do Backstage de maneira rápida.
      Buildar imagem com valor do "baseUrl" sendo o DNS NAME do ALB.
- Erro ao tentar expor Backstage via "kubectl port-forward". 
      ERRO:
      "The connection to the server localhost:8080 was refused - did you specify the right host or port?"
- Ler:
      https://medium.com/rahasak/deploy-spotify-backstage-with-kubernetes-b769e755e402
- Verificar sobre PVC avançado.
      https://backstage.io/docs/deployment/k8s/#set-up-a-more-reliable-volume
- Criar passo-a-passo, para subir o projeto do Backstage em Kubernetes, a ordem dos manifestos e etc.
- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.












- Ingress OK, porém a página do Backstage não abre, fica em branco, só consta o titulo da página.
      Verificar como montar uma pipeline para buildar imagem do Backstage de maneira rápida.
      Buildar imagem com valor do "baseUrl" sendo o DNS NAME do ALB.




- Ajustado para o v4 no Deployment do Backstage
fernandomj90/backstage-mandragora:v4

- Aplicando
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
deployment.apps/backstage configured
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get deployments --namespace=backstage

NAME        READY   UP-TO-DATE   AVAILABLE   AGE
backstage   1/1     1            1           172m
postgres    1/1     1            1           178m
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pods --namespace=backstage
NAME                         READY   STATUS        RESTARTS   AGE
backstage-854df67b6c-bcb4m   1/1     Terminating   0          172m
backstage-857684b4b8-6rlp6   1/1     Running       0          7s
postgres-77f59b67df-s6vvd    1/1     Running       0          178m
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ date
Sun 06 Aug 2023 09:15:59 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$


~~~~




kubectl logs --namespace=backstage -f backstage-854df67b6c-bcb4m -c backstage
kubectl logs --namespace=backstage -f backstage-857684b4b8-6rlp6 -c backstage



Inbound security group rules successfully modified on security group (sg-00e851dc559ad9135 | k8s-traffic-ekslab-2b77b87e46)
Details

    New


Inbound security group rules successfully modified on security group (sg-0d2d20986864f05f4 | k8s-backstag-backstag-6d4c15e58c)
Details

    New



- Ao tentar abrir via navegador, segue com página em branco só com titulo:
http://k8s-backstag-backstag-c3e6f62e16-370703358.us-east-1.elb.amazonaws.com/





# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- IMPORTANTE, Deletar ALB/ingress
- Ingress OK, porém a página do Backstage não abre, fica em branco, só consta o titulo da página.
      Buildar imagem com valor do "baseUrl" sendo o DNS NAME do ALB. Exemplo: fernandomj90/backstage-mandragora:v4
      Verificar sobre app-config e parametro "upgrade-insecure-requests", página: https://roadie.io/blog/backstage-fargate-up-and-running/
      LER: https://roadie.io/blog/backstage-fargate-up-and-running/
      Revisar Security Groups, ALB e EC2. Encaminhamento de portas do ALB e TargetGroup.
- Verificar como montar uma pipeline para buildar imagem do Backstage de maneira rápida.
- Erro ao tentar expor Backstage via "kubectl port-forward". 
      ERRO:
      "The connection to the server localhost:8080 was refused - did you specify the right host or port?"
- Ler:
      https://medium.com/rahasak/deploy-spotify-backstage-with-kubernetes-b769e755e402
- Verificar sobre PVC avançado.
      https://backstage.io/docs/deployment/k8s/#set-up-a-more-reliable-volume
- Criar passo-a-passo, para subir o projeto do Backstage em Kubernetes, a ordem dos manifestos e etc.
- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- IMPORTANTE, Deletar ALB/ingress













# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 12/08/2023

- Ingress OK, porém a página do Backstage não abre, fica em branco, só consta o titulo da página.
      Buildar imagem com valor do "baseUrl" sendo o DNS NAME do ALB. Exemplo: fernandomj90/backstage-mandragora:v4
      Verificar sobre app-config e parametro "upgrade-insecure-requests", página: https://roadie.io/blog/backstage-fargate-up-and-running/
      LER: https://roadie.io/blog/backstage-fargate-up-and-running/
      Revisar Security Groups, ALB e EC2. Encaminhamento de portas do ALB e TargetGroup.



- Subindo Backstage via Kubernetes:

https://backstage.io/docs/deployment/k8s/#creating-a-namespace
<https://backstage.io/docs/deployment/k8s/#creating-a-namespace>

cd /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s
kubectl apply -f namespace.yaml

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-secrets.yaml
kubectl get secrets -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml
kubectl get pv -n backstage
kubectl get pvc -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml
kubectl get pods --namespace=backstage
kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash
psql -U $POSTGRES_USER

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml
kubectl get services --namespace=backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml
kubectl get secret -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

kubectl logs --namespace=backstage -f backstage-857684b4b8-vvh5w -c backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
kubectl get ingress --namespace=backstage



- ERROS NO INGRESS:

~~~~BASH
           status code: 403, request id: 8864b7cb-81a6-43ec-b00f-3d13d9ecec65
  Warning  FailedDeployModel  45s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691879688087945211 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:loadbalancer/app/k8s-backstag-backstag-c3e6f62e16/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: 663d293f-6ae0-4e0c-8407-e3d59880eb02
  Warning  FailedDeployModel  44s  ingress  Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691879688087945211 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:loadbalancer/app/k8s-backstag-backstag-c3e6f62e16/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: ba26be67-1f40-4951-be27-3ad1573c62fc
  Warning  FailedDeployModel  3s (x5 over 42s)  ingress  (combined from similar events): Failed deploy model due to AccessDenied: User: arn:aws:sts::552925778543:assumed-role/eks-lab-aws-load-balancer-controller-sa-irsa/1691879688087945211 is not authorized to perform: elasticloadbalancing:AddTags on resource: arn:aws:elasticloadbalancing:us-east-1:552925778543:loadbalancer/app/k8s-backstag-backstag-c3e6f62e16/* because no identity-based policy allows the elasticloadbalancing:AddTags action
           status code: 403, request id: 1e5e8ad0-05c9-403f-952d-745bf8de5d63
fernando@debian10x64:~$

~~~~


- Anexando policy AWS Managed abaixo na role "arn:aws:iam::552925778543:role/eks-lab-aws-load-balancer-controller-sa-irsa":
ElasticLoadBalancingFullAccess

- Testando
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

kubectl get ingress -n backstage
kubectl describe ingress -n backstage backstage-ingress

- Agora gerou a URL:

~~~~BASH

fernando@debian10x64:~$ kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

ingress.networking.k8s.io "backstage-ingress" deleted
fernando@debian10x64:~$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
ingress.networking.k8s.io/backstage-ingress created
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get ingress -n backstage
NAME                CLASS    HOSTS   ADDRESS   PORTS   AGE
backstage-ingress   <none>   *                 80      1s
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get ingress -n backstage
NAME                CLASS    HOSTS   ADDRESS                                                                   PORTS   AGE
backstage-ingress   <none>   *       k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com   80      31s
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ date
Sat 12 Aug 2023 07:39:35 PM -03
fernando@debian10x64:~$

~~~~



kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage









k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com

- Ao tentar abrir o Backstage via navegador:
k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com

Backend service does not exist






kubectl logs --namespace=backstage -f backstage-75f785b69c-zwxdw -c backstage




- Verificando na AWS Console
esta mensagem "Backend service does not exist" é mensagem fixa do ALB, ele não criou o TargetGroup para associar no Listener:

"Return fixed response

    Response code: 503
    Response body: Backend service does not exist
    Response content type: text/plain"

- Não existem TargetGroups criados.



kubectl get ingress --namespace=backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
kubectl get ingress --namespace=backstage

kubectl describe ingress --namespace=backstage backstage-ingress

~~~~bash

fernando@debian10x64:~$ kubectl describe ingress --namespace=backstage backstage-ingress
Name:             backstage-ingress
Labels:           <none>
Namespace:        backstage
Address:          k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /   backstage:7007 (<error: endpoints "backstage" not found>)
Annotations:  alb.ingress.kubernetes.io/scheme: internet-facing
              kubernetes.io/ingress.class: alb
Events:
  Type    Reason                  Age                From     Message
  ----    ------                  ----               ----     -------
  Normal  SuccessfullyReconciled  21s (x2 over 37m)  ingress  Successfully reconciled
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ date
Sat 12 Aug 2023 08:16:28 PM -03
fernando@debian10x64:~$
~~~~


- Criando Service

~~~~bash
fernando@debian10x64:~$ kubectl get services --namespace=backstage
NAME       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
postgres   ClusterIP   172.20.126.185   <none>        5432/TCP   44m
fernando@debian10x64:~$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-service.yaml
service/backstage created
fernando@debian10x64:~$ kubectl get services --namespace=backstage
NAME        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
backstage   NodePort    172.20.252.62    <none>        7007:32071/TCP   11s
postgres    ClusterIP   172.20.126.185   <none>        5432/TCP         46m
fernando@debian10x64:~$
~~~~


- Agora o ingress achou o ip do 

~~~~bash
fernando@debian10x64:~$ kubectl describe ingress --namespace=backstage backstage-ingress
Name:             backstage-ingress
Labels:           <none>
Namespace:        backstage
Address:          k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /   backstage:7007 (10.0.0.164:7007)
Annotations:  alb.ingress.kubernetes.io/scheme: internet-facing
              kubernetes.io/ingress.class: alb
Events:
  Type    Reason                  Age                From     Message
  ----    ------                  ----               ----     -------
  Normal  SuccessfullyReconciled  33s (x3 over 40m)  ingress  Successfully reconciled
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ date
Sat 12 Aug 2023 08:19:43 PM -03
fernando@debian10x64:~$
~~~~




- Na console AWS
criou o TargetGroup corretamente
Target group: k8s-backstag-backstag-6328999918
ALB já associou no Listener o TargetGroup






- Abrindo URL do Load Balancer, mesmo sintoma, página em branco com titulo "Scaffold":
http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com/



- Ajustando app-config e upando nova Docker Image:

DE:

~~~~YAML
app:
  title: Scaffolded Backstage App
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com:3000

organization:
  name: My Company

backend:
  # Used for enabling authentication, secret is shared by all backend plugins
  # See https://backstage.io/docs/auth/service-to-service-auth for
  # information on the format
  # auth:
  #   keys:
  #     - secret: ${BACKEND_SECRET}
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com:7007
  listen:
    port: 7007
    # Uncomment the following host directive to bind to specific interfaces
    host: 0.0.0.0
  csp:
    connect-src: ["'self'", 'http:', 'https:']
~~~~


PARA:

~~~~YAML
app:
  title: Scaffolded Backstage App
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com

organization:
  name: My Company

backend:
  # Used for enabling authentication, secret is shared by all backend plugins
  # See https://backstage.io/docs/auth/service-to-service-auth for
  # information on the format
  # auth:
  #   keys:
  #     - secret: ${BACKEND_SECRET}
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com
  listen:
    port: 7007
    # Uncomment the following host directive to bind to specific interfaces
    host: 0.0.0.0
  csp:
    connect-src: ["'self'", 'http:', 'https:']
    upgrade-insecure-requests: false # For tutorial purposes only
~~~~


- Foi editado:
      url, removidas as portas
      adicionado o "upgrade-insecure-requests: false"


- Buildando:
cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-mandragora:v6 .
docker push fernandomj90/backstage-mandragora:v6


- Ajustando o Deployment:

          image: fernandomj90/backstage-mandragora:v6



kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

kubectl logs --namespace=backstage -f backstage-54bb7f778-lspmx -c backstage



- Testando
- Abrindo URL do ALB:
k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com

- OK!
- Abriu a página do Backstage:
http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com/catalog?filters%5Bkind%5D=component&filters%5Buser%5D=all




- Destroy
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/namespace.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/namespace.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/namespace.yaml

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ kubectl delete -f namespace.yaml
error: the path "namespace.yaml" does not exist
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/namespace.yaml
namespace "backstage" deleted


fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
~~~~

Successfully deleted volume vol-02672888638f9cd9a.



# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- IMPORTANTE, Deletar ALB/ingress
- Subir LAB e seguir configuração do Backstage:
        https://backstage.io/docs/getting-started/configuration
        Configurar integraçaõ com GitHub.
        Subir aplicativo e Template de teste.
        Ler restante da documentação do Backstage.
- LER: https://roadie.io/blog/backstage-fargate-up-and-running/
- Verificar como montar uma pipeline para buildar imagem do Backstage de maneira rápida.
- Erro ao tentar expor Backstage via "kubectl port-forward". 
      ERRO:
      "The connection to the server localhost:8080 was refused - did you specify the right host or port?"
- Ler:
      https://medium.com/rahasak/deploy-spotify-backstage-with-kubernetes-b769e755e402
- Verificar sobre PVC avançado.
      https://backstage.io/docs/deployment/k8s/#set-up-a-more-reliable-volume
- Criar passo-a-passo, para subir o projeto do Backstage em Kubernetes, a ordem dos manifestos e etc.
- Documentar sobre expor o Backstage, questão do "upgrade-insecure-requests: false # For tutorial purposes only" no app-config em "csp".
- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- IMPORTANTE, Deletar ALB/ingress













# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 19/08/2023

- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- IMPORTANTE, Deletar ALB/ingress
- Subir LAB e seguir configuração do Backstage:
        https://backstage.io/docs/getting-started/configuration
        Configurar integraçaõ com GitHub.
        Subir aplicativo e Template de teste.
        Ler restante da documentação do Backstage.
- LER: https://roadie.io/blog/backstage-fargate-up-and-running/



- Subindo Backstage via Kubernetes:

https://backstage.io/docs/deployment/k8s/#creating-a-namespace
<https://backstage.io/docs/deployment/k8s/#creating-a-namespace>

cd /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s
kubectl apply -f namespace.yaml

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-secrets.yaml
kubectl get secrets -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml
kubectl get pv -n backstage
kubectl get pvc -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml
kubectl get pods --namespace=backstage
kubectl exec -it --namespace=backstage postgres-77f59b67df-wxggr -- /bin/bash
psql -U $POSTGRES_USER

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml
kubectl get services --namespace=backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml
kubectl get secret -n backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage
kubectl get pods --namespace=backstage

kubectl logs --namespace=backstage -f backstage-854df67b6c-fmvcz -c backstage

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-service.yaml
kubectl get services --namespace=backstage
sudo kubectl port-forward --namespace=backstage svc/backstage 7007:7007

- Anexando policy AWS Managed abaixo na role "arn:aws:iam::552925778543:role/eks-lab-aws-load-balancer-controller-sa-irsa":
ElasticLoadBalancingFullAccess

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml
kubectl get ingress --namespace=backstage

- Buildar Docker Image com URL atualizada do ALB.

docker push fernandomj90/backstage-mandragora:latest

- Ajustar Docker image atualizada no Deployment:

kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl get deployments --namespace=backstage


- URL do ingress atual

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ kubectl get ingress --namespace=backstage
NAME                CLASS    HOSTS   ADDRESS                                                                   PORTS   AGE
backstage-ingress   <none>   *       k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com   80      17m
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

~~~~







kubectl exec --namespace=backstage -ti backstage-6c949b9bc-m7jwv -- sh



- Sobre as variáveis do POSTGRES
https://backstage.io/docs/deployment/k8s/#creating-a-backstage-deployment
<https://backstage.io/docs/deployment/k8s/#creating-a-backstage-deployment>


- Verificando ip do Service do Postgres:

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get services --namespace=backstage
NAME        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
backstage   NodePort    172.20.31.29    <none>        7007:32585/TCP   70m
postgres    ClusterIP   172.20.132.12   <none>        5432/TCP         71m
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
~~~~


- Informação, confirmando que as variáveis do POSTGRES são preenchidas automaticamente pelo Kubernetes no Container do Backstage:

There is no special wiring needed to access the PostgreSQL service. Since it's running on the same cluster, Kubernetes will inject POSTGRES_SERVICE_HOST and POSTGRES_SERVICE_PORT environment variables into our Backstage container. These can be used in the Backstage app-config.yaml along with the secrets:

~~~~yaml
backend:
  database:
    client: pg
    connection:
      host: ${POSTGRES_SERVICE_HOST}
      port: ${POSTGRES_SERVICE_PORT}
      user: ${POSTGRES_USER}
      password: ${POSTGRES_PASSWORD}
~~~~


- Acessando o container do Backstage e confirmando a existencia das variáveis:

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ kubectl exec --namespace=backstage -ti backstage-6c949b9bc-m7jwv -- sh
$ env
GITHUB_TOKEN=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
KUBERNETES_PORT=tcp://172.20.0.1:443
KUBERNETES_SERVICE_PORT=443
NODE_VERSION=18.17.0
HOSTNAME=backstage-6c949b9bc-m7jwv
YARN_VERSION=1.22.19
BACKSTAGE_SERVICE_PORT_BACKSTAGE_PORT=7007
BACKSTAGE_PORT_7007_TCP_ADDR=172.20.31.29
HOME=/home/node
BACKSTAGE_SERVICE_HOST=172.20.31.29
BACKSTAGE_PORT_7007_TCP_PORT=7007
BACKSTAGE_PORT_7007_TCP_PROTO=tcp
BACKSTAGE_SERVICE_PORT=7007
BACKSTAGE_PORT=tcp://172.20.31.29:7007
TERM=xterm
BACKSTAGE_PORT_7007_TCP=tcp://172.20.31.29:7007
KUBERNETES_PORT_443_TCP_ADDR=172.20.0.1
POSTGRES_PASSWORD=lab123456
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
POSTGRES_PORT_5432_TCP_ADDR=172.20.132.12
KUBERNETES_PORT_443_TCP_PORT=443
POSTGRES_SERVICE_HOST=172.20.132.12
KUBERNETES_PORT_443_TCP_PROTO=tcp
POSTGRES_USER=backstage
POSTGRES_PORT_5432_TCP_PORT=5432
POSTGRES_PORT_5432_TCP_PROTO=tcp
POSTGRES_SERVICE_PORT=5432
POSTGRES_PORT=tcp://172.20.132.12:5432
KUBERNETES_PORT_443_TCP=tcp://172.20.0.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
POSTGRES_PORT_5432_TCP=tcp://172.20.132.12:5432
KUBERNETES_SERVICE_HOST=172.20.0.1
PWD=/app
NODE_ENV=production
$
~~~~




- Sobre o app-config.
- É considerado o YAML de configuração passado via parametro --config, conforme informação obtida na página abaixo:
https://backstage.io/docs/conf/writing/#configuration-files
<https://backstage.io/docs/conf/writing/#configuration-files>

- Detalhado:

~~~~bash
Configuration Files

It is possible to have multiple configuration files (bundled and/or remote*), both to support different environments, but also to define configuration that is local to specific packages. The configuration files to load are selected using a --config <local-path|url> flag, and it is possible to load any number of files. Paths are relative to the working directory of the executed process, for example package/backend. This means that to select a config file in the repo root when running the backend, you would use --config ../../my-config.yaml, and for config file on a config server you would use --config https://some.domain.io/app-config.yaml

Note: In case URLs are passed, it is also needed to set the remote option in the loadBackendConfig call.

If no config flags are specified, the default behavior is to load app-config.yaml and, if it exists, app-config.local.yaml from the repo root. In the provided project setup, app-config.local.yaml is .gitignore'd, making it a good place to add config overrides and secrets for local development.

Note that if any config flags are provided, the default app-config.yaml files are NOT loaded. To include them you need to explicitly include them with a flag, for example:

yarn start --config ../../app-config.yaml --config ../../app-config.staging.yaml --config https://some.domain.io/app-config.yaml

All l
~~~~

- No caso do Backstage que estou usando, durante o build é passado este arquivo de config:
CMD ["node", "packages/backend", "--config", "app-config.yaml"]



- Agora é necessário:
Dar seguimento ao tutorial de inicio no Backstage:
https://backstage.io/docs/getting-started/configuration
Configurar o banco de dados, mudando de SQLITE para POSTGRES.







# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- IMPORTANTE, Deletar ALB/ingress
- Subir LAB e seguir configuração do Backstage:
        https://backstage.io/docs/getting-started/configuration
        Configurar o banco de dados, mudando de SQLITE para POSTGRES.
        Configurar integraçaõ com GitHub.
        Subir aplicativo e Template de teste.
        Ler restante da documentação do Backstage.
- LER: https://roadie.io/blog/backstage-fargate-up-and-running/
- Verificar como montar uma pipeline para buildar imagem do Backstage de maneira rápida. Usar Makefile???
- Erro ao tentar expor Backstage via "kubectl port-forward". 
      ERRO:
      "The connection to the server localhost:8080 was refused - did you specify the right host or port?"
- Ler:
      https://medium.com/rahasak/deploy-spotify-backstage-with-kubernetes-b769e755e402
- Verificar sobre PVC avançado.
      https://backstage.io/docs/deployment/k8s/#set-up-a-more-reliable-volume
- Criar passo-a-passo, para subir o projeto do Backstage em Kubernetes, a ordem dos manifestos e etc.
- Documentar sobre expor o Backstage, questão do "upgrade-insecure-requests: false # For tutorial purposes only" no app-config em "csp".
- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- IMPORTANTE, Deletar ALB/ingress








- Continuando
        Configurar o banco de dados, mudando de SQLITE para POSTGRES.
        Configurar integraçaõ com GitHub.
        Subir aplicativo e Template de teste.
        Ler restante da documentação do Backstage.


https://backstage.io/docs/getting-started/configuration
Use your favorite editor to open app-config.yaml and add your PostgreSQL configuration in the root directory of your Backstage app using the credentials from the previous steps.

- Ajustando o app-config
/home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3/app-config.yaml

DE:

~~~~YAML

backend:
  # Used for enabling authentication, secret is shared by all backend plugins
  # See https://backstage.io/docs/auth/service-to-service-auth for
  # information on the format
  # auth:
  #   keys:
  #     - secret: ${BACKEND_SECRET}
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com
  listen:
    port: 7007
    # Uncomment the following host directive to bind to specific interfaces
    host: 0.0.0.0
  csp:
    connect-src: ["'self'", 'http:', 'https:']
    upgrade-insecure-requests: false # For tutorial purposes only
    # Content-Security-Policy directives follow the Helmet format: https://helmetjs.github.io/#reference
    # Default Helmet Content-Security-Policy values can be removed by setting the key to false
  cors:
    origin: http://k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com:3000
    methods: [GET, HEAD, PATCH, POST, PUT, DELETE]
    credentials: true
  # This is for local development only, it is not recommended to use this in production
  # The production database configuration is stored in app-config.production.yaml
  database:
    client: better-sqlite3
    connection: ':memory:'
  # workingDirectory: /tmp # Use this to configure a working directory for the scaffolder, defaults to the OS temp-dir

~~~~



PARA:

~~~~YAML

backend:
  # Used for enabling authentication, secret is shared by all backend plugins
  # See https://backstage.io/docs/auth/service-to-service-auth for
  # information on the format
  # auth:
  #   keys:
  #     - secret: ${BACKEND_SECRET}
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com
  listen:
    port: 7007
    # Uncomment the following host directive to bind to specific interfaces
    host: 0.0.0.0
  csp:
    connect-src: ["'self'", 'http:', 'https:']
    upgrade-insecure-requests: false # For tutorial purposes only
    # Content-Security-Policy directives follow the Helmet format: https://helmetjs.github.io/#reference
    # Default Helmet Content-Security-Policy values can be removed by setting the key to false
  cors:
    origin: http://k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com:3000
    methods: [GET, HEAD, PATCH, POST, PUT, DELETE]
    credentials: true
  # This is for local development only, it is not recommended to use this in production
  # The production database configuration is stored in app-config.production.yaml
  database:
    client: pg
    connection:
      host: ${POSTGRES_HOST}
      port: ${POSTGRES_PORT}
      user: ${POSTGRES_USER}
      password: ${POSTGRES_PASSWORD}
  # workingDirectory: /tmp # Use this to configure a working directory for the scaffolder, defaults to the OS temp-dir

~~~~





https://backstage.io/docs/getting-started/configuration
<https://backstage.io/docs/getting-started/configuration>

## Setting up authentication

There are multiple authentication providers available for you to use with Backstage, feel free to follow the instructions for adding authentication.

For this tutorial we choose to use GitHub, a free service most of you might be familiar with. For other options, see the auth provider documentation.

Go to https://github.com/settings/applications/new to create your OAuth App. The Homepage URL should point to Backstage's frontend, in our tutorial it would be http://localhost:3000. The Authorization callback URL will point to the auth backend, which will most likely be http://localhost:7007/api/auth/github/handler/frame.




http://k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com/catalog?filters%5Bkind%5D=component&filters%5Buser%5D=all

http://k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com/
http://k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com/api/auth/github/handler/frame



Client ID
e5ff884b08192f9416a6

Secret
omitido

Take note of the Client ID and the Client Secret. Open app-config.yaml, and add your clientId and clientSecret to this file. It should end up looking like this:

- Editando app-config
/home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3/app-config.yaml

DE:

~~~~YAML
auth:
  # see https://backstage.io/docs/auth/ to learn about auth providers
  providers: {}
~~~~

PARA:

~~~~YAML

auth:
  # see https://backstage.io/docs/auth/ to learn about auth providers
  environment: development
  providers:
    github:
      development:
        clientId: e5ff884b08192f9416a6
        clientSecret: f9b2edbbe926a441c31b1c186425317680b672fb
~~~~





## Add sign-in option to the frontend

Backstage will re-read the configuration. If there's no errors, that's great! We can continue with the last part of the configuration. The next step is needed to change the sign-in page, this you actually need to add in the source code.

Open packages/app/src/App.tsx and below the last import line, add:
packages/app/src/App.tsx

~~~~TYPESCRIPT
import { githubAuthApiRef } from '@backstage/core-plugin-api';
import { SignInPage } from '@backstage/core-components';
~~~~

Search for const app = createApp({ in this file, and below apis, add:
packages/app/src/App.tsx

~~~~TYPESCRIPT
components: {
  SignInPage: props => (
    <SignInPage
      {...props}
      auto
      provider={{
        id: 'github-auth-provider',
        title: 'GitHub',
        message: 'Sign in using GitHub',
        apiRef: githubAuthApiRef,
      }}
    />
  ),
},
~~~~

    Note: The default Backstage app comes with a guest Sign In Resolver. This resolver makes all users share a single "guest" identity and is only intended as a minimum requirement to quickly get up and running. You can read more about how Sign In Resolvers play a role in creating a Backstage User Identity for logged in users.

Restart Backstage from the terminal, by stopping it with Control-C, and starting it with yarn dev . You should be welcomed by a login prompt!

    Note: Sometimes the frontend starts before the backend resulting in errors on the sign in page. Wait for the backend to start and then reload Backstage to proceed.




- Editando
/home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3/packages/app/src/App.tsx





- Sobre o "app-config.local.yaml":
    If no config flags are specified, the default behavior is to load app-config.yaml and, if it exists, app-config.local.yaml from the repo root. In the provided project setup, app-config.local.yaml is .gitignore'd, making it a good place to add config overrides and secrets for local development.


- Ajustando o "app-config.local.yaml"
/home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3/app-config.local.yaml

adicionadas configurações sensiveis nele

- Criado gitignore
adicionado o seguinte:
backstage/docker/multi-stage/tentativa3/app-config.local.yaml
docker/multi-stage/tentativa3/app-config.local.yaml
app-config.local.yaml





https://backstage.io/docs/getting-started/configuration
<https://backstage.io/docs/getting-started/configuration>

## Setting up a GitHub Integration

The GitHub integration supports loading catalog entities from GitHub or GitHub Enterprise. Entities can be added to static catalog configuration, registered with the catalog-import plugin, or discovered from a GitHub organization. Users and Groups can also be loaded from an organization. While using GitHub Apps might be the best way to set up integrations, for this tutorial you'll use a Personal Access Token.

Create your Personal Access Token by opening the GitHub token creation page. Use a name to identify this token and put it in the notes field. Choose a number of days for expiration. If you have a hard time picking a number, we suggest to go for 7 days, it's a lucky number.

-Criando:
backstage-mandragora-token-19-08-2023


Set the scope to your likings. For this tutorial, selecting repo and workflow is required as the scaffolding job in this guide configures a GitHub actions workflow for the newly created project.

For this tutorial, we will be writing the token to app-config.local.yaml. This file might not exist for you, so if it doesn't go ahead and create it alongside the app-config.yaml at the root of the project. This file should also be excluded in .gitignore, to avoid accidental committing of this file.

In your app-config.local.yaml go ahead and add the following:
app-config.local.yaml

~~~~YAML
integrations:
  github:
    - host: github.com
      token: ghp_urtokendeinfewinfiwebfweb # this should be the token from GitHub
~~~~

That's settled. This information will be leveraged by other plugins.

If you're looking for a more production way to manage this secret, then you can do the following with the token being stored in an environment variable called GITHUB_TOKEN.
app-config.local.yaml

~~~~YAML
integrations:
  github:
    - host: github.com
      token: ${GITHUB_TOKEN} # this will use the environment variable GITHUB_TOKEN
~~~~

    Note: If you've updated the configuration for your integration, it's likely that the backend will need a restart to apply these changes. To do this, stop the running instance in your terminal with Control-C, then start it again with yarn dev. Once the backend has restarted, retry the operation.



- Editando
removendo no app-config
/home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3/app-config.yaml

~~~~YAML
integrations:
  github:
    - host: github.com
      # This is a Personal Access Token or PAT from GitHub. You can find out how to generate this token, and more information
      # about setting up the GitHub integration here: https://backstage.io/docs/getting-started/configuration#setting-up-a-github-integration
      token: ${GITHUB_TOKEN}
    ### Example for how to add your GitHub Enterprise instance using the API:
    # - host: ghe.example.net
    #   apiBaseUrl: https://ghe.example.net/api/v3
    #   token: ${GHE_TOKEN}
~~~~


- Ajustando o "app-config.local.yaml":
/home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3/app-config.local.yaml

~~~~YAML
integrations:
  github:
    - host: github.com
      token: ghp_urtokendeinfewinfiwebfweb # this should be the token from GitHub

~~~~








- Novo build, devido ajustes nas configurações do app-config, Github, etc
ajustado baseurl
baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com:7007

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get ingress --namespace=backstage
NAME                CLASS    HOSTS   ADDRESS                                                                   PORTS   AGE
backstage-ingress   <none>   *       k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com   80      4m12s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$


cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-mandragora:latest .
docker push fernandomj90/backstage-mandragora:latest


~~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker push fernandomj90/backstage-mandragora:latest
The push refers to repository [docker.io/fernandomj90/backstage-mandragora]
750365bcbf97: Pushed
78da13a0bc11: Pushed
f7d9559dab6e: Layer already exists
ba18ff0e8af7: Layer already exists
7e4519697b19: Layer already exists
ac4716367973: Layer already exists
4b497447f3f1: Layer already exists
98342cba36b0: Layer already exists
ec66eb809bcb: Layer already exists
de28499355cf: Layer already exists
4b3ba104e9a8: Layer already exists
latest: digest: sha256:eaa77d236948928d9cf80f9e624d6135963e4b3b24433657f64a1cea30a78300 size: 2629
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ date
Sat 19 Aug 2023 11:43:49 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

~~~~~


kubectl get pods --namespace=backstage

kubectl delete pod --namespace=backstage backstage-6c949b9bc-m7jwv

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pods --namespace=backstage
NAME                        READY   STATUS    RESTARTS   AGE
backstage-6c949b9bc-m7jwv   1/1     Running   0          160m
postgres-77f59b67df-jkmgg   1/1     Running   0          178m
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl delete pod --namespace=backstage backstage-6c949b9bc-m7jwv
pod "backstage-6c949b9bc-m7jwv" deleted

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get pods --namespace=backstage
NAME                        READY   STATUS    RESTARTS   AGE
backstage-6c949b9bc-925ds   1/1     Running   0          73s
postgres-77f59b67df-jkmgg   1/1     Running   0          3h
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$

~~~~




## Login to Backstage and check profile

Open your Backstage frontend. You should see your login screen if you're not logged in yet. As soon as you've logged in, go to Settings, you'll see your profile. Hopefully you'll recognize the profile picture and name on your screen, otherwise something went terribly wrong.
Register an existing component

## Register a new component, by going to create and choose Register existing component

Software template main screen, with a blue button to add an existing component

As URL use https://github.com/backstage/demo/blob/master/catalog-info.yaml. This is used by our demo site.

- Ao tentar adicionar
https://github.com/backstage/demo/blob/master/catalog-info.yaml

- Ocorre o erro abaixo na GUI:
{"error":{"name":"InputError","message":"Error: Unable to read url, Error: https://github.com/backstage/demo/tree/master/catalog-info.yaml could not be read as https://api.github.com/repos/backstage/demo/contents/catalog-info.yaml?ref=master, 401 Unauthorized"},"request":{"method":"POST","url":"/locations?dryRun=true"},"response":{"statusCode":400}}






## DESTROY

cd /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s

kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-ingress.yaml

kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-service.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/backstage-secrets.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-service.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-storage.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres-secrets.yaml
kubectl delete -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/namespace.yaml

kubectl get all -n backstage
kubectl get ingress -n backstage






# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- IMPORTANTE, Deletar ALB/ingress
- Subir LAB e seguir configuração do Backstage:
        https://backstage.io/docs/getting-started/configuration
        Configurar o banco de dados, mudando de SQLITE para POSTGRES.
        Configurar integraçaõ com GitHub.
        Subir aplicativo e Template de teste.
        Ler restante da documentação do Backstage.
- TSHOOT, erro durante create de component. Avaliar se é necessário adicionar o site de exemplo no Github que tem as credenciais(meu Github)
          {"error":{"name":"InputError","message":"Error: Unable to read url, Error: https://github.com/backstage/demo/tree/master/catalog-info.yaml could not be read as https://api.github.com/repos/backstage/demo/contents/catalog-info.yaml?ref=master, 401 Unauthorized"},"request":{"method":"POST","url":"/locations?dryRun=true"},"response":{"statusCode":400}}
- LER: https://roadie.io/blog/backstage-fargate-up-and-running/
- Verificar como montar uma pipeline para buildar imagem do Backstage de maneira rápida. Usar Makefile???
- Erro ao tentar expor Backstage via "kubectl port-forward". 
      ERRO:
      "The connection to the server localhost:8080 was refused - did you specify the right host or port?"
- Ler:
      https://medium.com/rahasak/deploy-spotify-backstage-with-kubernetes-b769e755e402
- Verificar sobre PVC avançado.
      https://backstage.io/docs/deployment/k8s/#set-up-a-more-reliable-volume
- Criar passo-a-passo, para subir o projeto do Backstage em Kubernetes, a ordem dos manifestos e etc.
- Documentar sobre expor o Backstage, questão do "upgrade-insecure-requests: false # For tutorial purposes only" no app-config em "csp".
- Documentar sobre app-config local e variáveis sensiveis.
- IMPORTANTE, deletar PVC/EBS manualmente ao final do lab.
- IMPORTANTE, Deletar ALB/ingress


