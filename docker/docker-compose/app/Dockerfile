# Imagem OK:
#FROM node:14.15.4
#FROM node:16-bullseye-slim
FROM node:18.17
WORKDIR /
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
