FROM node:20.19.0-alpine3.21 AS test
WORKDIR /app

RUN npm install -g npm@10.9.7

COPY package*.json ./
RUN npm ci
COPY src ./src
COPY test ./test
RUN npm test

FROM node:20.19.0-alpine3.21 AS production
WORKDIR /app
ENV NODE_ENV=production

RUN npm install -g npm@10.9.7

COPY package*.json ./
RUN npm ci --omit=dev
COPY src ./src
USER node
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s --retries=3 CMD wget -qO- http://127.0.0.1:3000/health >/dev/null || exit 1
CMD ["node", "src/index.js"]