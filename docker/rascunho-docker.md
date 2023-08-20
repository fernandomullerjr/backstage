





# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
##  Git - Docker image

git status
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git add -u
git reset -- manifestos-k8s/backstage-secrets.yaml
git commit -m "Backstage lab, buildando Docker image."
git push
git status




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
##  Docker image - Efetuando a criação

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





# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 01/07/2023


https://backstage.io/docs/deployment/docker/
<https://backstage.io/docs/deployment/docker/>

This section assumes that an app has already been created with @backstage/create-app, in which the frontend is bundled and served from the backend. This is done using the @backstage/plugin-app-backend plugin, which also injects the frontend configuration into the app. This means that you only need to build and deploy a single container in a minimal setup of Backstage. If you wish to separate the serving of the frontend out from the backend, see the separate frontend topic below.

Conforme a mensagem acima, é necessário criar o app antes, seguindo as instruções da página abaixo:
https://backstage.io/docs/getting-started/create-an-app/
<https://backstage.io/docs/getting-started/create-an-app/>


## INSTALANDO APP - BACKSTAGE

- Instalando app:
npx @backstage/create-app@latest

- Erro ao tentar instalar:

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/app$ node -v
v16.16.0
fernando@debian10x64:~/cursos/idp-devportal/backstage/app$ npx @backstage/create-app@latest
Need to install the following packages:
  @backstage/create-app@0.5.2
Ok to proceed? (y) y
? Enter a name for the app [required] meu-backstage-app

Creating the app...

 Checking if the directory is available:
  checking      meu-backstage-app ✔

 Creating a temporary app directory:
  creating      temporary directory ✔

 Preparing files:
  copying       .dockerignore ✔
  templating    .eslintrc.js.hbs ✔
  templating    .gitignore.hbs ✔
  copying       README.md ✔
  copying       .prettierignore ✔
  copying       app-config.local.yaml ✔
  copying       app-config.production.yaml ✔
  templating    app-config.yaml.hbs ✔
  templating    backstage.json.hbs ✔
  templating    catalog-info.yaml.hbs ✔
  copying       lerna.json ✔
  templating    package.json.hbs ✔
  copying       tsconfig.json ✔
  copying       yarn.lock ✔
  copying       README.md ✔
  copying       entities.yaml ✔
  copying       org.yaml ✔
  copying       template.yaml ✔
  copying       catalog-info.yaml ✔
  copying       index.js ✔
  copying       package.json ✔
  copying       README.md ✔
  templating    .eslintrc.js.hbs ✔
  copying       Dockerfile ✔
  copying       README.md ✔
  templating    package.json.hbs ✔
  copying       index.test.ts ✔
  copying       index.ts ✔
  copying       types.ts ✔
  copying       auth.ts ✔
  copying       app.ts ✔
  copying       catalog.ts ✔
  copying       scaffolder.ts ✔
  templating    search.ts.hbs ✔
  copying       techdocs.ts ✔
  copying       proxy.ts ✔
  copying       .eslintignore ✔
  templating    .eslintrc.js.hbs ✔
  copying       cypress.json ✔
  templating    package.json.hbs ✔
  copying       android-chrome-192x192.png ✔
  copying       apple-touch-icon.png ✔
  copying       favicon-16x16.png ✔
  copying       favicon-32x32.png ✔
  copying       index.html ✔
  copying       manifest.json ✔
  copying       robots.txt ✔
  copying       safari-pinned-tab.svg ✔
  copying       favicon.ico ✔
  copying       .eslintrc.json ✔
  copying       app.js ✔
  copying       App.test.tsx ✔
  copying       App.tsx ✔
  copying       apis.ts ✔
  copying       index.tsx ✔
  copying       setupTests.ts ✔
  copying       EntityPage.tsx ✔
  copying       SearchPage.tsx ✔
  copying       LogoIcon.tsx ✔
  copying       LogoFull.tsx ✔
  copying       Root.tsx ✔
  copying       index.ts ✔

 Moving to final location:
  moving        meu-backstage-app ✔

 Installing dependencies:
  determining   yarn version ✖

Error: Command failed: yarn --version
/bin/sh: 1: yarn: not found


It seems that something went wrong when creating the app 🤔

🔥  Failed to create app!

npm notice
npm notice New major version of npm available! 8.15.0 -> 9.7.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v9.7.2
npm notice Run npm install -g npm@9.7.2 to update!
npm notice
fernando@debian10x64:~/cursos/idp-devportal/backstage/app$

~~~~



- Falta o yarn instalado

- Instalando o Yarn:

https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable
<https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable>

~~~~bash

root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app# npm install --global yarn

added 1 package, and audited 2 packages in 1s

found 0 vulnerabilities
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app#


root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app# yarn --version
1.22.19
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app#
~~~~



- Instalando app novamente:
npx @backstage/create-app@latest

- Novo erro


root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app# npx @backstage/create-app@latest
Need to install the following packages:
  @backstage/create-app@0.5.2
Ok to proceed? (y) y
/tmp/npx-9577c983.sh: 1: /tmp/npx-9577c983.sh: backstage-create-app: Permission denied
npm notice
npm notice New major version of npm available! 8.15.0 -> 9.7.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v9.7.2
npm notice Run npm install -g npm@9.7.2 to update!
npm notice


root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app# sudo npx @backstage/create-app@latest
/tmp/npx-7f079d91.sh: 1: /tmp/npx-7f079d91.sh: backstage-create-app: Permission denied
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app#




error /tmp/npx- .sh: backstage-create-app: Permission denied



https://www.appsloveworld.com/reactjs/100/11/ubuntu-create-react-app-fails-with-permission-denied
<https://www.appsloveworld.com/reactjs/100/11/ubuntu-create-react-app-fails-with-permission-denied>

Please execute few commands.

    Completely uninstall nodejs, npm

     sudo apt-get remove nodejs
     sudo apt-get remove npm

    Check for any .npm or .node folder and delete those.

     which nodejs
     which node

    Install Node.js

     sudo apt update
     sudo apt install nodejs

    The nodejs package contains the nodejs binary as well as npm, so you don’t need to install npm separately.

    Check nodejs and npm version.

     nodejs --version #v12.22.6
     npm --version    #6.14.15

    Create react-app.

     npm init
     npx create-react-app "your-app-name"




- Instalando app novamente:
npx @backstage/create-app@latest


root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app# ls
package.json
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app# npx @backstage/create-app@latest
sh: 1: backstage-create-app: Permission denied
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app# ls
package.json
root@debian10x64:/home/fernando/cursos/idp-devportal/backstage/app#



- Saí do usuário root.
- Executando instalação com usuário fernando(obs: agora o yarn tá instalado no Debian).
 npx @backstage/create-app@latest

- Instalação em andamento:

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/app$ npx @backstage/create-app@latest
? Enter a name for the app [required] meu-backstage-app

Creating the app...

 Checking if the directory is available:
  checking      meu-backstage-app ✔

 Creating a temporary app directory:
  creating      temporary directory ✔

 Preparing files:
  copying       .dockerignore ✔
  templating    .eslintrc.js.hbs ✔

[...RESTANTE OMITIDO...]

 Moving to final location:
  moving        meu-backstage-app ✔

 Installing dependencies:
  determining   yarn version ✔
  executing     yarn install ◜

fernando@debian10x64:~/cursos/idp-devportal/backstage$ ls app/meu-backstage-app/
app-config.local.yaml       app-config.yaml  catalog-info.yaml  lerna.json    package.json  plugins    tsconfig.json
app-config.production.yaml  backstage.json   examples           node_modules  packages      README.md  yarn.lock
fernando@debian10x64:~/cursos/idp-devportal/backstage$
~~~~



- Não incrementou o tamanho durante horas:

~~~~bash

4.0K -rw-r--r--    1 fernando fernando   27 Jul  1 21:04 yarn.lock
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
1.3G    app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$ date
Sat 01 Jul 2023 09:36:32 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
1.3G    app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
1.3G    app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
1.3G    app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$ date
Sat 01 Jul 2023 09:45:36 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
1.3G    app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$ date
Sat 01 Jul 2023 10:02:07 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
1.3G    app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$ date
Sat 01 Jul 2023 10:25:22 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
1.3G    app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$ date
Sat 01 Jul 2023 10:37:59 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
1.3G    app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
1.3G    app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ du -sh app/meu-backstage-app/
da1.3G  app/meu-backstage-app/
fernando@debian10x64:~/cursos/idp-devportal/backstage$ date
Sat 01 Jul 2023 10:53:29 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage$

~~~~




- Ficou eterno no "yarn install"

- Issue abaixo trata de um problema semelhante/igual
https://github.com/backstage/backstage/issues/18058
<https://github.com/backstage/backstage/issues/18058>




# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- Montar docker-compose com NodeJS semelhante a versão usada no doc sobre k8s da Backstage.
- Buildar o APP do Backstage com estrutura via Docker-compose.
- TSHOOT, erro do yarn install travado durante criação do APP do Backstage via npx.
        https://backstage.io/docs/getting-started/create-an-app/
        issue:
        https://github.com/backstage/backstage/issues/18058
        Analisar:
        https://lightrun.com/answers/backstage-backstage-npx-backstagecreate-app-node-is-incompatible-with-this-module
- Buildar imagem Docker, após APP ficar OK.
- Personalizar "app-config.yaml"








# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 02/07/2023

- Docker-hub
imagem
node:16-bullseye-slim
https://hub.docker.com/layers/library/node/16-bullseye-slim/images/sha256-4126251961c4619854e56de80e352b8962443d4d32d5a206ac2ed82b5592f5d1?context=explore


- GPT

preciso de um exemplo de docker-compose para subir aplicação de exemplo em NodeJS usando imagem Docker "node:16-bullseye-slim"
ChatGPT

Claro! Aqui está um exemplo de um arquivo docker-compose.yml para subir uma aplicação de exemplo em Node.js usando a imagem Docker node:16-bullseye-slim:

~~~~yaml

version: '3'
services:
  app:
    image: node:16-bullseye-slim
    container_name: node_app
    restart: always
    working_dir: /app
    volumes:
      - .:/app
    ports:
      - 3000:3000
    command: npm start
~~~~

Neste exemplo, o serviço chamado app usa a imagem node:16-bullseye-slim como base para o contêiner. Ele é reiniciado sempre que necessário e é configurado para trabalhar no diretório /app dentro do contêiner.

O diretório atual do host (onde você tem o arquivo docker-compose.yml) é montado no diretório /app do contêiner usando a opção volumes, permitindo que você desenvolva a aplicação localmente e tenha as alterações refletidas no contêiner.

O mapeamento de porta 3000:3000 é usado para redirecionar o tráfego da porta 3000 do contêiner para a porta 3000 do host. Isso permite acessar a aplicação através do endereço http://localhost:3000 no host.

Por fim, o comando npm start é executado dentro do contêiner para iniciar a aplicação Node.js.

Para iniciar a aplicação usando o Docker Compose, basta navegar até o diretório onde o arquivo docker-compose.yml está localizado e executar o seguinte comando:

docker-compose up

Isso irá construir a imagem e iniciar o contêiner com base na configuração fornecida. Você poderá ver os logs da aplicação em execução no terminal.
User
qual conteúdo preciso ter no  diretório atual do host, para que a aplicação suba? Em relação a arquivos js e etc
ChatGPT

No diretório atual do host, você precisa ter os arquivos da sua aplicação Node.js, incluindo o arquivo package.json. Esses arquivos serão montados no contêiner e a aplicação será executada com base nas dependências e configurações definidas no package.json.

Certifique-se de que o diretório atual contenha os seguintes arquivos e pastas necessários para a execução da aplicação:

    package.json: Este arquivo contém as informações sobre as dependências do seu projeto Node.js, bem como scripts de inicialização, testes e outros. Certifique-se de ter um arquivo package.json válido no diretório atual.

    package-lock.json (opcional): Se você estiver usando o npm para gerenciar as dependências, o package-lock.json pode ser útil para garantir a reprodução exata das versões das dependências instaladas. Embora seja opcional, é recomendado ter o package-lock.json no diretório.

    Arquivos JavaScript da aplicação: Seus arquivos JavaScript, incluindo o arquivo principal da aplicação (por exemplo, index.js ou app.js) e quaisquer outros módulos ou arquivos relacionados, devem estar presentes no diretório atual.

    Outros arquivos estáticos (opcional): Se sua aplicação Node.js depende de outros arquivos estáticos, como arquivos de estilo CSS, arquivos de modelo HTML, imagens ou qualquer outro recurso, certifique-se de tê-los no diretório atual. Eles serão copiados para o contêiner juntamente com os arquivos JavaScript.

Certifique-se de que todos os arquivos e pastas necessários para a execução da aplicação estejam no diretório atual antes de executar o comando docker-compose up. O Docker Compose irá montar o diretório atual no contêiner e iniciar a aplicação com base nos arquivos e configurações fornecidos.
User
preciso de um package.json e um index.js de exemplo
ChatGPT

Claro! Aqui está um exemplo simples de package.json e index.js para uma aplicação Node.js:

package.json:

~~~~json

{
  "name": "exemplo-nodejs",
  "version": "1.0.0",
  "description": "Exemplo de aplicação Node.js",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "author": "Seu Nome",
  "license": "MIT",
  "dependencies": {
    "express": "^4.17.1"
  }
}
~~~~


index.js:

~~~~javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Olá, mundo!');
});

app.listen(port, () => {
  console.log(`Aplicação está executando em http://localhost:${port}`);
});
~~~~

Neste exemplo, o arquivo package.json especifica o nome da aplicação, a versão, uma descrição, o arquivo principal index.js e um script de inicialização chamado start, que executa o comando node index.js. A dependência express também é listada.

O arquivo index.js é um exemplo básico de um servidor Express que responde com "Olá, mundo!" quando você acessa o endereço http://localhost:3000/.

Certifique-se de criar um diretório no seu sistema de arquivos, adicionar esses arquivos (package.json e index.js) ao diretório e navegar até ele no terminal antes de executar docker-compose up. Dessa forma, o Docker Compose será capaz de montar o diretório atual no contêiner e executar a aplicação Node.js.



- Ajustado o diretório local também
/home/fernando/cursos/idp-devportal/backstage/docker/docker-compose/docker-compose.yml

- Testando
docker-compose up

