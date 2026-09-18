
#check-api

API REST para registrar, consultar e gerenciar verificações ("checks") de endpoints externos, funcionando como um pequeno monitor de disponibilidade e integridade de serviços. Permite criar checks apontando para URLs e métodos HTTP, acompanhar sua configuração e consultar um resumo operacional do sistema.

O projeto é composto por um backend em NestJS (já pronto) e um frontend em React para visualização e interação com os recursos expostos pela API. A ideia central é oferecer uma base simples de monitoramento e administração de rotas que podem ser testadas em uma interface visual, servindo como demonstração prática de arquitetura em camadas com NestJS e consumo de API com React.

Funcionalidades
GET /api/health: verifica se a API está funcionando corretamente.
GET /api/dashboard/summary: retorna um resumo do estado do dashboard (total de checks, checks ativos, falhas, uptime).
POST /api/checks: cria um novo check de monitoramento.
GET /api/checks: lista todos os checks cadastrados.
GET /api/checks/:id: retorna um check específico pelo identificador.
PUT /api/checks/:id: atualiza os dados de um check existente.
DELETE /api/checks/:id: remove um check do armazenamento em memória.
Interface visual (frontend): permite cadastrar rotas, editar parâmetros e testar requisições diretamente no navegador, sem precisar de ferramentas externas como Postman.
Requisitos de Sistema
Node.js 18 ou superior
npm 9 ou superior
Git (opcional, para clonar o repositório)
Navegador moderno para acessar o frontend
Instalação e Configuração
1. Clonar o projeto
bash
cd "check api"
2. Instalar dependências do backend
bash
cd backend
npm install
3. Configurar a porta do backend (opcional)

Crie um arquivo .env na pasta backend/ se quiser definir uma porta específica:

env
PORT=3000

O backend também funciona sem esse arquivo, pois usa process.env.PORT || 3000 no bootstrap.

4. Instalar dependências do frontend
bash
cd ../frontend
npm install
5. Apontar o frontend para a API (opcional)

Se a API estiver rodando em outra URL, configure a variável de ambiente VITE_API_BASE no frontend. Por padrão, ele usa:

text
http://localhost:3000/api
Como Executar o Projeto
Backend

No diretório backend:

bash
npm run start:dev

Disponível em http://localhost:3000, com prefixo global de rota em /api.

Frontend

No diretório frontend:

bash
npm run dev

O Vite normalmente inicia em http://localhost:5173. O frontend acessa a API na URL configurada em VITE_API_BASE ou, por padrão, em http://localhost:3000/api.

Os dois precisam estar rodando ao mesmo tempo (em terminais separados) para a interface funcionar por completo.

Como Utilizar
Criar um check
bash
curl -X POST http://localhost:3000/api/checks \
  -H "Content-Type: application/json" \
  -d '{
    "name": "GitHub API",
    "url": "https://api.github.com",
    "method": "GET",
    "intervalMinutes": 5,
    "timeoutMs": 5000,
    "active": true
  }'

Resposta esperada:

json
{
  "id": "1",
  "name": "GitHub API",
  "url": "https://api.github.com",
  "method": "GET",
  "intervalMinutes": 5,
  "timeoutMs": 5000,
  "active": true,
  "createdAt": "2026-09-17T00:00:00.000Z"
}
Listar checks
bash
curl http://localhost:3000/api/checks
Verificar a saúde da API
bash
curl http://localhost:3000/api/health
json
{
  "status": "ok",
  "timestamp": "2026-09-17T00:00:00.000Z",
  "service": "check-api"
}
Consultar o resumo do dashboard
bash
curl http://localhost:3000/api/dashboard/summary
json
{
  "totalChecks": 0,
  "activeChecks": 0,
  "failedChecks": 0,
  "uptime": 0,
  "lastUpdated": "2026-09-17T00:00:00.000Z"
}
Estrutura do Projeto
text
check-api/
├── README.md
├── backend/
│   └── src/
│       ├── app.module.ts        # registra os módulos da aplicação
│       ├── main.ts              # bootstrap do NestJS e habilitação de CORS
│       ├── common/               # enums e types compartilhados
│       └── modules/
│           ├── checks/          # criação, listagem, atualização e remoção dos checks
│           ├── dashboard/       # resumo do estado do dashboard
│           └── health/          # verificação de saúde da API
└── frontend/
    └── src/
        ├── App.tsx              # tela principal do MVP
        ├── main.tsx             # entrada da aplicação React
        ├── components/
        │   ├── RequestEditor.tsx
        │   ├── ResponsePanel.tsx
        │   └── Sidebar.tsx
        └── services/
            └── api.ts           # cliente para consumo da API
Observações Especiais
O backend já existe e foi entregue pronto; ele deve ser usado como base do projeto e não deve ser modificado sem necessidade.
Os dados são armazenados em memória — não há banco de dados configurado. Ao reiniciar o backend, os checks criados anteriormente são perdidos.
O módulo de dashboard expõe um resumo estático e não realiza monitoramento real de endpoints em tempo real; esse comportamento é parte do MVP.
O CORS está habilitado no bootstrap do NestJS com app.enableCors(), permitindo que o frontend em outra porta acesse a API local.
Em ambientes com portas diferentes, ajuste VITE_API_BASE no frontend ou configure a origem permitida no backend.
Sem Docker, CI/CD ou testes automatizados configurados no estado atual do projeto.
