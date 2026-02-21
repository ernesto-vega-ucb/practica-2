FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install && npm audit fix --force

COPY --chown=node:node . .

RUN rm -rf node_modules/cross-spawn && npm install cross-spawn@7.0.6

USER node

EXPOSE 3000
CMD ["node", "src/server.js"]