- ERRO:
~~~~BASH

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ ls
app  docker-compose.yml
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker-compose up
Creating network "docker-compose_default" with the default driver
Creating node_app ... done
Attaching to node_app
node_app | 
node_app | > exemplo-nodejs@1.0.0 start
node_app | > node index.js
node_app | 
node_app | node:internal/modules/cjs/loader:1031
node_app |   throw err;
node_app |   ^
node_app | 
node_app | Error: Cannot find module 'express'
node_app | Require stack:
node_app | - /app/index.js
node_app |     at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1028:15)
node_app |     at Function.Module._load (node:internal/modules/cjs/loader:873:27)
node_app |     at Module.require (node:internal/modules/cjs/loader:1100:19)
node_app |     at require (node:internal/modules/cjs/helpers:108:18)
node_app |     at Object.<anonymous> (/app/index.js:1:17)
node_app |     at Module._compile (node:internal/modules/cjs/loader:1198:14)
node_app |     at Object.Module._extensions..js (node:internal/modules/cjs/loader:1252:10)
node_app |     at Module.load (node:internal/modules/cjs/loader:1076:32)
node_app |     at Function.Module._load (node:internal/modules/cjs/loader:911:12)
node_app |     at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12) {
node_app |   code: 'MODULE_NOT_FOUND',
node_app |   requireStack: [ '/app/index.js' ]
node_app | }
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
~~~~




- GPT

Como fazer um Dockerfile que instale o express e como ajustar o docker-compose para usar este dockerfile como base?
ChatGPT

Claro! Aqui está um exemplo de como você pode criar um Dockerfile para instalar o Express e como ajustar o docker-compose para usar esse Dockerfile como base:

    Crie um arquivo chamado Dockerfile (sem extensão) no diretório raiz do seu projeto.

    Abra o arquivo Dockerfile e adicione o seguinte conteúdo:

~~~~Dockerfile
# Use a imagem base do Node.js
FROM node:16-bullseye-slim

# Define o diretório de trabalho no contêiner
WORKDIR /app

# Copia o arquivo package.json e package-lock.json para o diretório de trabalho
COPY package*.json ./

# Instala as dependências do Node.js
RUN npm install

# Copia todos os arquivos do projeto para o diretório de trabalho
COPY . .

# Expõe a porta em que a aplicação está escutando
EXPOSE 3000

# Define o comando para iniciar a aplicação
CMD [ "npm", "start" ]
~~~~

Neste exemplo, o Dockerfile usa a imagem base do node:16-bullseye-slim. O diretório de trabalho no contêiner é definido como /app. Os arquivos package.json e package-lock.json são copiados para o diretório de trabalho e as dependências do Node.js são instaladas usando o comando RUN npm install. Em seguida, todos os arquivos do projeto são copiados para o diretório de trabalho. A porta 3000 é exposta para permitir o acesso à aplicação. Por fim, o comando npm start é usado para iniciar a aplicação.

    Agora, você pode ajustar o docker-compose.yml para usar o Dockerfile criado:

~~~~yaml
version: '3'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: node_app
    restart: always
    ports:
      - 3000:3000
~~~~

Nesse exemplo, a seção build é adicionada ao serviço app do docker-compose.yml. Ela define o contexto como o diretório atual (.) e o arquivo Dockerfile como Dockerfile. O Docker Compose usará essas configurações para construir a imagem do contêiner com base no Dockerfile.

Certifique-se de estar no diretório raiz do projeto (onde o Dockerfile e o docker-compose.yml estão localizados) e execute o comando docker-compose up para iniciar o contêiner usando o Dockerfile personalizado. Isso criará uma imagem Docker com o Express instalado e executará a aplicação Node.js dentro do contêiner.



- Ajustando apenas a pasta onde tá o json:
COPY /app/package*.json ./


- Testando
docker-compose up

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker-compose up
Building app
Sending build context to Docker daemon  5.632kB
Step 1/7 : FROM node:16-bullseye-slim
 ---> ac00507796e5
Step 2/7 : WORKDIR /app
 ---> Using cache
 ---> c5fe2fbc10ee
Step 3/7 : COPY /app/package*.json ./
 ---> aff79430da77
Step 4/7 : RUN npm install
 ---> Running in 83e7fab7f7ac

added 58 packages, and audited 59 packages in 5s

8 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice 
npm notice New major version of npm available! 8.19.4 -> 9.7.2
npm notice Changelog: <https://github.com/npm/cli/releases/tag/v9.7.2>
npm notice Run `npm install -g npm@9.7.2` to update!
npm notice 
Removing intermediate container 83e7fab7f7ac
 ---> 5170cae34f17
Step 5/7 : COPY . .
 ---> ece64ae2ff24
Step 6/7 : EXPOSE 3000
 ---> Running in fb02884e56f7
Removing intermediate container fb02884e56f7
 ---> 2d477b13c2c8
Step 7/7 : CMD [ "npm", "start" ]
 ---> Running in be68da3b23fd
Removing intermediate container be68da3b23fd
 ---> ebbfacfb6308
Successfully built ebbfacfb6308
Successfully tagged docker-compose_app:latest
WARNING: Image for service app was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Recreating node_app ... done
Attaching to node_app
node_app | 
node_app | > exemplo-nodejs@1.0.0 start
node_app | > node index.js
node_app | 
node_app | node:internal/modules/cjs/loader:1031
node_app |   throw err;
node_app |   ^
node_app | 
node_app | Error: Cannot find module '/app/index.js'
node_app |     at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1028:15)
node_app |     at Function.Module._load (node:internal/modules/cjs/loader:873:27)
node_app |     at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
node_app |     at node:internal/main/run_main_module:22:47 {
node_app |   code: 'MODULE_NOT_FOUND',
node_app |   requireStack: []
node_app | }
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
~~~~





Error: Cannot find module '/app/index.js'
ChatGPT

O erro "Cannot find module '/app/index.js'" ocorre quando o arquivo index.js não está presente no diretório /app dentro do contêiner.

Para resolver esse problema, certifique-se de que o arquivo index.js esteja localizado corretamente no diretório do seu projeto no host, que deve ser mapeado para o diretório /app no contêiner.

Verifique se o arquivo index.js está no diretório correto em seu ambiente local e, em seguida, ajuste o docker-compose.yml para mapear o diretório adequado.

Aqui está um exemplo de como ajustar o docker-compose.yml para mapear corretamente o diretório com o arquivo index.js:

~~~~yaml

version: '3'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: node_app
    restart: always
    ports:
      - 3000:3000
    volumes:
      - ./caminho/para/o/seu/diretorio:/app
~~~~

No exemplo acima, substitua ./caminho/para/o/seu/diretorio pelo caminho correto do diretório que contém o arquivo index.js em seu ambiente local. Isso fará com que o diretório local seja mapeado para o diretório /app no contêiner, garantindo que o arquivo index.js esteja acessível dentro do contêiner.

Certifique-se de ajustar corretamente o caminho para o diretório que contém o arquivo index.js em seu ambiente local. Depois de fazer a modificação no docker-compose.yml, execute o docker-compose up novamente para iniciar o contêiner com o diretório e o arquivo index.js corretamente mapeados. Isso deve resolver o erro "Cannot find module '/app/index.js'".



- Ajustando.
/home/fernando/cursos/idp-devportal/backstage/docker/docker-compose/docker-compose.yml
- Adicionado:
    volumes:
      - ./app:/app

- ERRO

~~~~BASH

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker-compose up --build
Building app
Sending build context to Docker daemon  5.632kB
Step 1/7 : FROM node:16-bullseye-slim
 ---> ac00507796e5
Step 2/7 : WORKDIR /app
 ---> Using cache
 ---> c5fe2fbc10ee
Step 3/7 : COPY /app/package*.json ./
 ---> Using cache
 ---> aff79430da77
Step 4/7 : RUN npm install
 ---> Using cache
 ---> 5170cae34f17
Step 5/7 : COPY . .
 ---> 6bb096ec5e93
Step 6/7 : EXPOSE 3000
 ---> Running in d31049114a53
Removing intermediate container d31049114a53
 ---> d1a2b4a7cfd3
Step 7/7 : CMD [ "npm", "start" ]
 ---> Running in 6cb9a5167eed
Removing intermediate container 6cb9a5167eed
 ---> 0ed2058b33de
Successfully built 0ed2058b33de
Successfully tagged docker-compose_app:latest
Recreating node_app ... done
Attaching to node_app
node_app | 
node_app | > exemplo-nodejs@1.0.0 start
node_app | > node index.js
node_app | 
node_app | node:internal/modules/cjs/loader:1031
node_app |   throw err;
node_app |   ^
node_app | 
node_app | Error: Cannot find module 'express'
node_app | Require stack:
node_app | - /app/index.js
node_app |     at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1028:15)
node_app |     at Function.Module._load (node:internal/modules/cjs/loader:873:27)
node_app |     at Module.require (node:internal/modules/cjs/loader:1100:19)
node_app |     at require (node:internal/modules/cjs/helpers:108:18)
node_app |     at Object.<anonymous> (/app/index.js:1:17)
node_app |     at Module._compile (node:internal/modules/cjs/loader:1198:14)
node_app |     at Object.Module._extensions..js (node:internal/modules/cjs/loader:1252:10)
node_app |     at Module.load (node:internal/modules/cjs/loader:1076:32)
node_app |     at Function.Module._load (node:internal/modules/cjs/loader:911:12)
node_app |     at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12) {
node_app |   code: 'MODULE_NOT_FOUND',
node_app |   requireStack: [ '/app/index.js' ]
node_app | }
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
~~~~





- Ajustando
COPY ./app/package*.json ./

# Instala o Express globalmente
RUN npm install -g express

- Testando





- Ajustando

adicionando
RUN mkdir -p /app

DE:
COPY ./app/package*.json ./
PARA:
COPY ./app/package*.json ./app



- ERRO

~~~~BASH
---> Running in 7b01e830c844
npm ERR! code ENOENT
npm ERR! syscall open
npm ERR! path /app/package.json
npm ERR! errno -2
npm ERR! enoent ENOENT: no such file or directory, open '/app/package.json'
npm ERR! enoent This is related to npm not being able to find a file.
npm ERR! enoent 

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2023-07-02T17_34_30_302Z-debug-0.log
~~~~








- Ajustando.

- Docker-compose
  # Anonymous volume:
  - /usr/src/app/node_modules

- Dockerfile
COPY ./app ./app


docker compose build --no-cache
docker compose up -d


- ERRO

~~~~BASH
npm ERR! code ENOENT
npm ERR! syscall open
npm ERR! path /app/package.json
npm ERR! errno -2
npm ERR! enoent ENOENT: no such file or directory, open '/app/package.json'
npm ERR! enoent This is related to npm not being able to find a file.
npm ERR! enoent 

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2023-07-02T17_40_13_508Z-debug-0.log
The command '/bin/sh -c npm install' returned a non-zero code: 254
ERROR: Service 'app' failed to build : Build failed
~~~~


- Criando dockerignore
/home/fernando/cursos/idp-devportal/backstage/docker/docker-compose/.dockerignore




# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- Idéia é subir um app em NodeJS de exemplo, para pegar a estrutura. Depois, fazer uma imagem/Dockerfile/Ou-via-commit, um build do Backstage usando o NodeJS do Container ou do Build via Dockerfile.
- Subir docker-compose com aplicação NodeJS de exemplo.
      Seguir tutorial:
      https://www.digitalocean.com/community/tutorials/containerizing-a-node-js-application-for-development-with-docker-compose
- Montar docker-compose com NodeJS semelhante a versão usada no doc sobre k8s da Backstage.
- Buildar o APP do Backstage com estrutura via Docker-compose.
- TSHOOT, erro do yarn install travado durante criação do APP do Backstage via npx.
        https://backstage.io/docs/getting-started/create-an-app/
        issue:
        https://github.com/backstage/backstage/issues/18058
        Analisar:
        https://lightrun.com/answers/backstage-backstage-npx-backstagecreate-app-node-is-incompatible-with-this-module
- Buildar imagem Docker, após APP ficar OK.
- Personalizar "app-config.yaml"










# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 15/07/2023

- Idéia é subir um app em NodeJS de exemplo, para pegar a estrutura. Depois, fazer uma imagem/Dockerfile/Ou-via-commit, um build do Backstage usando o NodeJS do Container ou do Build via Dockerfile.

- Testando
docker-compose build --no-cache

~~~~bash
ge.
    -q, --quiet             Don't print anything to STDOUT
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker-compose build --no-cache
Building app-teste-fusionist
Sending build context to Docker daemon  6.144kB
Step 1/9 : FROM node:16-bullseye-slim
 ---> ac00507796e5
Step 2/9 : RUN mkdir -p /app
 ---> Running in 881edb6b8d9b
Removing intermediate container 881edb6b8d9b
 ---> bd732f72fbe2
Step 3/9 : WORKDIR /app
 ---> Running in 656f88f9c1e1
Removing intermediate container 656f88f9c1e1
 ---> a3343ab627d3
Step 4/9 : COPY ./app/package*.json ./app
 ---> 5be9623ed79f
Step 5/9 : RUN npm install
 ---> Running in 200372c67c06
npm ERR! code ENOENT
npm ERR! syscall open
npm ERR! path /app/package.json
npm ERR! errno -2
npm ERR! enoent ENOENT: no such file or directory, open '/app/package.json'
npm ERR! enoent This is related to npm not being able to find a file.
npm ERR! enoent 

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2023-07-15T22_09_23_409Z-debug-0.log
The command '/bin/sh -c npm install' returned a non-zero code: 254
ERROR: Service 'app-teste-fusionist' failed to build : Build failed
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
~~~~



- Ajustando estrutura de pastas e Dockerfile.

- Alterando Dockerfile

DE:

~~~~DOCKERFILE
# Use a imagem base do Node.js
#FROM node:16-bullseye-slim
# Create app directory
RUN mkdir -p /app

# Define o diretório de trabalho no contêiner
WORKDIR /app

# Copia o arquivo package.json e package-lock.json para o diretório de trabalho
COPY ./app/package*.json ./app

# Instala as dependências do Node.js
RUN npm install

# Instala o Express globalmente
RUN npm install -g express

# Copia todos os arquivos do projeto para o diretório de trabalho
COPY ./app ./app

# Expõe a porta em que a aplicação está escutando
EXPOSE 3000

# Define o comando para iniciar a aplicação
CMD [ "npm", "start" ]

~~~~



PARA:

~~~~DOCKERFILE
FROM node:14.15.4
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
~~~~



- Novo docker-compose.
- Ajustando o context do build e o caminho do Dockerfile:

