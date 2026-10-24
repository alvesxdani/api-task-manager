---
name: php-task-api-curriculum
description: >-
  Guia curricular e procedimentos práticos para ensinar desenvolvimento de APIs REST com PHP moderno,
  arquitetura de software, modelagem de banco de dados e preparação para o mercado de trabalho.
---

# Currículo: Construção de API de Tarefas com PHP Moderno

Este documento serve como referência de conteúdo, arquitetura e fluxo de ensino para conduzir o projeto da API de Gerenciamento de Tarefas passo a passo.

---

## 🧭 Mapa da Trilha de Aprendizado

### Módulo 1: Fundamentos do PHP Moderno & Inicialização de Projetos
- **PHP Moderno vs Legado**: O que mudou do PHP 5/7 para o 8.2/8.3 (JIT, Attributes, Constructor Promotion, Enums, Readonly Classes, Match Expressions).
- **Composer**: O gerenciador de dependências oficial do ecossistema PHP. O que são `composer.json`, `composer.lock` e como funciona o autoload `PSR-4`.
- **Ambiente de Desenvolvimento**: Por que usar Docker em vez de instalar versões na máquina local (isolamento, paridade com produção, portabilidade).
- **Escolha do Framework**:
  - *Laravel*: Líder absoluto de vagas no mercado PHP mundial e brasileiro, ecossistema riquíssimo (Eloquent, Sanctum, Queues, Pest).
  - *Symfony*: Base de grandes corporações e a espinha dorsal de muitas ferramentas PHP (inclusive de partes do Laravel).
  - *Slim Framework*: Micro-framework minimalista ideal para entender o funcionamento interno de PSR-7/15.
  - *Recomendação para portfólio rápido e empregabilidade*: **Laravel 11+** com arquitetura em camadas (Services/Actions e DTOs) ou **Slim 4** se o foco for dominar PSRs do zero.

### Módulo 2: Arquitetura de Software em APIs
- **Monolito vs API REST**: Separação clara entre Backend (fornecedor de dados JSON) e Frontend (consumidor).
- **O ciclo de vida de uma requisição HTTP**:
  1. Cliente envia HTTP Request (Header, Body, Query Params).
  2. Nginx/Servidor web direciona para o arquivo de entrada (`public/index.php`).
  3. Roteador identifica o endpoint.
  4. Middlewares filtram (Autenticação, Rate Limit, CORS).
  5. Controller recebe os dados validados.
  6. Camada de Aplicação/Domínio (Service ou Action) executa a regra.
  7. Camada de Dados (Repository / ORM Eloquent / Query Builder) persiste ou consulta.
  8. Resposta é serializada (API Resource) em JSON com HTTP Status Code semântico.
- **Padrões Arquiteturais mais usados no mercado**:
  - *MVC Clássico*: Controllers frequentemente incham ("Fat Controllers").
  - *Service / Action Layer (Recomendado para APIs corporativas)*: Controllers magros, cada ação ou serviço possui uma única responsabilidade.
  - *DTO (Data Transfer Object)*: Objeto imutável e fortemente tipado para transportar dados entre requisição e serviços.
  - *Repository Pattern*: Quando usar (para desacoplar persistência) e quando evitar (sobrecarga desnecessária sobre ORMs modernos como Eloquent).

### Módulo 3: Modelagem e Criação de Banco de Dados
- **Modelagem Relacional (DER)**:
  - `users`: id, name, email, password, timestamps.
  - `categories` (ou tags): id, user_id, name, color, timestamps.
  - `tasks`: id, user_id, category_id, title, description, status (enum: pending, in_progress, completed, archived), priority (enum: low, medium, high), due_date, completed_at, timestamps.
- **Conceitos Essenciais de Banco**:
  - Chaves Primárias (BigInt x UUID/ULID).
  - Chaves Estrangeiras (Foreign Keys) e integridade referencial (`ON DELETE CASCADE` ou `RESTRICT`).
  - Índices (`INDEX` em `user_id`, `status`, `due_date` para acelerar filtros de busca).
  - Migrations: versionamento de esquema em código (o Git do banco de dados).
  - Seeders e Factories: geração de dados falsos (faker) para desenvolvimento e testes.

### Módulo 4: RESTful API Design & Boas Práticas de Mercado
- **Verbos HTTP e Idempotência**:
  - `GET /api/v1/tasks` (Listar com paginação, filtros e ordenação) - 200 OK
  - `POST /api/v1/tasks` (Criar tarefa) - 201 Created
  - `GET /api/v1/tasks/{id}` (Detalhar uma tarefa) - 200 OK ou 404 Not Found
  - `PUT /api/v1/tasks/{id}` (Substituir tarefa inteira) - 200 OK
  - `PATCH /api/v1/tasks/{id}/status` (Atualização parcial) - 200 OK
  - `DELETE /api/v1/tasks/{id}` (Remover tarefa) - 204 No Content
- **Validação e Tratamento de Erros**:
  - Retorno padrão em caso de erro de validação (HTTP 422 Unprocessable Content com mapa de campos).
  - Exception Handling global para nunca vazar stack trace em produção (HTTP 500).
- **Paginação**: Por que nunca retornar `SELECT * FROM tasks` em uma API de produção (uso de paginação baseada em cursor ou limit/offset).

### Módulo 5: Segurança e Autenticação
- **Autenticação Stateless via Tokens**:
  - Como funciona bearer token.
  - Laravel Sanctum ou JWT.
- **Autorização (Policies / Gates)**:
  - Garantir que o usuário A nunca consiga ver, editar ou deletar as tarefas do usuário B (vulnerabilidade IDOR / BOLA - Broken Object Level Authorization).

### Módulo 6: Testes Automatizados (Diferencial para Entrevistas)
- Por que empresas valorizam candidatos que testam: segurança para refatorar e menos bugs em produção.
- Testes de Feature com Pest:
  - Teste de cadastro e login.
  - Teste de criação de tarefa válida e inválida.
  - Teste de autorização (usuário não pode acessar tarefa de outro).

---

## 💡 Checklist de Competências para Entrevistas Técnicas
- [ ] Sabe explicar o que é e para que serve o Composer e o autoload PSR-4.
- [ ] Domina os princípios básicos de SOLID (especialmente Single Responsibility e Dependency Inversion).
- [ ] Sabe a diferença entre `PUT` e `PATCH`.
- [ ] Sabe explicar o que é uma migration e por que não criamos tabelas manualmente no banco em produção.
- [ ] Conhece e aplica códigos de status HTTP corretos (200, 201, 204, 400, 401, 403, 404, 422, 500).
- [ ] Sabe proteger rotas com autenticação e políticas de autorização.
- [ ] Sabe escrever pelo menos testes de integração básicos.
