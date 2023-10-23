





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
cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t backstage-tentativa3 .

- Executando container
docker run -it -p 7007:7007 backstage-tentativa3

- Push de Container
https://dev.to/pmutua/pushing-a-docker-container-image-to-docker-hub-m3p
docker build -t <hub-user>/<repo-name>[:<tag>]
docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]
docker commit <existing-container> <hub-user>/<repo-name>[:<tag>] 
docker push <hub-user>/<repo-name>:<tag>

- RESUMO
Buildando e efetuando Push ao Docker Hub
cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-mandragora:v4 .
docker push fernandomj90/backstage-mandragora:v4







# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 22/10/2023