~~~~YAML
version: '3'
services:
  app-teste-fusionist:
    build:
      context: ./app
      dockerfile: ./Dockerfile
    container_name: node_app_teste
    restart: always
    ports:
      - 3000:3000
    volumes:
      - ./app:/app
      # Anonymous volume:
      - /app/node_modules
~~~~



- Agora o build passou!
- Resultado:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker-compose build --no-cache
Building app-teste-fusionist
Sending build context to Docker daemon   5.12kB
Step 1/7 : FROM node:14.15.4
 ---> 924763541c0c
Step 2/7 : WORKDIR /app
 ---> Running in abb964c05907
Removing intermediate container abb964c05907
 ---> eabca08827b6
Step 3/7 : COPY package*.json ./
 ---> bd2e23961f27
Step 4/7 : RUN npm install
 ---> Running in 8e3e9c67b73f
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN exemplo-nodejs@1.0.0 No repository field.

added 58 packages from 42 contributors and audited 58 packages in 3.424s

8 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

Removing intermediate container 8e3e9c67b73f
 ---> 9fc7224fab4c
Step 5/7 : COPY . .
 ---> 6871ada29dc8
Step 6/7 : EXPOSE 3000
 ---> Running in cff6ac8cc166
Removing intermediate container cff6ac8cc166
 ---> 146378684a63
Step 7/7 : CMD ["node", "app.js"]
 ---> Running in 95918bb09875
Removing intermediate container 95918bb09875
 ---> 99bdd7cc751e
Successfully built 99bdd7cc751e
Successfully tagged docker-compose_app-teste-fusionist:latest
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
~~~~




- Imagem ficou pesada:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker image ls
REPOSITORY                                       TAG                IMAGE ID       CREATED          SIZE
docker-compose_app-teste-fusionist               latest             99bdd7cc751e   37 seconds ago   947MB

~~~~



- Testando imagem slim:

FROM node:16-bullseye-slim


~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker-compose build --no-cache
Building app-teste-fusionist
Sending build context to Docker daemon   5.12kB
Step 1/7 : FROM node:16-bullseye-slim
 ---> ac00507796e5
Step 2/7 : WORKDIR /app
 ---> Running in b76572123ebe
Removing intermediate container b76572123ebe
 ---> ce8bd527874a
Step 3/7 : COPY package*.json ./
 ---> 97c693f949d3
Step 4/7 : RUN npm install
 ---> Running in 6c185572e623

added 58 packages, and audited 59 packages in 5s

8 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice 
npm notice New major version of npm available! 8.19.4 -> 9.8.0
npm notice Changelog: <https://github.com/npm/cli/releases/tag/v9.8.0>
npm notice Run `npm install -g npm@9.8.0` to update!
npm notice 
Removing intermediate container 6c185572e623
 ---> 28a2d69e5ac1
Step 5/7 : COPY . .
 ---> 119f580b5dfe
Step 6/7 : EXPOSE 3000
 ---> Running in caa9384785ca
Removing intermediate container caa9384785ca
 ---> fe4e219df339
Step 7/7 : CMD ["node", "app.js"]
 ---> Running in 4ff610647d63
Removing intermediate container 4ff610647d63
 ---> af651a0e28c3
Successfully built af651a0e28c3
Successfully tagged docker-compose_app-teste-fusionist:latest
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
~~~~



- Só este ajuste deixou a imagem 10x mais leve:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker image ls | head
REPOSITORY                                       TAG                IMAGE ID       CREATED          SIZE
docker-compose_app-teste-fusionist               latest             af651a0e28c3   13 seconds ago   197MB

~~~~





- Subindo container

- ERROS

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED          STATUS                         PORTS     NAMES
d6a42b163c44   docker-compose_app-teste-fusionist   "docker-entrypoint.s…"   40 seconds ago   Restarting (1) 5 seconds ago             node_app_teste
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
~~~~



- Logs

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker logs node_app_teste 
node:internal/modules/cjs/loader:1031
  throw err;
  ^

Error: Cannot find module '/app/app.js'
    at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1028:15)
    at Function.Module._load (node:internal/modules/cjs/loader:873:27)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
    at node:internal/main/run_main_module:22:47 {
  code: 'MODULE_NOT_FOUND',
  requireStack: []
}
node:internal/modules/cjs/loader:1031
  throw err;
  ^

Error: Cannot find module '/app/app.js'
    at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1028:15)
    at Function.Module._load (node:internal/modules/cjs/loader:873:27)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
    at node:internal/main/run_main_module:22:47 {
  code: 'MODULE_NOT_FOUND',
  requireStack: []
}
node:internal/modules/cjs/loader:1031
  throw err;
  ^

Error: Cannot find module '/app/app.js'
    at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1028:15)
    at Function.Module._load (node:internal/modules/cjs/loader:873:27)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
    at node:internal/main/run_main_module:22:47 {
  code: 'MODULE_NOT_FOUND',
  requireStack: []
}
node:internal/modules/cjs/loader:1031
  throw err;
  ^

Error: Cannot find module '/app/app.js'
    at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1028:15)
    at Function.Module._load (node:internal/modules/cjs/loader:873:27)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
    at node:internal/main/run_main_module:22:47 {
  code: 'MODULE_NOT_FOUND',
  requireStack: []
}
node:internal/modules/cjs/loader:1031
  throw err;
  ^

Error: Cannot find module '/app/app.js'
    at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1028:15)
    at Function.Module._load (node:internal/modules/cjs/loader:873:27)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
    at node:internal/main/run_main_module:22:47 {
  code: 'MODULE_NOT_FOUND',
  requireStack: []
}
node:internal/modules/cjs/loader:1031
  throw err;
  ^

Error: Cannot find module '/app/app.js'
    at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1028:15)
    at Function.Module._load (node:internal/modules/cjs/loader:873:27)
~~~~




- Ajustando Dockerfile
apontando para index.js ao invés de app.js
workdir de /app para / apenas

- Testando
  510  docker-compose build --no-cache
  511  docker-compose up -d


- Buildando:

docker-compose build --no-cache

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker-compose build --no-cache
Building app-teste-fusionist
Sending build context to Docker daemon   5.12kB
Step 1/7 : FROM node:16-bullseye-slim
 ---> ac00507796e5
Step 2/7 : WORKDIR /
 ---> Running in 8418f8a6224c
Removing intermediate container 8418f8a6224c
 ---> 2be5f0e36eed
Step 3/7 : COPY package*.json ./
 ---> c0e0e7ef1e1e
Step 4/7 : RUN npm install
 ---> Running in f2aff9e90603

added 58 packages, and audited 59 packages in 5s

8 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice 
npm notice New major version of npm available! 8.19.4 -> 9.8.0
npm notice Changelog: <https://github.com/npm/cli/releases/tag/v9.8.0>
npm notice Run `npm install -g npm@9.8.0` to update!
npm notice 
Removing intermediate container f2aff9e90603
 ---> c0b6772f5f62
Step 5/7 : COPY . .
 ---> eda0032c70b8
Step 6/7 : EXPOSE 3000
 ---> Running in c8fab4439aa0
Removing intermediate container c8fab4439aa0
 ---> a16ee8fdab36
Step 7/7 : CMD ["node", "index.js"]
 ---> Running in 696c47ff186f
Removing intermediate container 696c47ff186f
 ---> af2a7918abb4
Successfully built af2a7918abb4
Successfully tagged docker-compose_app-teste-fusionist:latest
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 

~~~~



- Subindo app:

docker-compose up -d

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                         PORTS     NAMES
d6a42b163c44   af651a0e28c3   "docker-entrypoint.s…"   8 minutes ago   Restarting (1) 3 seconds ago             node_app_teste
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker rm node_app_teste
Error response from daemon: You cannot remove a restarting container d6a42b163c4437646d1e4997463251327223f71e6f3593c06c796215db608d28. Stop the container before attempting removal or force remove
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker rm -f node_app_teste
node_app_teste
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker-compose up -d
Creating node_app_teste ... done
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED         STATUS        PORTS                                       NAMES
149d2b769492   docker-compose_app-teste-fusionist   "docker-entrypoint.s…"   3 seconds ago   Up 1 second   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   node_app_teste
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED         STATUS         PORTS                                       NAMES
149d2b769492   docker-compose_app-teste-fusionist   "docker-entrypoint.s…"   6 seconds ago   Up 4 seconds   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   node_app_teste
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ date
Sat 15 Jul 2023 07:46:49 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ 

~~~~



- Validando

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker image ls | head
REPOSITORY                                       TAG                IMAGE ID       CREATED              SIZE
docker-compose_app-teste-fusionist               latest             af2a7918abb4   About a minute ago   197MB
<none>                                           <none>             af651a0e28c3   22 minutes ago       197MB
<none>                                           <none>             99bdd7cc751e   25 minutes ago       947MB
<none>                                           <none>             5be9623ed79f   38 minutes ago       191MB
<none>                                           <none>             8bfd83bc1c3b   13 days ago          191MB
docker-compose_app                               latest             719552da5403   13 days ago          199MB
<none>                                           <none>             0ed2058b33de   13 days ago          197MB
<none>                                           <none>             ebbfacfb6308   13 days ago          197MB
node                                             16-bullseye-slim   ac00507796e5   3 weeks ago          191MB
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ date
Sat 15 Jul 2023 07:47:23 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ cat | curl http://192.168.0.110:3000/
Olá, mundo!
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ date
Sat 15 Jul 2023 07:47:38 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
~~~~



- Abriu o endereço
<http://192.168.0.110:3000/>

Olá, mundo!












# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- Montar docker-compose com NodeJS semelhante a versão usada no doc sobre k8s da Backstage. Instalar o app do Backstage.
- Caso necessário, usar repo "kubedev" como apoio, sobre Dockerfile, NodeJS, etc.
- Buildar o APP do Backstage com estrutura via Docker-compose.
- TSHOOT, erro do yarn install travado durante criação do APP do Backstage via npx.
        https://backstage.io/docs/getting-started/create-an-app/
        issue:
        https://github.com/backstage/backstage/issues/18058
        Analisar:
        https://lightrun.com/answers/backstage-backstage-npx-backstagecreate-app-node-is-incompatible-with-this-module
- Buildar imagem Docker, após APP ficar OK.
- Personalizar "app-config.yaml"








- RETOMANDO INSTALAÇÃO DO APP

https://backstage.io/docs/deployment/docker/
<https://backstage.io/docs/deployment/docker/>

This section assumes that an app has already been created with @backstage/create-app, in which the frontend is bundled and served from the backend. This is done using the @backstage/plugin-app-backend plugin, which also injects the frontend configuration into the app. This means that you only need to build and deploy a single container in a minimal setup of Backstage. If you wish to separate the serving of the frontend out from the backend, see the separate frontend topic below.

Conforme a mensagem acima, é necessário criar o app antes, seguindo as instruções da página abaixo:
https://backstage.io/docs/getting-started/create-an-app/
<https://backstage.io/docs/getting-started/create-an-app/>


## INSTALANDO APP - BACKSTAGE

- Instalando app:
npx @backstage/create-app@latest





- 

~~~~BASH
fernando@debian10x64:~$ docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED          STATUS          PORTS                                       NAMES
149d2b769492   docker-compose_app-teste-fusionist   "docker-entrypoint.s…"   17 minutes ago   Up 17 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   node_app_teste
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ docker container exec -ti node_app_teste bash
root@149d2b769492:/#
root@149d2b769492:/#
root@149d2b769492:/# ls
app  bin  boot  dev  etc  home  index.js  lib  lib64  media  mnt  node_modules  opt  package-lock.json  package.json  proc  root  run  sbin  srv  sys  tmp  usr  var
root@149d2b769492:/# ls app/
Dockerfile  index.js  node_modules  package.json
root@149d2b769492:/# npx @backstage/create-app@latest
Need to install the following packages:
  @backstage/create-app@0.5.2
Ok to proceed? (y) y
? Enter a name for the app [required] meu-backstage-teste

Creating the app...

 Checking if the directory is available:
  checking      meu-backstage-teste ✔

 Creating a temporary app directory:
  creating      temporary directory ✔

 Preparing files:
  copying       .dockerignore ✔
  templating    .eslintrc.js.hbs ✔
  templating    .gitignore.hbs ✔
  copying       .prettierignore ✔
  copying       README.md ✔
  copying       app-config.local.yaml ✔
  copying       app-config.production.yaml ✔
  templating    app-config.yaml.hbs ✔
  templating    backstage.json.hbs ✔
  templating    catalog-info.yaml.hbs ✔
  copying       lerna.json ✔
  templating    package.json.hbs ✔
  copying       tsconfig.json ✔
  copying       yarn.lock ✔
  copying       README.md ✔
  copying       entities.yaml ✔
  copying       org.yaml ✔
  copying       template.yaml ✔
  copying       catalog-info.yaml ✔
  copying       index.js ✔
  copying       package.json ✔
  copying       README.md ✔
  templating    .eslintrc.js.hbs ✔
  copying       Dockerfile ✔
  copying       README.md ✔
  templating    package.json.hbs ✔
  copying       index.test.ts ✔
  copying       types.ts ✔
  copying       index.ts ✔
  copying       app.ts ✔
  copying       auth.ts ✔
  copying       catalog.ts ✔
  copying       proxy.ts ✔
  copying       scaffolder.ts ✔
  copying       techdocs.ts ✔
  templating    search.ts.hbs ✔
  copying       .eslintignore ✔
  templating    .eslintrc.js.hbs ✔
  copying       cypress.json ✔
  templating    package.json.hbs ✔
  copying       android-chrome-192x192.png ✔
  copying       favicon-16x16.png ✔
  copying       apple-touch-icon.png ✔
  copying       favicon-32x32.png ✔
  copying       index.html ✔
  copying       favicon.ico ✔
  copying       manifest.json ✔
  copying       safari-pinned-tab.svg ✔
  copying       robots.txt ✔
  copying       .eslintrc.json ✔
  copying       app.js ✔
  copying       App.test.tsx ✔
  copying       App.tsx ✔
  copying       apis.ts ✔
  copying       index.tsx ✔
  copying       setupTests.ts ✔
  copying       LogoFull.tsx ✔
  copying       LogoIcon.tsx ✔
  copying       Root.tsx ✔
  copying       index.ts ✔
  copying       EntityPage.tsx ✔
  copying       SearchPage.tsx ✔

 Moving to final location:
  moving        meu-backstage-teste ✔

 Installing dependencies:
  determining   yarn version ✔
  executing     yarn install ◜ warning @backstage/cli > fork-ts-checker-webpack-plugin > memfs@3.6.0: this will be v4
