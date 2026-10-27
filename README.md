# Yawara Platform & Y-SNA (Yawara System Neural Architecture)

Monorepo da plataforma tecnológica do **Projeto Yawara**, integrando o portal institucional, o sistema de gestão de processos seletivos e o **Y-SNA (Yawara System Neural Architecture)** — o motor de inteligência artificial responsável pela ingestão, triagem preditiva e análise de perfil dos candidatos aos núcleos de engenharia.

---

## 🏛 Arquitetura do Monorepo

O projeto é estruturado em formato de monorepo, separando as aplicações executáveis (`apps/`) das bibliotecas e contratos de tipagem compartilhados (`packages/`):

```text
yawara/
├── apps/
│   ├── frontend/                 # Aplicação web principal (Next.js 15 App Router)
│   │   ├── public/               # Ativos estáticos, logos, ícones e documentações
│   │   ├── src/
│   │   │   ├── app/              # Rotas públicas, autenticação, portal e área restrita (/in)
│   │   │   ├── components/       # Componentes React (PS, Auth, Núcleos, Certificados)
│   │   │   ├── hooks/            # Hooks customizados para gestão de estado e dados
│   │   │   ├── lib/              # Repositórios, serviços (API, DB, Redis, Cloudinary)
│   │   │   ├── styles/           # Estilos globais e modulares em SCSS
│   │   │   └── middleware.ts     # Proteção perimetral de rotas e validação de sessão
│   │   ├── next.config.ts
│   │   └── package.json
│   │
│   └── yawara-ysna/              # Motor neural de IA para processamento de candidatos (FastAPI)
│       ├── app/
│       │   ├── api/v1/           # Endpoints de predição, relatórios e calibração
│       │   ├── core/             # Configurações, segurança, agendadores e ontologias
│       │   ├── ml/               # Modelos PyTorch, Valence Engine, Canonical Engine e XAI
│       │   ├── resources/        # Datasets sintéticos, embeddings e labels treinados
│       │   ├── schemas/          # Modelos Pydantic para I/O
│       │   ├── services/         # Ingestão de PDFs, orquestração e geração de relatórios
│       │   └── utils/            # Parsers de históricos acadêmicos, métricas e banco
│       ├── docs/                 # Documentação técnica detalhada do modelo
│       ├── Dockerfile
│       └── requirements.txt
│
├── packages/
│   └── types/                    # Pacote compartilhado de tipos TypeScript
│       ├── auth/                 # Credenciais, sessão e controle de acesso
│       ├── bd/                   # Interfaces de banco de dados e RPC
│       ├── certificate/          # Modelos de certificados e validação
│       ├── nuclei/               # Estruturas dos núcleos técnicos
│       ├── ps/                   # Modelos de candidatos, desafios e etapas do PS
│       ├── ysna/                 # Contratos de integração com a IA
│       └── user/                 # Tipos de perfil e permissões
│
├── docker-compose.yml            # Orquestração de todos os serviços (Frontend, Y-SNA, Redis, DB)
├── package.json                  # Gerenciador de workspaces da raiz
└── tsconfig.json                 # Configuração base de TypeScript
```

---

## 🧠 Y-SNA (Yawara System Neural Architecture)

O **Y-SNA** é o núcleo de inteligência artificial desenvolvido em Python para automatizar e otimizar o Processo Seletivo do Projeto Yawara:

* **Ingestão Automatizada de Históricos:** Extração e normalização de disciplinas, notas, cargas horárias e rendimento acadêmico diretamente de históricos em PDF (suporte a layouts universitários como UFGD).
* **Canonical Subject Engine:** Mapeamento ontológico de nomenclaturas de matérias heterogêneas para conceitos canônicos de engenharia via redes neurais e similaridade semântica.
* **Valence Model Engine:** Modelo neural que calcula o vetor de valência e afinidade do candidato para cada núcleo técnico (Mecânica, Combustão, Sistemas Embarcados, Aerodesign, Gestão e Tecnologias Sustentáveis).
* **XAI (Explainable AI):** Geração de explicações determinísticas e baseadas em ativações neurais para justificar a pontuação aos avaliadores técnicos.
* **Relatórios Automatizados:** Emissão de pareceres técnicos em HTML/PDF com gráficos e score detalhado por competência.

---

## ⚡ Tecnologias Utilizadas

