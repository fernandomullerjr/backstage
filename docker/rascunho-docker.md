

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

