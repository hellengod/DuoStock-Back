# DuoStock Back-end

API do **DuoStock**, um sistema web para gerenciamento de estoque e vendas de pequenos negócios.

Este repositório contém o back-end da aplicação, responsável por fornecer os recursos necessários para autenticação, produtos, categorias, fornecedores, vendas e movimentações de estoque.

## Objetivo da API

A API do DuoStock tem como objetivo centralizar as regras de negócio do sistema, permitindo que pequenos negócios possam:

* cadastrar e gerenciar produtos;
* controlar entrada e saída de estoque;
* registrar vendas;
* consultar histórico de movimentações;
* visualizar produtos com estoque baixo;
* autenticar usuários do sistema;
* diferenciar ações entre dono e funcionário futuramente.

## Tecnologias utilizadas

* Node.js
* TypeScript
* Express
* PostgreSQL
* Prisma ORM
* CORS
* Dotenv
* TSX

Futuramente, o projeto também utilizará:

* JWT
* Bcrypt

## Pré-requisitos

Antes de rodar o projeto, é necessário ter instalado:

* Node.js
* npm
* Git
* PostgreSQL

## Como instalar o projeto

Clone o repositório:

```bash
git clone https://github.com/hellengod/DuoStock-Back.git
```

Acesse a pasta do projeto:

```bash
cd DuoStock-Back
```

Instale as dependências:

```bash
npm install
```

## Configuração de ambiente

Crie um arquivo `.env` na raiz do projeto.

Exemplo:

```env
PORT=3333
DATABASE_URL="postgresql://USER:PASSWORD@localhost:5432/duostock?schema=public"
```

A porta padrão da API é:

```txt
3333
```

O banco local esperado para desenvolvimento é:

```txt
duostock
```

O arquivo `.env` não deve ser enviado para o GitHub, pois pode conter informações sensíveis como usuário e senha do banco de dados.

## Como rodar localmente

Para iniciar o servidor em modo de desenvolvimento, execute:

```bash
npm run dev
```

Se tudo estiver correto, o terminal exibirá uma mensagem parecida com:

```txt
Server is running on port 3333
```

## Rota de saúde da API

A API possui uma rota inicial para verificar se o servidor está funcionando corretamente.

### Endpoint

```http
GET /health
```

### URL local

```txt
http://localhost:3333/health
```

### Resposta esperada

```json
{
  "status": "ok",
  "message": "DuoStock API is running"
}
```

Essa rota serve apenas para confirmar que a API está ativa e respondendo corretamente.

## Prisma

O projeto utiliza **Prisma ORM** para modelagem e comunicação com o banco de dados PostgreSQL.

Comandos úteis:

```bash
npx prisma validate
```

Valida se o schema do Prisma está correto.

```bash
npx prisma format
```

Formata o arquivo `schema.prisma`.

```bash
npx prisma generate
```

Gera o Prisma Client após alterações no schema.

As migrations serão criadas após a definição final dos models iniciais.

## Estrutura inicial do projeto

```txt
src
├── database
├── middlewares
├── modules
│   ├── auth
│   ├── categories
│   ├── products
│   ├── sales
│   ├── stock-movements
│   ├── suppliers
│   └── users
├── routes
├── shared
└── server.ts
```

## Documentação

A modelagem inicial do banco está documentada em:

```txt
docs/database-modeling.md
```

Esse documento descreve as entidades principais, campos, relacionamentos e regras de negócio do banco de dados.

## Padrão de branches

O projeto utilizará branches por funcionalidade, ajuste ou documentação.

Exemplos:

```txt
feature/create-product
feature/list-products
feature/setup-prisma
feature/login
fix/stock-validation
docs/update-readme
docs/database-modeling
chore/setup-local-database
```

### Regras

* Não desenvolver diretamente na branch `main`.
* Criar uma branch para cada tarefa.
* Abrir Pull Request ao finalizar uma task.
* Outra pessoa deve revisar antes de fazer merge.

## Padrão de commits

O projeto seguirá um padrão simples de commits semânticos.

Exemplos:

```txt
feat: create product endpoint
feat: setup prisma
fix: validate stock quantity
docs: update backend readme
docs: define initial database modeling
chore: adjust project structure
refactor: organize product module
```

### Prefixos principais

* `feat`: nova funcionalidade
* `fix`: correção de bug
* `docs`: alteração de documentação
* `chore`: tarefa de configuração ou manutenção
* `refactor`: melhoria interna sem alterar comportamento
* `test`: criação ou ajuste de testes

## Status do projeto

Projeto em fase inicial de desenvolvimento.

Funcionalidades atuais:

* Estrutura inicial do back-end
* Servidor Express configurado
* Rota `/health` funcionando
* Prisma configurado
* PostgreSQL local configurado
* Modelagem inicial do banco documentada