warning @backstage/cli > react-dev-utils > fork-ts-checker-webpack-plugin > memfs@3.6.0: this will be v4
warning @backstage/cli > webpack-dev-server > webpack-dev-middleware > memfs@3.6.0: this will be v4
warning @backstage/cli > rollup-plugin-dts > magic-string > sourcemap-codec@1.4.8: Please use @jridgewell/sourcemap-codec instead
warning @backstage/cli > @svgr/plugin-svgo > svgo > stable@0.1.8: Modern JS already guarantees Array#sort() is a stable sort, so this library is deprecated. See the compatibility table on MDN: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort#browser_compatibility
warning lerna > @lerna/bootstrap > read-package-tree@5.3.1: The functionality that this package provided is now in @npmcli/arborist
warning lerna > @lerna/version > temp-write > uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See https://v8.dev/blog/math-random for details.
warning lerna > @lerna/bootstrap > read-package-tree > readdir-scoped-modules@1.1.0: This functionality has been moved to @npmcli/fs
warning lerna > @lerna/bootstrap > read-package-tree > readdir-scoped-modules > debuglog@1.0.1: Package no longer supported. Contact Support at https://www.npmjs.com/support for more info.

warning "workspace-aggregator-53f68cd8-4af1-43f6-80c8-bc401f29160f > app > @backstage/plugin-scaffolder > @uiw/react-codemirror > codemirror > @codemirror/autocomplete@6.8.1" has unmet peer dependency "@lezer/common@^1.0.0".
warning Workspaces can only be enabled in private projects.
warning Workspaces can only be enabled in private projects.
warning Workspaces can only be enabled in private projects.
error An unexpected error occurred: "ENOSPC: no space left on device, copyfile '/usr/local/share/.cache/yarn/v6/npm-eslint-visitor-keys-3.4.1-c22c48f48942d08ca824cc526211ae400478a994-integrity/node_modules/eslint-visitor-keys/dist/eslint-visitor-keys.d.cts' -> '/meu-backstage-teste/node_modules/eslint-visitor-keys/dist/eslint-visitor-keys.d.cts'".
yarn install v1.22.19
[1/5] Validating package.json...
[2/5] Resolving packages...
[3/5] Fetching packages...
[4/5] Linking dependencies...
info If you think this is a bug, please open a bug report with the information provided in "/meu-backstage-teste/yarn-error.log".
info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
  executing     yarn install ✖

Error: Could not execute command yarn install

It seems that something went wrong when creating the app 🤔

🔥  Failed to create app!

root@149d2b769492:/#

~~~~




- Ocorreu erro, devido espaço em disco.

- Verificando disco

~~~~BASH

fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$ df -h
Filesystem                        Size  Used Avail Use% Mounted on
udev                              4.8G     0  4.8G   0% /dev
tmpfs                             983M   18M  965M   2% /run
/dev/mapper/debian10x64--vg-root   58G   58G     0 100% /
tmpfs                             4.8G     0  4.8G   0% /dev/shm
tmpfs                             5.0M     0  5.0M   0% /run/lock
tmpfs                             4.8G     0  4.8G   0% /sys/fs/cgroup
/dev/loop1                        119M  119M     0 100% /snap/core/15511
/dev/loop3                        228M  228M     0 100% /snap/kontena-lens/223
/dev/loop0                        119M  119M     0 100% /snap/core/15419
/dev/loop2                        117M  117M     0 100% /snap/robo3t-snap/9
/dev/loop4                         56M   56M     0 100% /snap/core18/2785
/dev/loop5                         56M   56M     0 100% /snap/core18/2751
/dev/loop6                        218M  218M     0 100% /snap/kontena-lens/218
/dev/sda1                         472M   59M  389M  14% /boot
tmpfs                             983M   28K  983M   1% /run/user/117
tmpfs                             983M     0  983M   0% /run/user/1000
overlay                            58G   58G     0 100% /var/lib/docker/overlay2/b397a7207e0f6bfde896b31a67479ad6e04e4802bf109bf35aae3e66a3818122/merged
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$ date
Sat 15 Jul 2023 08:46:33 PM -03
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$
~~~~




- Efetuando limpeza
- Removidas imagens Docker obsoletas.

~~~~bash
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$ df -h
Filesystem                        Size  Used Avail Use% Mounted on
udev                              4.8G     0  4.8G   0% /dev
tmpfs                             983M   18M  965M   2% /run
/dev/mapper/debian10x64--vg-root   58G   51G  3.8G  94% /
tmpfs                             4.8G     0  4.8G   0% /dev/shm
tmpfs                             5.0M     0  5.0M   0% /run/lock
tmpfs                             4.8G     0  4.8G   0% /sys/fs/cgroup
/dev/loop1                        119M  119M     0 100% /snap/core/15511
/dev/loop3                        228M  228M     0 100% /snap/kontena-lens/223
/dev/loop0                        119M  119M     0 100% /snap/core/15419
/dev/loop2                        117M  117M     0 100% /snap/robo3t-snap/9
/dev/loop4                         56M   56M     0 100% /snap/core18/2785
/dev/loop5                         56M   56M     0 100% /snap/core18/2751
/dev/loop6                        218M  218M     0 100% /snap/kontena-lens/218
/dev/sda1                         472M   59M  389M  14% /boot
tmpfs                             983M   28K  983M   1% /run/user/117
tmpfs                             983M     0  983M   0% /run/user/1000
overlay                            58G   51G  3.8G  94% /var/lib/docker/overlay2/b397a7207e0f6bfde896b31a67479ad6e04e4802bf109bf35aae3e66a3818122/merged
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$ date
Sat 15 Jul 2023 08:49:28 PM -03
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$

~~~~




- Efetuando limpeza adicional.
- Pastas .terraform

