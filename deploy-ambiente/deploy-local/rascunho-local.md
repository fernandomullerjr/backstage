
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





- Node do 

~~~~bash
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
- Buildar imagem publica no Docker Hub, com portas para acesso local.
- Efetuar deploy do Backstage no Kubernetes Local.