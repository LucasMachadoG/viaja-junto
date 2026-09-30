FROM node:22-alpine AS build
WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
# O prisma.config.ts exige DATABASE_URL, mas o generate não conecta no banco
RUN DATABASE_URL="postgresql://build:build@localhost:5432/build" npx prisma generate \
  && npm run build

FROM node:22-alpine AS prod-deps
WORKDIR /app

COPY package.json package-lock.json ./
# O @prisma/client declara `prisma` (CLI) como peer opcional; como ele está nas devDependencies,
# o lockfile o marca como devOptional e `--omit=dev` sozinho não o remove (nem studio-core,
# mysql2, typescript etc.). `--omit=optional` tira esses pacotes; o client gerado não precisa da CLI.
RUN npm ci --omit=dev --omit=optional

FROM node:22-alpine
ENV NODE_ENV=production
WORKDIR /app

# npm, yarn e corepack não são usados em runtime e só aumentam a superfície de CVEs
RUN rm -rf /usr/local/lib/node_modules/npm /usr/local/lib/node_modules/corepack \
  /usr/local/bin/npm /usr/local/bin/npx /usr/local/bin/corepack \
  /usr/local/bin/yarn /usr/local/bin/yarnpkg /opt/yarn-*

COPY --from=prod-deps --chown=node:node /app/package.json ./
COPY --from=prod-deps --chown=node:node /app/node_modules ./node_modules
COPY --from=build --chown=node:node /app/dist ./dist

USER node
EXPOSE 3000
CMD ["node", "dist/src/main.js"]
