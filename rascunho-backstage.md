

## Git

git status
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git add .
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