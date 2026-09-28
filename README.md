
# 🩺 Check API

API REST para registrar, consultar e gerenciar verificações ("checks") de endpoints externos, funcionando como um pequeno monitor de disponibilidade de serviços.

Acompanha um frontend em React onde é possível cadastrar rotas, editar parâmetros e testar requisições direto no navegador, sem precisar de Postman.

---

## 🚀 Funcionalidades

**🩺 Saúde da API**

- Endpoint de health check para confirmar que a API está no ar
- Resposta com status, timestamp e nome do serviço

**📡 Gestão de checks**

- Criar checks com nome, URL, método HTTP, intervalo e timeout
- Listar todos os checks e consultar um check por ID
- Atualizar e remover checks existentes
- Ativar ou desativar um check

**📊 Dashboard**

- Resumo com total de checks, checks ativos, falhas e uptime

**🖥️ Frontend**

- Barra lateral com as rotas da API
- Editor de requisições e painel de resposta
- Testes de endpoints direto no navegador

---

## 📡 Endpoints

- `GET /api/health` — verifica se a API está funcionando
- `GET /api/dashboard/summary` — resumo do dashboard
- `POST /api/checks` — cria um novo check
- `GET /api/checks` — lista todos os checks
- `GET /api/checks/:id` — retorna um check específico
- `PUT /api/checks/:id` — atualiza um check
- `DELETE /api/checks/:id` — remove um check

---

## 🛠️ Tecnologias

**Backend**

- NestJS 10
- TypeScript
- Node.js 18+

**Frontend**

- React 18
- Vite
- TypeScript

---

## 🏗️ Decisões Técnicas

- Módulos separados por responsabilidade: health, checks e dashboard
- Prefixo global `/api` em todas as rotas
- Dados armazenados em memória, para manter o MVP simples
- CORS habilitado com `app.enableCors()` para o frontend acessar a API em outra porta
- Chamadas HTTP centralizadas no arquivo `services/api.ts`
- URL da API configurável pela variável `VITE_API_BASE`

---

## 📁 Estrutura do Projeto

**Backend (pasta `backend/src/`)**

- `main.ts` — inicia o NestJS e habilita o CORS
- `app.module.ts` — registra os módulos da aplicação
- `common/` — enums e types compartilhados
- `modules/checks/` — criação, listagem, atualização e remoção dos checks
- `modules/dashboard/` — resumo do estado do dashboard
- `modules/health/` — verificação de saúde da API

**Frontend (pasta `frontend/src/`)**

- `main.tsx` — entrada da aplicação React
- `App.tsx` — tela principal do MVP
- `components/RequestEditor.tsx` — editor de requisições
- `components/ResponsePanel.tsx` — painel de resposta
- `components/Sidebar.tsx` — barra lateral
- `services/api.ts` — cliente para consumo da API

---

## ▶️ Como Executar

**Pré-requisitos**

- Node.js 18 ou superior
- npm 9 ou superior

**Rodando o backend**

1. Entre na pasta: `cd backend`
2. Instale as dependências: `npm install`
3. Inicie a API: `npm run start:dev`
4. Acesse: http://localhost:3000/api

Opcional: crie um arquivo `.env` na pasta `backend` com `PORT=3000` para definir a porta. Sem ele, a API já usa a 3000.

**Rodando o frontend**

1. Em outro terminal, entre na pasta: `cd frontend`
2. Instale as dependências: `npm install`
3. Inicie a interface: `npm run dev`
4. Acesse: http://localhost:5173

Opcional: crie um arquivo `.env` na pasta `frontend` com `VITE_API_BASE=http://localhost:3000/api` para apontar para outra URL da API.

Os dois precisam estar rodando ao mesmo tempo.

---

## 🧪 Exemplo de Uso

**Criar um check**

Envie um `POST` para `http://localhost:3000/api/checks` com o corpo em JSON contendo:

- `name`: "GitHub API"
- `url`: "https://api.github.com"
- `method`: "GET"
- `intervalMinutes`: 5
- `timeoutMs`: 5000
- `active`: true

A API responde com o check criado, incluindo os campos `id` e `createdAt`.

**Outras consultas**

- Listar checks: `GET http://localhost:3000/api/checks`
- Saúde da API: `GET http://localhost:3000/api/health`
- Resumo do dashboard: `GET http://localhost:3000/api/dashboard/summary`

---

## ⚠️ Observações

- Os dados ficam em memória: ao reiniciar o backend, os checks criados são perdidos
- O dashboard retorna um resumo estático, sem monitoramento em tempo real (parte do MVP)
- O projeto ainda não tem Docker, CI/CD nem testes automatizados

---

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

---

Desenvolvido por **Elder Sampaio** · [LinkedIn](https://www.linkedin.com/in/elder404sampaio)
