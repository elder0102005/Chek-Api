
#health-check-api

API de verificação de saúde (Health Check) construída em Node.js com TypeScript, monitorando em tempo real a conectividade com um banco de dados relacional (PostgreSQL) e um banco não-relacional (MongoDB).

Este projeto foi desenvolvido como demonstração prática de boas práticas de backend moderno: tipagem estática com TypeScript, testes automatizados, containerização com Docker e um pipeline de integração contínua (CI) via GitHub Actions.

Health checks são um padrão amplamente usado em sistemas distribuídos — orquestradores como Kubernetes, ECS e load balancers consultam esse tipo de endpoint para decidir se uma instância da aplicação deve continuar recebendo tráfego.

## Funcionalidades principais

- **API REST em TypeScript**: endpoint `GET /health` que retorna o status da aplicação e de suas dependências.
- **Verificação de PostgreSQL**: checagem ativa da conexão com o banco relacional a cada chamada.
- **Verificação de MongoDB**: checagem ativa da conexão com o banco não-relacional a cada chamada.
- **Resposta em falha controlada**: se qualquer dependência estiver fora do ar, a API responde `503` com `"status": "degraded"`, em vez de travar ou mentir dizendo que está tudo bem.
- **Testes automatizados**: cobertura com Jest e Supertest, incluindo cenário de sucesso e de falha do endpoint.
- **Ambiente containerizado**: `Dockerfile` multi-stage otimizado e `docker-compose.yml` que sobe API, PostgreSQL e MongoDB juntos.
- **Pipeline de CI**: workflow no GitHub Actions que roda type-check, testes e build a cada push/PR.

## Requisitos de Sistema

- Node.js 20.x ou superior
- Docker e Docker Compose (para rodar via containers, recomendado)
- npm

## Instalação e Configuração

### 1. Clonar o projeto

Certifique-se de que a estrutura de pastas do `health-check-api` esteja extraída no seu ambiente.

### 2. Configurar variáveis de ambiente

Copie o arquivo de exemplo e ajuste se necessário:

```bash
cp .env.example .env
```

### 3. Instalar dependências (apenas se for rodar sem Docker)

```bash
npm install
```

## Como Executar o Projeto

Você tem duas formas de executar o `health-check-api`:

### 1. Via Docker (Recomendado)

Sobe a API junto com o PostgreSQL e o MongoDB em containers isolados:

```bash
docker-compose up --build
```

A API estará disponível em `http://localhost:3000`. Essa versão não exige que você tenha PostgreSQL ou MongoDB instalados na sua máquina — tudo roda dentro dos containers.

### 2. Via Código-Fonte

Caso prefira rodar diretamente, certificando-se de ter um PostgreSQL e um MongoDB acessíveis localmente:

```bash
npm run dev
```

## Como Utilizar

1. Suba o projeto através de um dos métodos acima.
2. Faça uma requisição `GET` para o endpoint de saúde:

```bash
curl http://localhost:3000/health
```

3. A resposta indica o status geral e de cada dependência:

```json
{
  "status": "ok",
  "timestamp": "2026-08-24T12:00:00.000Z",
  "responseTimeMs": 8,
  "dependencies": {
    "postgres": "up",
    "mongo": "up"
  }
}
```

Se alguma dependência estiver indisponível, o `status` muda para `"degraded"` e o código HTTP retornado é `503`.

## Estrutura do Projeto

- `src/`: diretório principal contendo o código-fonte da aplicação.
  - `index.ts`: script de entrada, conecta aos bancos e sobe o servidor.
  - `app.ts`: criação e configuração do app Express, isolada do `index.ts` para facilitar os testes.
  - `config/`: conexão e verificação de saúde do PostgreSQL (`database.ts`) e do MongoDB (`mongo.ts`).
  - `controllers/`: lógica de orquestração do endpoint de health check.
  - `routes/`: mapeamento das rotas HTTP para os controllers.
- `tests/`: testes automatizados com Jest e Supertest.
- `.github/workflows/ci.yml`: pipeline de integração contínua.
- `Dockerfile`: build multi-stage otimizado para produção.
- `docker-compose.yml`: orquestração local de API, PostgreSQL e MongoDB.
