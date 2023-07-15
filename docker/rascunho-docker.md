





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