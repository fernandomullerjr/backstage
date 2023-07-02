# Use a imagem base do Node.js
FROM node:16-bullseye-slim
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