~~~~bash
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$ ncdu -x /
ncdu 1.13 ~ Use the arrow keys to navigate, press ? for help
--- /home/fernando/cursos/terraform/terraform-udemy-cleber/terraform-aws -----------------------------------------------------------------------------------------------------------------
                         /..
    6.7 GiB [##########] /aulas
   12.6 MiB [          ] /.git
    3.7 MiB [          ] /.git-old
    2.1 MiB [          ] /material-do-curso
    4.0 KiB [          ]  README.md
    4.0 KiB [          ]  .gitignore



DEPOIS:


fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$ ncdu -x /
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$ ncdu -x /
ncdu 1.13 ~ Use the arrow keys to navigate, press ? for help
--- /home/fernando/cursos/terraform/terraform-udemy-cleber/terraform-aws -----------------------------------------------------------------------------------------------------------------
                         /..
    2.2 GiB [##########] /aulas
   12.6 MiB [          ] /.git
    3.7 MiB [          ] /.git-old
    2.1 MiB [          ] /material-do-curso
    4.0 KiB [          ]  README.md
    4.0 KiB [          ]  .gitignore


fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$ df -h
Filesystem                        Size  Used Avail Use% Mounted on
udev                              4.8G     0  4.8G   0% /dev
tmpfs                             983M   18M  965M   2% /run
/dev/mapper/debian10x64--vg-root   58G   41G   14G  75% /
tmpfs                             4.8G     0  4.8G   0% /dev/shm
tmpfs                             5.0M     0  5.0M   0% /run/lock
tmpfs                             4.8G     0  4.8G   0% /sys/fs/cgroup
/dev/loop1                        119M  119M     0 100% /snap/core/15511
/dev/loop3                        228M  228M     0 100% /snap/kontena-lens/223
/dev/loop0                        119M  119M     0 100% /snap/core/15419
/dev/loop2                        117M  117M     0 100% /snap/robo3t-snap/9
/dev/loop4                         56M   56M     0 100% /snap/core18/2785
/dev/loop5                         56M   56M     0 100% /snap/core18/2751
/dev/loop6                        218M  218M     0 100% /snap/kontena-lens/218
/dev/sda1                         472M   59M  389M  14% /boot
tmpfs                             983M  3.5M  979M   1% /run/user/1000
overlay                            58G   41G   14G  75% /var/lib/docker/overlay2/b397a7207e0f6bfde896b31a67479ad6e04e4802bf109bf35aae3e66a3818122/merged
/dev/sr0                          336M  336M     0 100% /media/cdrom0
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$ date
Sat 15 Jul 2023 09:05:36 PM -03
fernando@debian10x64:~/cursos/boilerplates/nodejs-boilerplates$

~~~~







- Novo teste de instalação do app, iniciado as 21:08h:

chPage.tsx ✔

 Moving to final location:
  moving        meu-backstage-teste ✔

 Installing dependencies:
  determining   yarn version ✔
  executing     yarn install ◞





- NOVO ERRO

~~~~bash
gyp ERR! find Python - Set the npm configuration variable python:
gyp ERR! find Python   npm config set python "/path/to/pythonexecutable"
gyp ERR! find Python For more information consult the documentation at:
gyp ERR! find Python https://github.com/nodejs/node-gyp#installation
gyp ERR! find Python **********************************************************
gyp ERR! find Python
gyp ERR! configure error
gyp ERR! stack Error: Could not find any Python installation to use
gyp ERR! stack     at PythonFinder.fail (/meu-backstage-teste/node_modules/node-gyp/lib/find-python.js:330:47)
gyp ERR! stack     at PythonFinder.runChecks (/meu-backstage-teste/node_modules/node-gyp/lib/find-python.js:159:21)
gyp ERR! stack     at PythonFinder.<anonymous> (/meu-backstage-teste/node_modules/node-gyp/lib/find-python.js:202:16)
gyp ERR! stack     at PythonFinder.execFileCallback (/meu-backstage-teste/node_modules/node-gyp/lib/find-python.js:294:16)
gyp ERR! stack     at exithandler (node:child_process:410:5)
gyp ERR! stack     at ChildProcess.errorhandler (node:child_process:422:5)
gyp ERR! stack     at ChildProcess.emit (node:events:513:28)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (node:internal/child_process:291:12)
gyp ERR! stack     at onErrorNT (node:internal/child_process:485:16)
gyp ERR! stack     at processTicksAndRejections (node:internal/process/task_queues:83:21)
gyp ERR! System Linux 4.19.0-17-amd64
gyp ERR! command "/usr/local/bin/node" "/meu-backstage-teste/node_modules/.bin/node-gyp" "rebuild" "--release" "-j" "4"
gyp ERR! cwd /meu-backstage-teste/node_modules/isolated-vm
gyp ERR! node -v v16.20.1
gyp ERR! node-gyp -v v9.4.0
gyp ERR! not ok
yarn install v1.22.19
[1/5] Validating package.json...
[2/5] Resolving packages...
[3/5] Fetching packages...
[4/5] Linking dependencies...
[5/5] Building fresh packages...
info This module is OPTIONAL, you can safely ignore this error
info This module is OPTIONAL, you can safely ignore this error
info This module is OPTIONAL, you can safely ignore this error
info This module is OPTIONAL, you can safely ignore this error
info Visit https://yarnpkg.com/en/docs/cli/install for documentation about this command.
  executing     yarn install ✖

Error: Could not execute command yarn install

It seems that something went wrong when creating the app 🤔

🔥  Failed to create app!

root@149d2b769492:/#

~~~~










# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 22/07/2023


- RETOMANDO INSTALAÇÃO DO APP

https://backstage.io/docs/deployment/docker/
<https://backstage.io/docs/deployment/docker/>

This section assumes that an app has already been created with @backstage/create-app, in which the frontend is bundled and served from the backend. This is done using the @backstage/plugin-app-backend plugin, which also injects the frontend configuration into the app. This means that you only need to build and deploy a single container in a minimal setup of Backstage. If you wish to separate the serving of the frontend out from the backend, see the separate frontend topic below.

Conforme a mensagem acima, é necessário criar o app antes, seguindo as instruções da página abaixo:
https://backstage.io/docs/getting-started/create-an-app/
<https://backstage.io/docs/getting-started/create-an-app/>


## INSTALANDO APP - BACKSTAGE

- Instalando app:
docker ps
docker-compose up -d
docker ps
docker container exec -ti node_app_teste bash
npx @backstage/create-app@latest


~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED      STATUS          PORTS                                       NAMES
149d2b769492   docker-compose_app-teste-fusionist   "docker-entrypoint.s…"   6 days ago   Up 40 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   node_app_teste
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker-compose down
Stopping node_app_teste ... done
Removing node_app_teste ... done
Removing network docker-compose_default
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$


fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ docker ps
CONTAINER ID   IMAGE                                     COMMAND                  CREATED         STATUS         PORTS                                       NAMES
4d901093c7c3   docker-compose_backstage-app-mandragora   "docker-entrypoint.s…"   6 seconds ago   Up 4 seconds   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   container_backstage_app_mandragora
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$ date
Sat 22 Jul 2023 01:55:18 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$

~~~~


container_backstage_app_mandragora
docker container exec -ti container_backstage_app_mandragora bash



- Antes de instalar o app novamente, vou buildar a imagem novamente, trocando a imagem Node slim pela normal, para evitar o erro de Python ocorrido antes.
VERSÃO ESCOLHIDA, 18.17
https://hub.docker.com/layers/library/node/18.17/images/sha256-adada03d29a294500de44f4d223c8dc44c28f66f68bc31b828b47d8010962ceb?context=explore
<https://hub.docker.com/layers/library/node/18.17/images/sha256-adada03d29a294500de44f4d223c8dc44c28f66f68bc31b828b47d8010962ceb?context=explore>
ela tem este package, por exemplo:
debian/python3.11 3.11.2-6
indicando que deve ter tudo sobre Python a principio

DE:
FROM node:16-bullseye-slim

PARA:
FROM node:18.17



~~~~bash
Removing intermediate container 24221d24c099
 ---> c5b549ebccbb
Step 5/7 : COPY . .
 ---> 49ad2a249366
Step 6/7 : EXPOSE 3000
 ---> Running in 208a74915a70
Removing intermediate container 208a74915a70
 ---> 65d2ed00b2c1
Step 7/7 : CMD ["node", "index.js"]
 ---> Running in b42b5a0f4397
Removing intermediate container b42b5a0f4397
 ---> ae1e7d59f1a5
Successfully built ae1e7d59f1a5
Successfully tagged docker-compose_backstage-app-mandragora:latest
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/docker-compose$

~~~~



- Instalando app:
docker ps
docker-compose up -d
docker ps
docker container exec -ti container_backstage_app_mandragora bash
npx @backstage/create-app@latest

~~~~bash
root@8a5b7e21d5d1:/#
root@8a5b7e21d5d1:/#
root@8a5b7e21d5d1:/# npx @backstage/create-app@latest
Need to install the following packages:
  @backstage/create-app@0.5.3
Ok to proceed? (y) y
? Enter a name for the app [required] backstage-app-mandragora

[................................................................]

 Moving to final location:
  moving        backstage-app-mandragora ✔

 Installing dependencies:
  determining   yarn version ✔
  executing     yarn install ✔
  executing     yarn tsc ✔

🥇  Successfully created backstage-app-mandragora


 All set! Now you might want to:
  Install the dependencies: cd backstage-app-mandragora && yarn install
  Run the app: cd backstage-app-mandragora && yarn dev
  Set up the software catalog: https://backstage.io/docs/features/software-catalog/configuration
  Add authentication: https://backstage.io/docs/auth/

root@8a5b7e21d5d1:/#

root@8a5b7e21d5d1:/# ls
app                       bin   dev  home      lib    lib64   media  node_modules  package-lock.json  proc  run   srv  tmp  var
backstage-app-mandragora  boot  etc  index.js  lib32  libx32  mnt    opt           package.json       root  sbin  sys  usr
root@8a5b7e21d5d1:/# ls -lhasp backstage-app-mandragora/
total 1.2M
 4.0K drwxr-xr-x    7 root root  4.0K Jul 22 17:32 ./
 4.0K drwxr-xr-x    1 root root  4.0K Jul 22 17:17 ../
 4.0K -rw-r--r--    1 root root   113 Jul 22 17:17 .dockerignore
 4.0K -rw-r--r--    1 root root    36 Jul 22 17:17 .eslintrc.js
 4.0K -rw-r--r--    1 root root   650 Jul 22 17:17 .gitignore
 4.0K -rw-r--r--    1 root root    33 Jul 22 17:17 .prettierignore
 4.0K -rw-r--r--    1 root root   150 Jul 22 17:17 README.md
 4.0K -rw-r--r--    1 root root    74 Jul 22 17:17 app-config.local.yaml
 4.0K -rw-r--r--    1 root root  1.6K Jul 22 17:17 app-config.production.yaml
 4.0K -rw-r--r--    1 root root  3.8K Jul 22 17:17 app-config.yaml
 4.0K -rw-r--r--    1 root root    26 Jul 22 17:17 backstage.json
 4.0K -rw-r--r--    1 root root   365 Jul 22 17:17 catalog-info.yaml
 4.0K drwxr-xr-x    3 root root  4.0K Jul 22 17:32 dist-types/
 4.0K drwxr-xr-x    3 root root  4.0K Jul 22 17:17 examples/
 4.0K -rw-r--r--    1 root root   116 Jul 22 17:17 lerna.json
  64K drwxr-xr-x 1688 root root   64K Jul 22 17:32 node_modules/
 4.0K -rw-r--r--    1 root root  1.5K Jul 22 17:17 package.json
 4.0K drwxr-xr-x    4 root root  4.0K Jul 22 17:17 packages/
 4.0K drwxr-xr-x    2 root root  4.0K Jul 22 17:17 plugins/
 4.0K -rw-r--r--    1 root root   272 Jul 22 17:17 tsconfig.json
1004K -rw-r--r--    1 root root 1003K Jul 22 17:32 yarn.lock

~~~~


- OK! Instalou o app.



https://backstage.io/docs/getting-started/create-an-app/

Run the app

When the installation is complete you can open the app folder and start the app.

cd my-backstage-app
yarn dev

The yarn dev command will run both the frontend and backend as separate processes (named [0] and [1]) in the same window. When the command finishes running, it should open up a browser window displaying your app. If not, you can open a browser and directly navigate to the frontend at http://localhost:3000.

Now you're free to hack away on your own Backstage installation!

As you get more experienced with the app, in future you can run just the frontend with yarn start in one window, and the backend with yarn start-backend in a different window.


- Comando editado
cd backstage-app-mandragora
yarn dev



<http://192.168.0.110:3000/>







- ERROS
porta 3000 em uso

~~~~bash

root@8a5b7e21d5d1:/# cd backstage-app-mandragora
yarn dev
yarn run v1.22.19
$ concurrently "yarn start" "yarn start-backend"
$ yarn workspace app start
$ yarn workspace backend start
$ backstage-cli package start
$ backstage-cli package start
[1] Build succeeded
[0] Loaded config from app-config.yaml
[0] fatal: not a git repository (or any of the parent directories): .git
[0] WARNING: Failed to read git commit, ExitCodeError: Command 'git rev-parse HEAD' exited with code 128
[0] fatal: not a git repository (or any of the parent directories): .git
[0] WARNING: Failed to describe git version, ExitCodeError: Command 'git describe --always' exited with code 128
[0] /backstage-app-mandragora/node_modules/webpack-dev-server/lib/Server.js:2557
[0]         throw error;
[0]         ^
[0]
[0] Error: listen EADDRINUSE: address already in use 127.0.0.1:3000
[0]     at Server.setupListenHandle [as _listen2] (node:net:1751:16)
[0]     at listenInCluster (node:net:1799:12)
[0]     at GetAddrInfoReqWrap.doListen [as callback] (node:net:1948:7)
[0]     at GetAddrInfoReqWrap.onlookup [as oncomplete] (node:dns:110:8) {
[0]   code: 'EADDRINUSE',
[0]   errno: -98,
[0]   syscall: 'listen',
[0]   address: '127.0.0.1',
[0]   port: 3000
[0] }
[0]
[0] Node.js v18.17.0
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
error Command failed.
[0] Exit code: 1
[0] Command: /usr/local/bin/node
[0] Arguments: /opt/yarn-v1.22.19/lib/cli.js start
[0] Directory: /backstage-app-mandragora/packages/app
[0] Output:
[0]
info Visit https://yarnpkg.com/en/docs/cli/workspace for documentation about this command.
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
[0] yarn start exited with code 1
[1] Loaded config from app-config.yaml
[1] 2023-07-22T18:53:32.582Z backstage info Found 2 new secrets in config that will be redacted
[1] 2023-07-22T18:53:32.600Z backstage info Created UrlReader predicateMux{readers=azure{host=dev.azure.com,authed=false},bitbucketCloud{host=bitbucket.org,authed=false},github{host=github.com,authed=false},gitlab{host=gitlab.com,authed=false},awsS3{host=amazonaws.com,authed=false},fetch{}
[1] 2023-07-22T18:53:32.634Z catalog info Performing database migration type=plugin
[1] 2023-07-22T18:53:32.815Z auth info Configuring "database" as KeyStore provider type=plugin
[1] 2023-07-22T18:53:32.833Z techdocs info Creating Local publisher for TechDocs type=plugin
[1] 2023-07-22T18:53:32.840Z search info Added DefaultCatalogCollatorFactory collator factory for type software-catalog type=plugin
[1] 2023-07-22T18:53:32.842Z search info Added DefaultTechDocsCollatorFactory collator factory for type techdocs type=plugin
[1] 2023-07-22T18:53:32.843Z search info Starting all scheduled search tasks. type=plugin
[1] 2023-07-22T18:53:32.859Z backstage info Listening on :7007
[1] 2023-07-22T18:53:32.872Z backstage info Task worker starting: search_index_software_catalog, {"version":2,"cadence":"PT10M","initialDelayDuration":"PT3S","timeoutAfterDuration":"PT15M"} type=taskManager task=search_index_software_catalog
[1] 2023-07-22T18:53:32.873Z backstage info Task worker starting: search_index_techdocs, {"version":2,"cadence":"PT10M","initialDelayDuration":"PT3S","timeoutAfterDuration":"PT15M"} type=taskManager task=search_index_techdocs
[1] 2023-07-22T18:53:35.884Z search info Collating documents for software-catalog via DefaultCatalogCollatorFactory type=plugin documentType=software-catalog
[1] 2023-07-22T18:53:35.930Z search info Collating documents for techdocs via DefaultTechDocsCollatorFactory type=plugin documentType=techdocs
[1] 2023-07-22T18:53:35.965Z backstage info ::ffff:127.0.0.1 - - [22/Jul/2023:18:53:35 +0000] "GET /api/catalog/entities?filter=metadata.annotations.backstage.io%2Ftechdocs-ref&fields=kind,namespace,metadata.annotations,metadata.name,metadata.title,metadata.namespace,spec.type,spec.lifecycle,relations&offset=0&limit=500 HTTP/1.1" 200 2 "-" "node-fetch/1.0 (+https://github.com/bitinn/node-fetch)" type=incomingRequest
[1] 2023-07-22T18:53:35.971Z search warn Index for techdocs was not created: indexer received 0 documents type=plugin documentType=techdocs
[1] 2023-07-22T18:53:35.973Z search info Collating documents for techdocs succeeded type=plugin documentType=techdocs
[1] 2023-07-22T18:53:35.978Z backstage info ::ffff:127.0.0.1 - - [22/Jul/2023:18:53:35 +0000] "GET /api/catalog/entities?offset=0&limit=500 HTTP/1.1" 200 - "-" "node-fetch/1.0 (+https://github.com/bitinn/node-fetch)" type=incomingRequest
[1] 2023-07-22T18:53:35.992Z search info Collating documents for software-catalog succeeded type=plugin documentType=software-catalog
^C
root@8a5b7e21d5d1:/backstage-app-mandragora# [1] yarn start-backend exited with code SIGINT
^C^C
root@8a5b7e21d5d1:/backstage-app-mandragora# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 17:16 ?        00:00:00 node index.js
root         15      0  0 17:16 pts/0    00:00:00 bash
root       1829      1  0 18:53 pts/0    00:00:00 [sh] <defunct>
root       1861      1  0 18:53 pts/0    00:00:00 [sh] <defunct>
root       1885      1  0 18:53 pts/0    00:00:00 [node] <defunct>
root       1907      1  0 18:53 pts/0    00:00:00 [sh] <defunct>
root       1939      1  7 18:53 pts/0    00:00:11 [node] <defunct>
root       1955     15  0 18:56 pts/0    00:00:00 ps -ef
root@8a5b7e21d5d1:/backstage-app-mandragora# kill -9 1
root@8a5b7e21d5d1:/backstage-app-mandragora# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 17:16 ?        00:00:00 node index.js
root         15      0  0 17:16 pts/0    00:00:00 bash
root       1829      1  0 18:53 pts/0    00:00:00 [sh] <defunct>
root       1861      1  0 18:53 pts/0    00:00:00 [sh] <defunct>
root       1885      1  0 18:53 pts/0    00:00:00 [node] <defunct>
root       1907      1  0 18:53 pts/0    00:00:00 [sh] <defunct>
root       1939      1  6 18:53 pts/0    00:00:11 [node] <defunct>
root       1956     15  0 18:56 pts/0    00:00:00 ps -ef
root@8a5b7e21d5d1:/backstage-app-mandragora#

~~~~






https://github.com/backstage/demo/blob/master/Dockerfile

https://john-tucker.medium.com/backstage-by-example-part-1-a18e74849240









- Testando local novamente
cd ~/cursos/idp-devportal/backstage/docker/multi-stage/teste2
npx @backstage/create-app@latest

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ du -sh app-teste2/
376K    app-teste2/
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ date
Sat 22 Jul 2023 05:06:54 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ du -sh app-teste2/
376K    app-teste2/
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ date
Sat 22 Jul 2023 05:09:42 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ du -sh app-teste2/
75M     app-teste2/
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ date
Sat 22 Jul 2023 05:13:22 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$

~~~~


- Incrementando devagar, mas tá.


- Instalação terminou:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ du -sh app-teste2/
1.3G    app-teste2/
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ date
Sat 22 Jul 2023 05:30:29 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$

~~~~






https://github.com/backstage/demo/blob/master/Dockerfile

https://john-tucker.medium.com/backstage-by-example-part-1-a18e74849240





# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- Efetuar build usando a instalação feita via container com node18.17, instalação tá na pasta "backstage/backup-fernando-instalacao".
- Ou instalação local via Debain mesmo, que está na pasta "backstage/docker/multi-stage/teste2/app-teste2".
- Utilizar o Multi-Stage na primeira tentativa:
        https://backstage.io/docs/deployment/docker/
        https://github.com/backstage/demo/blob/master/Dockerfile
- Ou tentar tutorial de 3 partes do blog:
        https://john-tucker.medium.com/backstage-by-example-part-1-a18e74849240

- Montar docker-compose com NodeJS semelhante a versão usada no doc sobre k8s da Backstage. Instalar o app do Backstage.
- Caso necessário, usar repo "kubedev" como apoio, sobre Dockerfile, NodeJS, etc.
- Buildar o APP do Backstage com estrutura via Docker-compose.
- TSHOOT, erro do yarn install travado durante criação do APP do Backstage via npx.
        https://backstage.io/docs/getting-started/create-an-app/
        issue:
        https://github.com/backstage/backstage/issues/18058
        Analisar:
        https://lightrun.com/answers/backstage-backstage-npx-backstagecreate-app-node-is-incompatible-with-this-module
- Buildar imagem Docker, após APP ficar OK.
- Personalizar "app-config.yaml"










- Efetuar build usando a instalação feita via container com node18.17, instalação tá na pasta "backstage/backup-fernando-instalacao".
- Ou instalação local via Debain mesmo, que está na pasta "backstage/docker/multi-stage/teste2/app-teste2".

- Copiada pasta "backstage/docker/multi-stage/teste2/app-teste2" para:
/home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3/app-teste2

- Modificar o node de 16 para 18.17
https://hub.docker.com/layers/library/node/18.17.0-bullseye-slim/images/sha256-947fcbfa6e6bc43e8dcf2223aa2b37b5543cb8fc40bf0049804e45125fb6873a?context=explore
node:18.17.0-bullseye-slim

- Buildando
cd /home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3

Once you have added both the Dockerfile and .dockerignore to the root of your project, run the following to build the container under a specified tag.

docker image build -t backstage-tentativa3 .

To try out the image locally you can run the following:

docker run -it -p 7007:7007 backstage-tentativa3



- Testando build

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ cd /home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker image build -t backstage-tentativa3 .
Sending build context to Docker daemon  1.604MB
Step 1/27 : FROM node:18.17.0-bullseye-slim AS packages


fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/teste2$ cd /home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker image build -t backstage-tentativa3 .
Sending build context to Docker daemon  1.604MB
Step 1/27 : FROM node:18.17.0-bullseye-slim AS packages
18.17.0-bullseye-slim: Pulling from library/node
9d21b12d5fab: Already exists
ce632e02ef79: Already exists
17fd286e480b: Pull complete
58dba8cfdcb8: Pull complete
6f617a58ba1e: Pull complete
Digest: sha256:d2617c7df857596e4f29715c7a4d8e861852648865d110c7d3c82e3606e6f0ba
Status: Downloaded newer image for node:18.17.0-bullseye-slim
 ---> eb0946b189e9
Step 2/27 : WORKDIR /app
 ---> Running in 766ad1e455dd
Removing intermediate container 766ad1e455dd
 ---> 18ac5537d7c5
Step 3/27 : COPY package.json yarn.lock ./
 ---> 04947893e477
Step 4/27 : COPY packages packages
 ---> 7dbe71c18599
Step 5/27 : COPY plugins plugins
COPY failed: file not found in build context or excluded by .dockerignore: stat plugins: file does not exist
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

~~~~


ERRO
COPY failed: file not found in build context or excluded by .dockerignore: stat plugins: file does not exist




- Ajustando
/home/fernando/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3/.dockerignore

DE:

.git
.yarn/cache
.yarn/install-state.gz
node_modules
packages/*/src
packages/*/node_modules
plugins
*.local.yaml


PARA:

dist-types
node_modules
packages/*/dist
packages/*/node_modules
plugins/*/dist
plugins/*/node_modules



- ERRO

~~~~BASH

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker image build -t backstage-tentativa3 .
Sending build context to Docker daemon  1.212MB
Step 1/27 : FROM node:18.17.0-bullseye-slim AS packages
 ---> eb0946b189e9
Step 2/27 : WORKDIR /app
 ---> Using cache
 ---> 18ac5537d7c5
Step 3/27 : COPY package.json yarn.lock ./
 ---> Using cache
 ---> 04947893e477
Step 4/27 : COPY packages packages
 ---> 3a1dfa38caa2
Step 5/27 : COPY plugins plugins
 ---> 1c136d8c62f4
Step 6/27 : RUN find packages \! -name "package.json" -mindepth 2 -maxdepth 2 -exec rm -rf {} \+
 ---> Running in 5ba3bb850f07
Removing intermediate container 5ba3bb850f07
 ---> 6991084d6825
Step 7/27 : FROM node:18.17.0-bullseye-slim AS build
 ---> eb0946b189e9
Step 8/27 : RUN --mount=type=cache,target=/var/cache/apt,sharing=locked     --mount=type=cache,target=/var/lib/apt,sharing=locked     apt-get update &&     apt-get install -y --no-install-recommends libsqlite3-dev python3 build-essential &&     yarn config set python /usr/bin/python3
the --mount option requires BuildKit. Refer to https://docs.docker.com/go/buildkit/ to learn how to build images with BuildKit enabled
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

~~~~



- Tratar erro
the --mount option requires BuildKit. Refer to https://docs.docker.com/go/buildkit/ to learn how to build images with BuildKit enabled






# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- Tratar erro
the --mount option requires BuildKit. Refer to https://docs.docker.com/go/buildkit/ to learn how to build images with BuildKit enabled

- Efetuar build usando a instalação feita via container com node18.17, instalação tá na pasta "backstage/backup-fernando-instalacao".
- Ou instalação local via Debain mesmo, que está na pasta "backstage/docker/multi-stage/teste2/app-teste2".
- Utilizar o Multi-Stage na primeira tentativa:
        https://backstage.io/docs/deployment/docker/
        https://github.com/backstage/demo/blob/master/Dockerfile
- Ou tentar tutorial de 3 partes do blog:
        https://john-tucker.medium.com/backstage-by-example-part-1-a18e74849240

- Montar docker-compose com NodeJS semelhante a versão usada no doc sobre k8s da Backstage. Instalar o app do Backstage.
- Caso necessário, usar repo "kubedev" como apoio, sobre Dockerfile, NodeJS, etc.
- Buildar o APP do Backstage com estrutura via Docker-compose.
- TSHOOT, erro do yarn install travado durante criação do APP do Backstage via npx.
        https://backstage.io/docs/getting-started/create-an-app/
        issue:
        https://github.com/backstage/backstage/issues/18058
        Analisar:
        https://lightrun.com/answers/backstage-backstage-npx-backstagecreate-app-node-is-incompatible-with-this-module
- Buildar imagem Docker, após APP ficar OK.
- Personalizar "app-config.yaml"










# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 29/07/2023


cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
docker image build -t backstage-tentativa3 .

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage$
fernando@debian10x64:~/cursos/idp-devportal/backstage$ cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker image build -t backstage-tentativa3 .
Sending build context to Docker daemon  1.212MB
Step 1/27 : FROM node:18.17.0-bullseye-slim AS packages
 ---> eb0946b189e9
Step 2/27 : WORKDIR /app
 ---> Using cache
 ---> 18ac5537d7c5
Step 3/27 : COPY package.json yarn.lock ./
 ---> Using cache
 ---> 04947893e477
Step 4/27 : COPY packages packages
 ---> Using cache
 ---> 3a1dfa38caa2
Step 5/27 : COPY plugins plugins
 ---> Using cache
 ---> 1c136d8c62f4
Step 6/27 : RUN find packages \! -name "package.json" -mindepth 2 -maxdepth 2 -exec rm -rf {} \+
 ---> Using cache
 ---> 6991084d6825
Step 7/27 : FROM node:18.17.0-bullseye-slim AS build
 ---> eb0946b189e9
Step 8/27 : RUN --mount=type=cache,target=/var/cache/apt,sharing=locked     --mount=type=cache,target=/var/lib/apt,sharing=locked     apt-get update &&     apt-get install -y --no-install-recommends libsqlite3-dev python3 build-essential &&     yarn config set python /usr/bin/python3
the --mount option requires BuildKit. Refer to https://docs.docker.com/go/buildkit/ to learn how to build images with BuildKit enabled
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

~~~~




https://docs.docker.com/build/buildkit/#getting-started
<https://docs.docker.com/build/buildkit/#getting-started>

To set the BuildKit environment variable when running the docker build command, run:

DOCKER_BUILDKIT=1 docker build .



DOCKER_BUILDKIT=1 docker build -t backstage-tentativa3 .

- Testando

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ DOCKER_BUILDKIT=1 docker build -t backstage-tentativa3 .
[+] Building 6.0s (11/23)
 => [internal] load build definition from Dockerfile                                                                                                                                 0.1s
 => => transferring dockerfile: 3.24kB                                                                                                                                               0.0s
 => [internal] load .dockerignore                                                                                                                                                    0.0s
 => => transferring context: 141B                                                                                                                                                    0.0s
 => [internal] load metadata for docker.io/library/node:18.17.0-bullseye-slim                                                                                                        0.0s
 => [internal] settings cache mount permissions                                                                                                                                      0.0s
 => [internal] load build context                                                                                                                                                    0.1s
 => => transferring context: 1.16MB                                                                                                                                                  0.0s
 => [packages 1/6] FROM docker.io/library/node:18.17.0-bullseye-slim                                                                                                                 0.1s
 => [packages 2/6] WORKDIR /app                                                                                                                                                      0.1s
 => [build 2/9] RUN --mount=type=cache,target=/var/cache/apt,sharing=locked     --mount=type=cache,target=/var/lib/apt,sharing=locked     apt-get update &&     apt-get install -y   5.7s
 => => # Get:12 http://deb.debian.org/debian bullseye/main amd64 libncursesw6 amd64 6.2+20201114-2+deb11u1 [132 kB]
 => => # Get:13 http://deb.debian.org/debian bullseye/main amd64 readline-common all 8.1-1 [73.7 kB]
 => => # Get:14 http://deb.debian.org/debian bullseye/main amd64 libreadline8 amd64 8.1-1 [169 kB]
 => => # Get:15 http://deb.debian.org/debian bullseye/main amd64 libsqlite3-0 amd64 3.34.1-3 [797 kB]
 => => # Get:16 http://deb.debian.org/debian bullseye/main amd64 libpython3.9-stdlib amd64 3.9.2-1 [1684 kB]
 => => # Get:17 http://deb.debian.org/debian bullseye/main amd64 python3.9 amd64 3.9.2-1 [466 kB]
 => [packages 3/6] COPY package.json yarn.lock ./                                                                                                                                    0.2s
 => [packages 4/6] COPY packages packages                                                                                                                                            0.1s
 => [packages 5/6] COPY plugins plugins                                                                                                                                              0.1s
 => [packages 6/6] RUN find packages ! -name "package.json" -mindepth 2 -maxdepth 2 -exec rm -rf {} +                                                                                0.6s




fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ DOCKER_BUILDKIT=1 docker build -t backstage-tentativa3 .
[+] Building 138.9s (14/23)
 => [internal] load build definition from Dockerfile                                                                                                                                 0.1s
 => => transferring dockerfile: 3.24kB                                                                                                                                               0.0s
 => [internal] load .dockerignore                                                                                                                                                    0.0s
 => => transferring context: 141B                                                                                                                                                    0.0s
 => [internal] load metadata for docker.io/library/node:18.17.0-bullseye-slim                                                                                                        0.0s
 => [internal] settings cache mount permissions                                                                                                                                      0.0s
 => [internal] load build context                                                                                                                                                    0.1s
 => => transferring context: 1.16MB                                                                                                                                                  0.0s
 => [packages 1/6] FROM docker.io/library/node:18.17.0-bullseye-slim                                                                                                                 0.1s
 => [packages 2/6] WORKDIR /app                                                                                                                                                      0.1s
 => [build 2/9] RUN --mount=type=cache,target=/var/cache/apt,sharing=locked     --mount=type=cache,target=/var/lib/apt,sharing=locked     apt-get update &&     apt-get install -y  39.7s
 => [packages 3/6] COPY package.json yarn.lock ./                                                                                                                                    0.2s
 => [packages 4/6] COPY packages packages                                                                                                                                            0.1s
 => [packages 5/6] COPY plugins plugins                                                                                                                                              0.1s
 => [packages 6/6] RUN find packages ! -name "package.json" -mindepth 2 -maxdepth 2 -exec rm -rf {} +                                                                                0.6s
 => [build 3/9] WORKDIR /app                                                                                                                                                         0.1s
 => [build 4/9] COPY --from=packages --chown=node:node /app .                                                                                                                        0.2s
 => [build 5/9] RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000     yarn install --frozen-lockfile --network-timeout 600000                   98.6s
 => => # yarn install v1.22.19
 => => # [1/5] Validating package.json...
 => => # [2/5] Resolving packages...
 => => # [3/5] Fetching packages...
 => => # warning Pattern ["app@link:packages/app"] is trying to unpack in the same destination "/home/node/.cache/yarn/v6/npm-app-0.0.0/node_modules/app" as pattern ["app@0.0.0","app@0.
 => => # 0.0"]. This could result in non-deterministic behavior, skipping.




fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ DOCKER_BUILDKIT=1 docker build -t backstage-tentativa3 .
[+] Building 589.5s (17/23)
 => [internal] load build definition from Dockerfile                                                                                                                                 0.1s
 => => transferring dockerfile: 3.24kB                                                                                                                                               0.0s
 => [internal] load .dockerignore                                                                                                                                                    0.0s
 => => transferring context: 141B                                                                                                                                                    0.0s
 => [internal] load metadata for docker.io/library/node:18.17.0-bullseye-slim                                                                                                        0.0s
 => [internal] settings cache mount permissions                                                                                                                                      0.0s
 => [internal] load build context                                                                                                                                                    0.1s
 => => transferring context: 1.16MB                                                                                                                                                  0.0s
 => [packages 1/6] FROM docker.io/library/node:18.17.0-bullseye-slim                                                                                                                 0.1s
 => [packages 2/6] WORKDIR /app                                                                                                                                                      0.1s
 => [build 2/9] RUN --mount=type=cache,target=/var/cache/apt,sharing=locked     --mount=type=cache,target=/var/lib/apt,sharing=locked     apt-get update &&     apt-get install -y  39.7s
 => [packages 3/6] COPY package.json yarn.lock ./                                                                                                                                    0.2s
 => [packages 4/6] COPY packages packages                                                                                                                                            0.1s
 => [packages 5/6] COPY plugins plugins                                                                                                                                              0.1s
 => [packages 6/6] RUN find packages ! -name "package.json" -mindepth 2 -maxdepth 2 -exec rm -rf {} +                                                                                0.6s
 => [build 3/9] WORKDIR /app                                                                                                                                                         0.1s
 => [build 4/9] COPY --from=packages --chown=node:node /app .                                                                                                                        0.2s
 => [build 5/9] RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000     yarn install --frozen-lockfile --network-timeout 600000                  500.2s
 => [build 6/9] COPY --chown=node:node . .                                                                                                                                           0.1s
 => [build 7/9] RUN yarn tsc                                                                                                                                                        11.5s
 => [build 8/9] RUN yarn --cwd packages/backend build                                                                                                                               37.3s
 => => # app:  WARNING: Failed to read git commit, ExitCodeError: Command 'git rev-parse HEAD' exited with code 127
 => => # app:
 => => # app:  /bin/sh: 1: git: not found
 => => # app:
 => => # app:  WARNING: Failed to describe git version, ExitCodeError: Command 'git describe --always' exited with code 127
 => => # app:




fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ DOCKER_BUILDKIT=1 docker build -t backstage-tentativa3 .
[+] Building 697.7s (23/24)
 => [internal] load build definition from Dockerfile                                                                                                                                 0.1s
 => => transferring dockerfile: 3.24kB                                                                                                                                               0.0s
 => [internal] load .dockerignore                                                                                                                                                    0.0s
 => => transferring context: 141B                                                                                                                                                    0.0s
 => [internal] load metadata for docker.io/library/node:18.17.0-bullseye-slim                                                                                                        0.0s
 => [internal] settings cache mount permissions                                                                                                                                      0.0s
 => [internal] load build context                                                                                                                                                    0.1s
 => => transferring context: 1.16MB                                                                                                                                                  0.0s
 => [packages 1/6] FROM docker.io/library/node:18.17.0-bullseye-slim                                                                                                                 0.1s
 => [packages 2/6] WORKDIR /app                                                                                                                                                      0.1s
 => [build 2/9] RUN --mount=type=cache,target=/var/cache/apt,sharing=locked     --mount=type=cache,target=/var/lib/apt,sharing=locked     apt-get update &&     apt-get install -y  39.7s
 => [packages 3/6] COPY package.json yarn.lock ./                                                                                                                                    0.2s
 => [packages 4/6] COPY packages packages                                                                                                                                            0.1s
 => [packages 5/6] COPY plugins plugins                                                                                                                                              0.1s
 => [packages 6/6] RUN find packages ! -name "package.json" -mindepth 2 -maxdepth 2 -exec rm -rf {} +                                                                                0.6s
 => [build 3/9] WORKDIR /app                                                                                                                                                         0.1s
 => [build 4/9] COPY --from=packages --chown=node:node /app .                                                                                                                        0.2s
 => [build 5/9] RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000     yarn install --frozen-lockfile --network-timeout 600000                  500.2s
 => [build 6/9] COPY --chown=node:node . .                                                                                                                                           0.1s
 => [build 7/9] RUN yarn tsc                                                                                                                                                        11.5s
 => [build 8/9] RUN yarn --cwd packages/backend build                                                                                                                               50.5s
 => [build 9/9] RUN mkdir packages/backend/dist/skeleton packages/backend/dist/bundle     && tar xzf packages/backend/dist/skeleton.tar.gz -C packages/backend/dist/skeleton     &&  1.2s
 => [stage-2 4/7] COPY --from=build --chown=node:node /app/yarn.lock /app/package.json /app/packages/backend/dist/skeleton/ ./                                                       0.3s
 => [stage-2 5/7] RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000     yarn install --frozen-lockfile --production --network-timeout 600000    82.3s
 => [stage-2 6/7] COPY --from=build --chown=node:node /app/packages/backend/dist/bundle/ ./                                                                                          0.4s
 => [stage-2 7/7] COPY --chown=node:node app-config.yaml ./                                                                                                                          0.1s
 => exporting to image                                                                                                                                                               7.2s
 => => exporting layers                                                                                                                                                              7.2s




fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ DOCKER_BUILDKIT=1 docker build -t backstage-tentativa3 .
[+] Building 698.0s (24/24) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                                                 0.1s
 => => transferring dockerfile: 3.24kB                                                                                                                                               0.0s
 => [internal] load .dockerignore                                                                                                                                                    0.0s
 => => transferring context: 141B                                                                                                                                                    0.0s
 => [internal] load metadata for docker.io/library/node:18.17.0-bullseye-slim                                                                                                        0.0s
 => [internal] settings cache mount permissions                                                                                                                                      0.0s
 => [internal] load build context                                                                                                                                                    0.1s
 => => transferring context: 1.16MB                                                                                                                                                  0.0s
 => [packages 1/6] FROM docker.io/library/node:18.17.0-bullseye-slim                                                                                                                 0.1s
 => [packages 2/6] WORKDIR /app                                                                                                                                                      0.1s
 => [build 2/9] RUN --mount=type=cache,target=/var/cache/apt,sharing=locked     --mount=type=cache,target=/var/lib/apt,sharing=locked     apt-get update &&     apt-get install -y  39.7s
 => [packages 3/6] COPY package.json yarn.lock ./                                                                                                                                    0.2s
 => [packages 4/6] COPY packages packages                                                                                                                                            0.1s
 => [packages 5/6] COPY plugins plugins                                                                                                                                              0.1s
 => [packages 6/6] RUN find packages ! -name "package.json" -mindepth 2 -maxdepth 2 -exec rm -rf {} +                                                                                0.6s
 => [build 3/9] WORKDIR /app                                                                                                                                                         0.1s
 => [build 4/9] COPY --from=packages --chown=node:node /app .                                                                                                                        0.2s
 => [build 5/9] RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000     yarn install --frozen-lockfile --network-timeout 600000                  500.2s
 => [build 6/9] COPY --chown=node:node . .                                                                                                                                           0.1s
 => [build 7/9] RUN yarn tsc                                                                                                                                                        11.5s
 => [build 8/9] RUN yarn --cwd packages/backend build                                                                                                                               50.5s
 => [build 9/9] RUN mkdir packages/backend/dist/skeleton packages/backend/dist/bundle     && tar xzf packages/backend/dist/skeleton.tar.gz -C packages/backend/dist/skeleton     &&  1.2s
 => [stage-2 4/7] COPY --from=build --chown=node:node /app/yarn.lock /app/package.json /app/packages/backend/dist/skeleton/ ./                                                       0.3s
 => [stage-2 5/7] RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000     yarn install --frozen-lockfile --production --network-timeout 600000    82.3s
 => [stage-2 6/7] COPY --from=build --chown=node:node /app/packages/backend/dist/bundle/ ./                                                                                          0.4s
 => [stage-2 7/7] COPY --chown=node:node app-config.yaml ./                                                                                                                          0.1s
 => exporting to image                                                                                                                                                               7.5s
 => => exporting layers                                                                                                                                                              7.5s
 => => writing image sha256:ab2bbae1faa9d1705b816441db3fafc147f0d90a38ae47c9d076bee2ff10a25d                                                                                         0.0s
 => => naming to docker.io/library/backstage-tentativa3                                                                                                                              0.0s
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ date
Sat 29 Jul 2023 04:24:58 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$


~~~~






- OK, buildou a imagem:

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker image ls
REPOSITORY                                TAG                     IMAGE ID       CREATED          SIZE
backstage-tentativa3                      latest                  ab2bbae1faa9   31 seconds ago   1.01GB
<none>                                    <none>                  6991084d6825   6 days ago       249MB
<none>                                    <none>                  7dbe71c18599   6 days ago       249MB
docker-compose_backstage-app-mandragora   latest                  ae1e7d59f1a5   7 days ago       1.1GB
<none>                                    <none>                  18f9cae4a259   7 days ago       197MB
node                                      18.17.0-bullseye-slim   eb0946b189e9   9 days ago       248MB
node                                      18.17                   0f9df951673d   9 days ago       1.09GB
docker-compose_app-teste-fusionist        latest                  af2a7918abb4   13 days ago      197MB
node                                      16-bullseye-slim        6b02cfd592ca   3 weeks ago      191MB
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ date
Sat 29 Jul 2023 04:25:15 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

~~~~





https://backstage.io/docs/deployment/docker/
<https://backstage.io/docs/deployment/docker/>

To try out the image locally you can run the following:

docker run -it -p 7007:7007 backstage


- Comando editado:
docker run -it -p 7007:7007 backstage-tentativa3


~~~~bash
fernando@debian10x64:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS          PORTS                                       NAMES
af06e17e5f73   backstage-tentativa3   "docker-entrypoint.s…"   21 seconds ago   Up 20 seconds   0.0.0.0:7007->7007/tcp, :::7007->7007/tcp   nervou                                        s_johnson
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS          PORTS                                       NAMES
af06e17e5f73   backstage-tentativa3   "docker-entrypoint.s…"   29 seconds ago   Up 29 seconds   0.0.0.0:7007->7007/tcp, :::7007->7007/tcp   nervous_johnson
fernando@debian10x64:~$
fernando@debian10x64:~$
fernando@debian10x64:~$ date
Sat 29 Jul 2023 04:37:56 PM -03
fernando@debian10x64:~$


~~~~



http://192.168.0.110:7007/


~~~~bash
fernando@debian10x64:~$ curl http://192.168.0.110:7007/
<!doctype html><html lang="en"><head><meta charset="utf-8"/><meta name="viewport" content="width=device-width,initial-scale=1"/><meta name="theme-color" content="#000000"/><meta name="description" content="Backstage is an open platform for building developer portals"/><link rel="apple-touch-icon" href="/logo192.png"/><link rel="manifest" href="/manifest.json" crossorigin="use-credentials"/><link rel="icon" href="/favicon.ico"/><link rel="shortcut icon" href="/favicon.ico"/><link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png"/><link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png"/><link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png"/><link rel="mask-icon" href="/safari-pinned-tab.svg" color="#5bbad5"/><title>Scaffolded Backstage App</title><script defer="defer" src="/static/runtime.e117edc2.js"></script><script defer="defer" src="/static/module-material-ui.74b8ec81.js"></script><script defer="defer" src="/static/module-lodash.4255ece8.js"></script><script defer="defer" src="/static/module-backstage.f5d0c5b8.js"></script><script defer="defer" src="/static/module-date-fns.7572de26.js"></script><script defer="defer" src="/static/module-yaml.3442c513.js"></script><script defer="defer" src="/static/module-material-table.4dbe2987.js"></script><script defer="defer" src="/static/module-luxon.4200c284.js"></script><script defer="defer" src="/static/module-react-beautiful-dnd.58836c3e.js"></script><script defer="defer" src="/static/module-micromark-core-commonmark.ca51b777.js"></script><script defer="defer" src="/static/module-octokit.36cfc981.js"></script><script defer="defer" src="/static/module-zod.bf185f0f.js"></script><script defer="defer" src="/static/module-photoswipe.c7e60322.js"></script><script defer="defer" src="/static/module-react-dom.d69a898f.js"></script><script defer="defer" src="/static/module-remix-run.5b9692ce.js"></script><script defer="defer" src="/static/vendor.e117edc2.js"></script><script defer="defer" src="/static/main.e117edc2.js"></script><link href="/static/vendor.a3186a5f.css" rel="stylesheet"></head><body><noscript>You need to enable JavaScript to run this app.</noscript><div id="root"></div></body></html>fernando@debian10x64:~$
~~~~



- No navegador do Notebook nao abre, abre uma página em branco com titulo "Scaffolded Backstage App":
http://192.168.0.110:7007/
<http://192.168.0.110:7007/>

- No browser do servidor Debian, abriu corretamente:
http://localhost:7007/
<http://localhost:7007/>











- Ler
https://roadie.io/blog/backstage-docker-service-catalog/





# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- Ler
https://roadie.io/blog/backstage-docker-service-catalog/

- Verificar porque nao abre via notebook.

- Efetuar build usando a instalação feita via container com node18.17, instalação tá na pasta "backstage/backup-fernando-instalacao".
- Ou instalação local via Debain mesmo, que está na pasta "backstage/docker/multi-stage/teste2/app-teste2".
- Utilizar o Multi-Stage na primeira tentativa:
        https://backstage.io/docs/deployment/docker/
        https://github.com/backstage/demo/blob/master/Dockerfile
- Ou tentar tutorial de 3 partes do blog:
        https://john-tucker.medium.com/backstage-by-example-part-1-a18e74849240

- Montar docker-compose com NodeJS semelhante a versão usada no doc sobre k8s da Backstage. Instalar o app do Backstage.
- Caso necessário, usar repo "kubedev" como apoio, sobre Dockerfile, NodeJS, etc.
- Buildar o APP do Backstage com estrutura via Docker-compose.
- TSHOOT, erro do yarn install travado durante criação do APP do Backstage via npx.
        https://backstage.io/docs/getting-started/create-an-app/
        issue:
        https://github.com/backstage/backstage/issues/18058
        Analisar:
        https://lightrun.com/answers/backstage-backstage-npx-backstagecreate-app-node-is-incompatible-with-this-module
- Buildar imagem Docker, após APP ficar OK.
- Personalizar "app-config.yaml"








cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3

docker run -it -p 7007:7007 backstage-tentativa3







https://backstage.io/docs/conf/writing/
<https://backstage.io/docs/conf/writing/>

- Ajustando listen

DE:

~~~~YAML

backend:
  # Used for enabling authentication, secret is shared by all backend plugins
  # See https://backstage.io/docs/auth/service-to-service-auth for
  # information on the format
  # auth:
  #   keys:
  #     - secret: ${BACKEND_SECRET}
  baseUrl: http://localhost:7007
  listen:
    port: 7007
    # Uncomment the following host directive to bind to specific interfaces
    # host: 127.0.0.1
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
  baseUrl: http://localhost:7007
  listen:
    port: 7007
    # Uncomment the following host directive to bind to specific interfaces
    host: 0.0.0.0
~~~~





- Novo build

cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t backstage-tentativa3 .
docker run -it -p 7007:7007 backstage-tentativa3

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ DOCKER_BUILDKIT=1 docker build -t backstage-tentativa3 .
[+] Building 97.4s (24/24) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                                                 0.0s
 => => transferring dockerfile: 38B                                                                                                                                                  0.0s
 => [internal] load .dockerignore                                                                                                                                                    0.0s
 => => transferring context: 34B                                                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/node:18.17.0-bullseye-slim                                                                                                        0.0s
 => [internal] load build context                                                                                                                                                    0.0s
 => => transferring context: 7.60kB                                                                                                                                                  0.0s
 => [internal] settings cache mount permissions                                                                                                                                      0.0s
 => [stage-2 1/7] FROM docker.io/library/node:18.17.0-bullseye-slim                                                                                                                  0.0s
 => CACHED [stage-2 2/7] RUN --mount=type=cache,target=/var/cache/apt,sharing=locked     --mount=type=cache,target=/var/lib/apt,sharing=locked     apt-get update &&     apt-get in  0.0s
 => CACHED [stage-2 3/7] WORKDIR /app                                                                                                                                                0.0s
 => CACHED [packages 2/6] WORKDIR /app                                                                                                                                               0.0s
 => CACHED [packages 3/6] COPY package.json yarn.lock ./                                                                                                                             0.0s
 => CACHED [packages 4/6] COPY packages packages                                                                                                                                     0.0s
 => CACHED [packages 5/6] COPY plugins plugins                                                                                                                                       0.0s
 => CACHED [packages 6/6] RUN find packages ! -name "package.json" -mindepth 2 -maxdepth 2 -exec rm -rf {} +                                                                         0.0s
 => CACHED [build 4/9] COPY --from=packages --chown=node:node /app .                                                                                                                 0.0s
 => CACHED [build 5/9] RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000     yarn install --frozen-lockfile --network-timeout 600000             0.0s
 => [build 6/9] COPY --chown=node:node . .                                                                                                                                           0.3s
 => [build 7/9] RUN yarn tsc                                                                                                                                                        16.6s
 => [build 8/9] RUN yarn --cwd packages/backend build                                                                                                                               70.4s
 => [build 9/9] RUN mkdir packages/backend/dist/skeleton packages/backend/dist/bundle     && tar xzf packages/backend/dist/skeleton.tar.gz -C packages/backend/dist/skeleton     &&  1.2s
 => CACHED [stage-2 4/7] COPY --from=build --chown=node:node /app/yarn.lock /app/package.json /app/packages/backend/dist/skeleton/ ./                                                0.0s
 => CACHED [stage-2 5/7] RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000     yarn install --frozen-lockfile --production --network-timeout 60  0.0s
 => CACHED [stage-2 6/7] COPY --from=build --chown=node:node /app/packages/backend/dist/bundle/ ./                                                                                   0.0s
 => [stage-2 7/7] COPY --chown=node:node app-config.yaml ./                                                                                                                          0.2s
 => exporting to image                                                                                                                                                               0.0s
 => => exporting layers                                                                                                                                                              0.0s
 => => writing image sha256:e81fe459c2841429df65810a4eac8b77397c15eae050bef2b57460ec2f7abc8b                                                                                         0.0s
 => => naming to docker.io/library/backstage-tentativa3                                                                                                                              0.0s
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

~~~~



- Nova imagem

~~~~bash

fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker image ls
REPOSITORY                                TAG                     IMAGE ID       CREATED              SIZE
backstage-tentativa3                      latest                  e81fe459c284   About a minute ago   1.01GB
<none>                                    <none>                  ab2bbae1faa9   2 hours ago          1.01GB
<none>                                    <none>                  6991084d6825   7 days ago           249MB
<none>                                    <none>                  7dbe71c18599   7 days ago           249MB
docker-compose_backstage-app-mandragora   latest                  ae1e7d59f1a5   7 days ago           1.1GB
<none>                                    <none>                  18f9cae4a259   7 days ago           197MB
node                                      18.17.0-bullseye-slim   eb0946b189e9   10 days ago          248MB
node                                      18.17                   0f9df951673d   10 days ago          1.09GB
docker-compose_app-teste-fusionist        latest                  af2a7918abb4   13 days ago          197MB
node                                      16-bullseye-slim        6b02cfd592ca   3 weeks ago          191MB
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

~~~~





- Executando container
docker run -it -p 7007:7007 backstage-tentativa3

- Testando acesso via browser no Notebook, sem sucesso.
http://192.168.0.110:7007/
http://192.168.0.110:3000/

- Nos logs do Container:

~~~~bash
{"documentType":"software-catalog","level":"info","message":"Collating documents for software-catalog succeeded","plugin":"search","service":"backstage","type":"plugin"}
{"documentType":"techdocs","level":"warn","message":"Index for techdocs was not created: indexer received 0 documents","plugin":"search","service":"backstage","type":"plugin"}
{"documentType":"techdocs","level":"info","message":"Collating documents for techdocs succeeded","plugin":"search","service":"backstage","type":"plugin"}
{"level":"info","message":"192.168.0.109 - - [29/Jul/2023:21:11:08 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36 Edg/115.0.1901.183\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"192.168.0.109 - - [29/Jul/2023:21:11:13 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36 Edg/115.0.1901.183\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"192.168.0.109 - - [29/Jul/2023:21:11:29 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"192.168.0.109 - - [29/Jul/2023:21:11:31 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0\"","service":"backstage","type":"incomingRequest"}
{"level":"info","message":"192.168.0.109 - - [29/Jul/2023:21:11:39 +0000] \"GET / HTTP/1.1\" 200 - \"-\" \"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36 Edg/115.0.1901.183\"","service":"backstage","type":"incomingRequest"}


~~~~


- Validando dentro do Container, a app-config pegou o endereço 0.0.0.0 na configuração, conforme esperado no build:

~~~~bash

fernando@debian10x64:~$
fernando@debian10x64:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                                       NAMES
44c71ab1f444   backstage-tentativa3   "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   0.0.0.0:7007->7007/tcp, :::7007->7007/tcp   sweet_nightingale
fernando@debian10x64:~$ docker exec -ti sweet_nightingale bash
node@44c71ab1f444:/app$ cat app-config.yaml | head -n 22
app:
  title: Scaffolded Backstage App
  baseUrl: http://localhost:3000

organization:
  name: My Company

backend:
  # Used for enabling authentication, secret is shared by all backend plugins
  # See https://backstage.io/docs/auth/service-to-service-auth for
  # information on the format
  # auth:
  #   keys:
  #     - secret: ${BACKEND_SECRET}
  baseUrl: http://localhost:7007
  listen:
    port: 7007
    # Uncomment the following host directive to bind to specific interfaces
    host: 0.0.0.0
  csp:
    connect-src: ["'self'", 'http:', 'https:']
    # Content-Security-Policy directives follow the Helmet format: https://helmetjs.github.io/#reference
node@44c71ab1f444:/app$ date
Sat Jul 29 21:14:39 UTC 2023
node@44c71ab1f444:/app$

~~~~





# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## PENDENTE

- Ler
https://roadie.io/blog/backstage-docker-service-catalog/
https://backstage.spotify.com/learn/standing-up-backstage/
https://backstage.io/docs/conf/writing/

- Efetuar commit e push da Docker image para o Docker Hub
- Tentativa de abrir "http://192.168.0.110:7007/" não funciona, somente via browser do Debian. Verificar porque nao abre via notebook.
      Listen para 0.0.0.0 não resolveu.
      Efetuar commit e push da Docker image para o Docker Hub, depois tentar expor via Kubernetes.
      Via Minikube "minikube service meu-ingress-controller-ingress-nginx-controller"	Expor um Service. OBS, Expõe o Service mas não gera um EXTERNAL-IP, é acessível na LAN da VM apenas.
      Ou via EKS-ingress.
      Avaliar maneira de abrir o Browser do Debian direto no Notebook.

- Efetuar build usando a instalação feita via container com node18.17, instalação tá na pasta "backstage/backup-fernando-instalacao".
- Ou instalação local via Debain mesmo, que está na pasta "backstage/docker/multi-stage/teste2/app-teste2".
- Utilizar o Multi-Stage na primeira tentativa:
        https://backstage.io/docs/deployment/docker/
        https://github.com/backstage/demo/blob/master/Dockerfile
- Ou tentar tutorial de 3 partes do blog:
        https://john-tucker.medium.com/backstage-by-example-part-1-a18e74849240

- Montar docker-compose com NodeJS semelhante a versão usada no doc sobre k8s da Backstage. Instalar o app do Backstage.
- Caso necessário, usar repo "kubedev" como apoio, sobre Dockerfile, NodeJS, etc.
- Buildar o APP do Backstage com estrutura via Docker-compose.
- TSHOOT, erro do yarn install travado durante criação do APP do Backstage via npx.
        https://backstage.io/docs/getting-started/create-an-app/
        issue:
        https://github.com/backstage/backstage/issues/18058
        Analisar:
        https://lightrun.com/answers/backstage-backstage-npx-backstagecreate-app-node-is-incompatible-with-this-module
- Buildar imagem Docker, após APP ficar OK.
- Personalizar "app-config.yaml"













# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 05/08/2023


- Novo build

cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t backstage-mandragora .
docker run -it -p 7007:7007 backstage-mandragora



- Efetuar commit e push da Docker image para o Docker Hub
docker commit <nome-ou-id-do-container> ubuntu-curl-commit	Imagem	Commit	Comando para fazer o docker commit, salvando o estado do Container numa imagem.

When you build them, using docker build -t <hub-user>/<repo-name>[:<tag>]

By re-tagging an existing local image docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]

By using docker commit <existing-container> <hub-user>/<repo-name>[:<tag>] to commit changes

Now you can push this repository to the registry designated by its name or tag.

$ docker push <hub-user>/<repo-name>:<tag>
The image is then uploaded and available for use by your teammates and/or the community.


docker tag backstage-mandragora fernandomj90/backstage-mandragora:v2

fernando@debian10x64:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                                       NAMES
59b6a7c41968   backstage-mandragora   "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes   0.0.0.0:7007->7007/tcp, :::7007->7007/tcp   angry_gates
fernando@debian10x64:~$

docker commit 59b6a7c41968 fernandomj90/backstage-mandragora:v3


fernando@debian10x64:~$ docker image ls
REPOSITORY                           TAG                     IMAGE ID       CREATED         SIZE
fernandomj90/backstage-mandragora    v3                      df229c7d8995   4 seconds ago   1.01GB
fernandomj90/backstage-mandragora    v2                      e81fe459c284   6 days ago      1.01GB
backstage-mandragora                 latest                  e81fe459c284   6 days ago      1.01GB
backstage-tentativa3                 latest                  e81fe459c284   6 days ago      1.01GB
<none>                               <none>                  ab2bbae1faa9   7 days ago      1.01GB
<none>                               <none>                  6991084d6825   13 days ago     249MB
<none>                               <none>                  7dbe71c18599   13 days ago     249MB
<none>                               <none>                  18f9cae4a259   2 weeks ago     197MB
node                                 18.17.0-bullseye-slim   eb0946b189e9   2 weeks ago     248MB
node                                 18.17                   0f9df951673d   2 weeks ago     1.09GB
docker-compose_app-teste-fusionist   latest                  af2a7918abb4   2 weeks ago     197MB
node                                 16-bullseye-slim        6b02cfd592ca   4 weeks ago     191MB
gcr.io/k8s-minikube/kicbase          v0.0.27                 9fa1cc16ad6d   22 months ago   1.08GB
fernando@debian10x64:~$ date
Sat 05 Aug 2023 04:38:20 PM -03
fernando@debian10x64:~$


docker push <hub-user>/<repo-name>:<tag>
docker push fernandomj90/backstage-mandragora:v3


fernando@debian10x64:~$ docker login
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /home/fernando/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
fernando@debian10x64:~$


docker push fernandomj90/backstage-mandragora:v3




fernando@debian10x64:~$ docker push fernandomj90/backstage-mandragora:v3
The push refers to repository [docker.io/fernandomj90/backstage-mandragora]
33a6ee2549f9: Pushed
db4157ce807b: Pushed
ccf42828e54f: Pushed
f7d9559dab6e: Pushed
ba18ff0e8af7: Pushed
7e4519697b19: Pushed
ac4716367973: Pushed
4b497447f3f1: Mounted from library/node
98342cba36b0: Mounted from library/node
ec66eb809bcb: Mounted from library/node
de28499355cf: Mounted from library/node
4b3ba104e9a8: Mounted from library/node
v3: digest: sha256:06c7adeafea135d68e676af14a7c88baa8af93076484f208885144c469f685cc size: 2839
fernando@debian10x64:~$







# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 06/08/2023


- Novo build
ajustado baseurl
baseUrl: http://k8s-backstag-backstag-c3e6f62e16-370703358.us-east-1.elb.amazonaws.com:7007

cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-mandragora:v4 .
docker push fernandomj90/backstage-mandragora:v4


~~~~bash

                                                                                 0.0s
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker image ls
REPOSITORY                           TAG                     IMAGE ID       CREATED         SIZE
fernandomj90/backstage-mandragora    v4                      ccfb27d03cb7   5 minutes ago   1.01GB
fernandomj90/backstage-mandragora    v3                      df229c7d8995   29 hours ago    1.01GB
backstage-tentativa3                 latest                  e81fe459c284   8 days ago      1.01GB
fernandomj90/backstage-mandragora    v2                      e81fe459c284   8 days ago      1.01GB
backstage-mandragora                 latest                  e81fe459c284   8 days ago      1.01GB
<none>                               <none>                  ab2bbae1faa9   8 days ago      1.01GB
<none>                               <none>                  6991084d6825   2 weeks ago     249MB
<none>                               <none>                  7dbe71c18599   2 weeks ago     249MB
<none>                               <none>                  18f9cae4a259   2 weeks ago     197MB
node                                 18.17.0-bullseye-slim   eb0946b189e9   2 weeks ago     248MB
node                                 18.17                   0f9df951673d   2 weeks ago     1.09GB
docker-compose_app-teste-fusionist   latest                  af2a7918abb4   3 weeks ago     197MB
node                                 16-bullseye-slim        6b02cfd592ca   4 weeks ago     191MB
gcr.io/k8s-minikube/kicbase          v0.0.27                 9fa1cc16ad6d   22 months ago   1.08GB


fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker push fernandomj90/backstage-mandragora:v4
The push refers to repository [docker.io/fernandomj90/backstage-mandragora]
1bf075bdc031: Pushed
82bd28422cf7: Pushed
f7d9559dab6e: Layer already exists
ba18ff0e8af7: Layer already exists
7e4519697b19: Layer already exists
ac4716367973: Layer already exists
4b497447f3f1: Layer already exists
98342cba36b0: Layer already exists
ec66eb809bcb: Layer already exists
de28499355cf: Layer already exists
4b3ba104e9a8: Layer already exists
v4: digest: sha256:11a42e4efc6d4fa22e2996deedc21d2cb14ca883b2d76ad1a335a9650662c19f size: 2629
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

~~~~


- Upload ok
https://hub.docker.com/repository/docker/fernandomj90/backstage-mandragora/general
<https://hub.docker.com/repository/docker/fernandomj90/backstage-mandragora/general>













# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 12/08/2023

- Novo build
ajustado baseurl
baseUrl: http://k8s-backstag-backstag-c3e6f62e16-370703358.us-east-1.elb.amazonaws.com:7007

cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-mandragora:v5 .
docker push fernandomj90/backstage-mandragora:v5


- Ad








cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-mandragora:v6 .
docker push fernandomj90/backstage-mandragora:v6





# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
# ####################################################################################################################################################
## Dia 19/08/2023

- Novo build
ajustado baseurl
baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com:7007

fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$ kubectl get ingress --namespace=backstage
NAME                CLASS    HOSTS   ADDRESS                                                                   PORTS   AGE
backstage-ingress   <none>   *       k8s-backstag-backstag-c3e6f62e16-1355060445.us-east-1.elb.amazonaws.com   80      4m12s
fernando@debian10x64:~/cursos/idp-devportal/backstage/manifestos-k8s$


cd ~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3
DOCKER_BUILDKIT=1 docker build -t fernandomj90/backstage-mandragora:latest .
docker push fernandomj90/backstage-mandragora:latest


- Push efetuado

~~~~bash
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ docker push fernandomj90/backstage-mandragora:latest
The push refers to repository [docker.io/fernandomj90/backstage-mandragora]
69e33c9b537a: Pushed
08ad892b6ee3: Pushed
f7d9559dab6e: Layer already exists
ba18ff0e8af7: Layer already exists
7e4519697b19: Layer already exists
ac4716367973: Layer already exists
4b497447f3f1: Layer already exists
98342cba36b0: Layer already exists
ec66eb809bcb: Layer already exists
de28499355cf: Layer already exists
4b3ba104e9a8: Layer already exists
latest: digest: sha256:eb7a6043f437c672f8ee79993749a5e1922aa16189f3760c231406f3c0abad39 size: 2629
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$ date
Sat 19 Aug 2023 09:02:31 PM -03
fernando@debian10x64:~/cursos/idp-devportal/backstage/docker/multi-stage/tentativa3$

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