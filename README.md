Yawara Platform

Monorepo da plataforma web do Projeto Yawara, integrando o portal institucional, o sistema de gestão de processos seletivos e a área autenticada de membros e candidatos. A arquitetura é construída com Next.js (App Router), TypeScript e bibliotecas compartilhadas em modelo de monorepo.

🏛 Arquitetura do Repositório

O projeto adota uma estrutura monorepo dividida entre aplicações finais (apps/) e bibliotecas compartilhadas de tipos e utilitários (packages/):

yawara-main/
├── apps/
│   └── frontend/                # Aplicação principal em Next.js
│       ├── public/              # Ativos estáticos (ícones dos núcleos, imagens, logos)
│       ├── src/
│       │   ├── app/             # Rotas do App Router (auth, content, protected, api)
│       │   ├── components/      # Componentes reutilizáveis (home, auth, account, etc.)
│       │   ├── hooks/           # React Hooks customizados (usePs, useCard, etc.)
│       │   ├── lib/             # Camada de serviços, repositórios e conexões
│       │   ├── mocks/           # Dados mockados para testes e desenvolvimento
│       │   ├── styles/          # Estilos globais, mixins e variáveis SCSS
│       │   ├── utils/           # Funções utilitárias (tokens, cookies, db helpers)
│       │   └── middleware.ts    # Proteção de rotas e validação de sessão
│       ├── next.config.ts
│       └── package.json
├── packages/
│   └── types/                   # Tipagens TypeScript compartilhadas
│       ├── art/                 # Definições de arte e layouts
│       ├── arttc/               # Tipagens adicionais de componentes
│       ├── auth/                # Tipos de credenciais, login e sessão
│       ├── bd/                  # Interfaces de banco de dados e RPC
│       ├── cloudinary/          # Tipos para upload de mídia
│       ├── layout/              # Tipos de estruturas de layout e navegação
│       ├── my-account/          # Dados do perfil do usuário
│       ├── protect/             # Tipagens de controle de acesso e proteção
│       ├── ps/                  # Entidades do Processo Seletivo
│       ├── responses/           # Formato de resposta padronizado da API
│       └── user/                # Modelos de usuário
├── docker-compose.yml           # Orquestração de serviços auxiliares e aplicação
├── package.json                 # Configurações do workspace raiz
└── tsconfig.json                # Configuração base de TypeScript


⚡ Tecnologias Utilizadas

Framework Web: Next.js (App Router, React Server Components & Server Actions)

Linguagem: TypeScript (Tipagem estrita ponta a ponta)

Estilização: SCSS Modules (.module.scss) com sistema centralizado de variáveis e mixins

Cache & Sessão: Redis para controle de sessões ativas e rate limiting

Banco de Dados: Conector SQL/RPC customizado com repositórios e serviços desacoplados

Armazenamento de Mídia: Cloudinary para upload e gestão de arquivos do processo seletivo

Containerização: Docker e Docker Compose

🎯 Principais Funcionalidades

1. Portal Institucional & Núcleos

Apresentação dos núcleos de atuação: Mecânica, Combustão, Sistemas Embarcados, Aerodesign, Gestão e Tecnologias Sustentáveis.

Seções informativas sobre projetos, sustentabilidade e impacto institucional.

2. Sistema de Processo Seletivo (PS)

Landing page de inscrição com cronograma e etapas.

Fluxo de inscrição guiado por etapas (CardFlow) com validação de dados em tempo real.

Upload seguro de documentos e comprovações acadêmicas via Cloudinary.

Validação e conferência de inscrições.

3. Painel do Candidato / Membro (/account)

Dashboard personalizado com status da candidatura e avisos dinâmicos (DynamicAlertCard).

Gestão cadastral de perfil, dados acadêmicos e preferências.

Central de documentos com verificação de autenticidade.

4. Autenticação & Segurança

Fluxo completo de cadastro, confirmação de conta, login e recuperação de senha.

Sessões protegidas via cookies HttpOnly com validação no Redis.

Interceptação e proteção de rotas privadas via Next.js Middleware.

📋 Pré-requisitos

Node.js: versão 20.x ou superior recomendada

Gerenciador de pacotes: npm, pnpm ou yarn

Docker & Docker Compose: para execução dos serviços em containers

🔧 Configuração de Ambiente

Crie o arquivo de variáveis de ambiente na aplicação frontend:

cp apps/frontend/.env.example apps/frontend/.env.local


Preencha as variáveis de ambiente necessárias:

# Aplicação
NEXT_PUBLIC_APP_URL=http://localhost:3000
NODE_ENV=development

# Autenticação & Sessão
JWT_SECRET=sua_chave_secreta_jwt_longa_e_aleatoria
SESSION_COOKIE_NAME=yawara_session
SESSION_EXPIRATION_SECONDS=86400

# Redis (Sessões e Cache)
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# Banco de Dados
DATABASE_URL=postgresql://usuario:senha@localhost:5432/yawara_db

# Cloudinary (Armazenamento de Documentos)
CLOUDINARY_CLOUD_NAME=seu_cloud_name
CLOUDINARY_API_KEY=sua_api_key
CLOUDINARY_API_SECRET=seu_api_secret


🚀 Instalação e Execução

Modo de Desenvolvimento Local

Instale todas as dependências da raiz e dos workspaces:

npm install


Suba os serviços auxiliares (Redis/Banco) com Docker:

docker compose up -d redis


Inicie a aplicação frontend:

npm run dev --workspace=apps/frontend


A aplicação estará disponível em http://localhost:3000.

Execução via Docker Compose

Para subir a aplicação completa e as dependências em containers isolados:

docker compose up --build


Para encerrar a execução:

docker compose down


🛠 Scripts Disponíveis

Na raiz do repositório:

npm run dev: Inicia o ambiente de desenvolvimento nos workspaces configurados.

npm run build: Compila os pacotes e gera o build de produção do Next.js.

npm run lint: Executa a verificação estática de código via ESLint.

npm run test: Executa os testes automatizados da suíte Jest/Vitest.

No diretório apps/frontend:

npm run dev: Inicia o servidor Next.js em modo de desenvolvimento (next dev).

npm run build: Cria a versão otimizada para produção (next build).

npm run start: Inicia o servidor com o build gerado (next start).

npm run lint: Executa o linter no código do frontend.

🔒 Segurança e Boas Práticas

Proteção de Rotas: Toda navegação em áreas autenticadas (/account/*) passa pelo middleware.ts, impedindo acesso de usuários não logados.

Validação de Entrada: Todas as submissões de formulário e rotas da API em apps/frontend/src/app/api/ são validadas através de schemas centralizados em src/lib/validations/.

Persistência Segura: Nenhuma operação no banco executa comandos destrutivos diretos sem verificação de identidade e escopo de permissão.
