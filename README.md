# ViajaJunto

Boilerplate inicial do projeto, com front-end e back-end separados.

**Stack**

- Front-end: React + Vite + TypeScript
- Back-end: Node.js + NestJS + TypeScript
- ORM: Prisma
- Banco de dados: PostgreSQL
- Infra local: Docker Compose

## Estrutura

```
/
  frontend/   -> aplicação React (Vite)
  backend/    -> API NestJS
  docs/       -> documentação e modelagem (dbdiagram, Astah, etc.)
  docker-compose.yml
```

## Pré-requisitos

> **Obrigatório:** Node.js `>= 22.13.0`. Versões anteriores (incluindo `22.11.0` e `22.12.0`) não são compatíveis com dependências atuais do projeto (Vite 8 no front-end, Prisma 7 no back-end).

- Node.js >= 22.13.0
- Docker / Docker Compose
- npm

O projeto define a versão exigida em `frontend/.nvmrc` e `backend/.nvmrc`. Se você usa [nvm](https://github.com/nvm-sh/nvm), rode `nvm use` dentro de cada pasta (`frontend/` ou `backend/`) para carregar automaticamente a versão correta do Node.

## 1. Subir o PostgreSQL com Docker

Na raiz do repositório:

```bash
cp .env.example .env
docker compose up -d
```

Isso sobe um container PostgreSQL com os dados persistidos em um volume Docker.

## 2. Configurar e executar o back-end

Certifique-se de estar usando uma versão de Node compatível (ver `backend/.nvmrc`). Com `nvm` instalado:

```bash
cd backend
nvm use
cp .env.example .env
npm install
npx prisma generate
npm run start:dev
```

A API sobe por padrão em `http://localhost:3000`.

Scripts úteis:

```bash
npm run build   # compila o projeto
npm run lint    # roda o ESLint
npm run test    # roda os testes
```

Quando os primeiros models forem definidos em `backend/prisma/schema.prisma`, use:

```bash
npx prisma migrate dev
```

para criar e aplicar migrations.

## 3. Configurar e executar o front-end

Certifique-se de estar usando uma versão de Node compatível (ver `frontend/.nvmrc`). Com `nvm` instalado:

```bash
cd frontend
nvm use
cp .env.example .env
npm install
npm run dev
```

A aplicação sobe por padrão em `http://localhost:5173`.

Scripts úteis:

```bash
npm run build    # compila o projeto
npm run lint     # roda o oxlint
npm run format   # formata o código com Prettier
```

## Deploy do front-end (Vercel)

O front-end está pronto para ser publicado na Vercel (build padrão do Vite), mas nenhum deploy ou configuração de conta foi feito neste momento.

## Observações

Este repositório contém apenas o boilerplate inicial dos projetos (estrutura de pastas, configuração de ferramentas e infraestrutura local). Nenhuma funcionalidade, regra de negócio ou tela da aplicação foi implementada ainda.
