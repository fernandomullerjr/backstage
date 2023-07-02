


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




https://hub.docker.com/layers/library/node/16-bullseye-slim/images/sha256-4126251961c4619854e56de80e352b8962443d4d32d5a206ac2ed82b5592f5d1?context=explore