### Frontend & Plataforma Web
* **Framework:** [Next.js](https://nextjs.org/) (App Router, Server Actions e Route Handlers)
* **Linguagem:** [TypeScript](https://www.typescriptlang.org/) (Tipagem estrita de ponta a ponta)
* **Estilização:** SCSS Modules (`.module.scss`) com sistema centralizado de mixins e temas
* **Sessão & Cache:** [Redis](https://redis.io/) para controle volátil de autenticação e rate limiting
* **Mídia & Uploads:** [Cloudinary](https://cloudinary.com/) para documentos e portfólios

### Inteligência Artificial & Backend (Y-SNA)
* **Framework:** [FastAPI](https://fastapi.tiangolo.com/) com Uvicorn
* **Deep Learning:** [PyTorch](https://pytorch.org/) (redes neurais canônicas, valência e recomendação)
* **Processamento de Dados:** NumPy, Pandas, Scikit-learn
* **Extração Documental:** PyMuPDF / pdfplumber para leitura de históricos acadêmicos
* **Orquestração de Tarefas:** QStash / Cron tasks

### Infraestrutura
* **Containerização:** [Docker](https://www.docker.com/) e Docker Compose
* **Banco de Dados:** Conector SQL/RPC customizado

---

## 🎯 Principais Módulos da Aplicação

### 1. Portal Institucional & Núcleos
* Apresentação dos núcleos de engenharia e projetos ativos.
* Seções de inovação, transparência, patrocinadores e sustentabilidade.

### 2. Processo Seletivo (PS)
* Landing page e cronograma com inscrições guiadas via `CardFlow`.
* Upload e validação de documentação acadêmica integrada ao pipeline do Y-SNA.
* Simulador e preditor de pontuação (`score-predictor`).

### 3. Painel Administrativo & Membro (`/in`)
* **Dashboard do Candidato:** Acompanhamento de status da inscrição, avisos dinâmicos (`DynamicAlertCard`) e desafios técnicos.
* **Gestão de Equipes (`my-team`):** Criação e aprovação de ARTs e ARTTCs, quadro de tarefas e avisos internos.
* **Gestão do Processo Seletivo:** Painel de notas, relatórios gerados pelo Y-SNA e aprovação de candidatos.
* **Emissão e Validação de Certificados:** Geração de documentos com validação pública via QR Code e código verificador.
* **Alocação de Ferramentas:** Controle de empréstimo e devolução de ferramentas da oficina.

---

## 🔧 Configuração de Ambiente

### 1. Frontend (`apps/frontend/.env.local`)

Crie o arquivo a partir do exemplo:

```bash
cp apps/frontend/.env.example apps/frontend/.env.local
```

Preencha as variáveis principais:

```env
# Aplicação
NEXT_PUBLIC_APP_URL=http://localhost:3000
NODE_ENV=development

# Autenticação
JWT_SECRET=sua_chave_jwt_secreta_longa_e_segura
SESSION_COOKIE_NAME=yawara_session
SESSION_EXPIRATION_SECONDS=86400

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# Banco de Dados
DATABASE_URL=postgresql://usuario:senha@localhost:5432/yawara_db

# Cloudinary
CLOUDINARY_CLOUD_NAME=seu_cloud_name
CLOUDINARY_API_KEY=sua_api_key
CLOUDINARY_API_SECRET=seu_api_secret

# Integração Y-SNA
YSNA_API_URL=http://localhost:8000/api/v1
YSNA_API_KEY=sua_chave_de_comunicacao_ysna
```

### 2. Y-SNA (`apps/yawara-ysna/.env`)

```env
API_V1_STR=/api/v1
PROJECT_NAME="Yawara System Neural Architecture"
ENVIRONMENT=development
PORT=8000

# Segurança
YSNA_SECRET_KEY=sua_chave_secreta_ysna
ACCESS_TOKEN_EXPIRE_MINUTES=1440

# Banco de Dados / Redis
DATABASE_URL=postgresql://usuario:senha@localhost:5432/yawara_db
REDIS_URL=redis://localhost:6379/0
```

---

## 🚀 Instalação e Execução

### Opção 1: Execução com Docker Compose (Recomendada)

Para subir o frontend, o Y-SNA e os serviços de apoio (Redis e Banco de Dados) de forma orquestrada:

```bash
docker compose up --build
```

* **Frontend:** `http://localhost:3000`
* **API Y-SNA:** `http://localhost:8000/api/v1`
* **Docs Swagger (Y-SNA):** `http://localhost:8000/docs`

Para parar todos os containers:

```bash
docker compose down
```

---

### Opção 2: Desenvolvimento Local

#### 1. Instalar dependências da raiz (Monorepo Node)
```bash
npm install
```

#### 2. Iniciar serviços auxiliares (Docker)
```bash
docker compose up -d redis
```

#### 3. Iniciar o Frontend
```bash
npm run dev --workspace=apps/frontend
```

#### 4. Iniciar o Y-SNA (Python)
Em outro terminal:
```bash
cd apps/yawara-ysna
python -m venv .venv
source .venv/bin/activate   # No Windows: .venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

---

## 🧪 Testes

### Testes do Frontend (Vitest / Testes de Integração)
```bash
npm run test --workspace=apps/frontend
```

### Testes do Motor Y-SNA (Pytest)
```bash
cd apps/yawara-ysna
pytest
```

---

## 🔒 Segurança e Boas Práticas

* **Middleware Next.js:** Interceptação perimetral de requisições para rotas `/in/*` com conferência de sessão no Redis e atribuição de papéis (`roles`).
* **Validação de Schemas:** Validação de entradas no frontend com schemas em `src/lib/validations/` e no Y-SNA através de modelos Pydantic.
* **Tratamento de Arquivos:** Ingestão de arquivos com análise de MIME-type e upload com sanitização para armazenamento em nuvem.
