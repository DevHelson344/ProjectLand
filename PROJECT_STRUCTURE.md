# Estrutura do Projeto

## Visão Geral

Este projeto está organizado em duas aplicações principais: frontend e backend.

## Frontend (React + Vite + TypeScript)

```
frontend/
├── src/
│   ├── components/      # Componentes React reutilizáveis
│   ├── pages/          # Páginas da aplicação
│   ├── services/       # Serviços de API e lógica de negócio
│   ├── hooks/          # Custom React hooks
│   ├── contexts/       # Context API providers
│   ├── utils/          # Funções utilitárias
│   ├── types/          # Definições de tipos TypeScript
│   ├── test/           # Configuração e utilitários de teste
│   ├── App.tsx         # Componente principal
│   ├── main.tsx        # Entry point
│   └── index.css       # Estilos globais (Tailwind)
├── public/             # Arquivos estáticos
├── .env                # Variáveis de ambiente
├── .env.example        # Exemplo de variáveis de ambiente
├── vite.config.ts      # Configuração do Vite e Vitest
├── tailwind.config.js  # Configuração do Tailwind CSS
├── postcss.config.js   # Configuração do PostCSS
├── tsconfig.json       # Configuração do TypeScript
└── package.json        # Dependências e scripts
```

### Scripts Disponíveis

- `npm run dev` - Inicia o servidor de desenvolvimento
- `npm run build` - Compila para produção
- `npm test` - Executa os testes com Vitest
- `npm run test:watch` - Executa os testes em modo watch
- `npm run test:ui` - Abre a interface de testes do Vitest
- `npm run test:coverage` - Gera relatório de cobertura

## Backend (Node.js + Express + TypeScript)

```
backend/
├── src/
│   ├── controllers/    # Controllers da API
│   ├── services/       # Lógica de negócio
│   ├── repositories/   # Acesso a dados
│   ├── models/         # Modelos de dados
│   ├── middleware/     # Middlewares Express
│   ├── routes/         # Definição de rotas
│   ├── config/         # Configurações
│   ├── utils/          # Funções utilitárias
│   ├── __tests__/      # Testes
│   └── index.ts        # Entry point
├── dist/               # Código compilado (gerado)
├── .env                # Variáveis de ambiente
├── .env.example        # Exemplo de variáveis de ambiente
├── tsconfig.json       # Configuração do TypeScript
├── jest.config.js      # Configuração do Jest
└── package.json        # Dependências e scripts
```

### Scripts Disponíveis

- `npm run dev` - Inicia o servidor em modo desenvolvimento com hot reload
- `npm run build` - Compila TypeScript para JavaScript
- `npm start` - Inicia o servidor em produção
- `npm test` - Executa os testes com Jest
- `npm run test:watch` - Executa os testes em modo watch
- `npm run test:coverage` - Gera relatório de cobertura

## Dependências Principais

### Frontend
- **React 19** - Biblioteca UI
- **Vite** - Build tool e dev server
- **Tailwind CSS 4** - Framework CSS
- **TypeScript** - Tipagem estática
- **Vitest** - Framework de testes
- **@testing-library/react** - Utilitários de teste
- **fast-check** - Property-based testing

### Backend
- **Express 5** - Framework web
- **PostgreSQL (pg)** - Cliente de banco de dados
- **JWT (jsonwebtoken)** - Autenticação
- **bcrypt** - Hash de senhas
- **TypeScript** - Tipagem estática
- **Jest** - Framework de testes
- **Supertest** - Testes de API
- **fast-check** - Property-based testing

## Configuração Inicial

### 1. Backend

```bash
cd backend
npm install
cp .env.example .env
# Edite o .env com suas configurações
npm run dev
```

### 2. Frontend

```bash
cd frontend
npm install
cp .env.example .env
# Edite o .env com suas configurações
npm run dev
```

### 3. Banco de Dados

Configure o PostgreSQL e atualize as variáveis de ambiente no `.env` do backend:

```
DATABASE_URL=postgresql://user:password@localhost:5432/landing_page_builder
```

## Próximos Passos

Siga o plano de implementação em `.kiro/specs/landing-page-builder/tasks.md` para desenvolver as funcionalidades do sistema.
