# Imagem OK:
#FROM node:14.15.4
FROM node:16-bullseye-slim
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
