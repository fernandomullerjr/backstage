

## Git

git status
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git add -u
git reset -- manifestos-k8s/backstage-secrets.yaml
git commit -m "Backstage lab."
git push
git status




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
fernando@debian10x64:~$ kubectl apply -f /home/fernando/cursos/idp-devportal/backstage/manifestos-k8s/postgres.yaml
deployment.apps/postgres created
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ kubectl get pods --namespace=backstage
NAME                        READY   STATUS    RESTARTS   AGE
postgres-77f59b67df-7wffm   1/1     Running   0          9s
fernando@debian10x64:~$


kubectl exec -it --namespace=backstage postgres-77f59b67df-7wffm -- /bin/bash

psql -U $POSTGRES_USER

fernando@debian10x64:~$ kubectl exec -it --namespace=backstage postgres-77f59b67df-7wffm -- /bin/bash
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


## PENDENTE
- Ver sobre build de imagem Docker personalizada para o Backstage
    https://backstage.io/docs/deployment/docker/
    avaliar se o build da imagem Docker vai ser via:
    Host Build, ou Multi-stage Build, ou Separate Frontend
a