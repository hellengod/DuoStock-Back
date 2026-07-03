# Modelagem inicial do banco de dados

Este documento descreve as entidades iniciais do banco de dados do DuoStock, seus campos, tipos, relacionamentos e principais regras de negócio.

## Business

Representa o pequeno negócio que utiliza o DuoStock.

### Campos

| Campo       | Tipo     | Obrigatório | Descrição                      |
| ----------- | -------- | ----------: | ------------------------------ |
| `id`        | UUID     |         Sim | Identificador único da empresa |
| `name`      | String   |         Sim | Nome da empresa                |
| `document`  | String   |         Não | CPF ou CNPJ da empresa         |
| `active`    | Boolean  |         Sim | Indica se a empresa está ativa |
| `createdAt` | DateTime |         Sim | Data de criação do registro    |
| `updatedAt` | DateTime |         Sim | Data da última atualização     |

### Valores padrão

* `id` deve ser gerado automaticamente como UUID.
* `active` deve iniciar como `true`.
* `createdAt` deve ser preenchido automaticamente.
* `updatedAt` deve ser atualizado automaticamente.

### Relacionamentos

Uma empresa pode possuir:

* vários usuários;
* vários produtos;
* várias categorias;
* vários fornecedores;
* várias vendas;
* várias movimentações de estoque.

### Regras

* O nome da empresa é obrigatório.
* O documento é opcional.
* Caso informado, o documento deve ser único.
* A empresa deve ser inativada em vez de excluída quando possuir dados vinculados.
* Usuários, produtos, vendas e movimentações devem estar associados a uma empresa.

---

## User

Representa um usuário do sistema, que pode ser dono ou funcionário.

### Campos

| Campo        | Tipo     | Obrigatório | Descrição                                          |
| ------------ | -------- | ----------: | -------------------------------------------------- |
| `id`         | UUID     |         Sim | Identificador único do usuário                     |
| `name`       | String   |         Sim | Nome do usuário                                    |
| `email`      | String   |         Sim | E-mail utilizado para login                        |
| `password`   | String   |         Sim | Hash da senha do usuário                           |
| `role`       | Enum     |         Sim | Perfil do usuário                                  |
| `businessId` | UUID     |         Sim | Identificador da empresa à qual o usuário pertence |
| `active`     | Boolean  |         Sim | Indica se o usuário está ativo                     |
| `createdAt`  | DateTime |         Sim | Data de criação do registro                        |
| `updatedAt`  | DateTime |         Sim | Data da última atualização                         |

### Valores de `role`

* `OWNER`: dono do negócio;
* `EMPLOYEE`: funcionário do negócio.

### Valores padrão

* `id` deve ser gerado automaticamente como UUID.
* `active` deve iniciar como `true`.
* `createdAt` deve ser preenchido automaticamente.
* `updatedAt` deve ser atualizado automaticamente.

### Relacionamentos

* Cada usuário pertence a uma empresa.
* Uma empresa pode possuir vários usuários.
* Um usuário poderá registrar várias vendas.
* Um usuário poderá registrar várias movimentações de estoque.

### Regras

* O nome é obrigatório.
* O e-mail é obrigatório e deve ser único.
* A senha original nunca deve ser armazenada no banco.
* O campo `password` deve guardar apenas o hash da senha.
* O perfil deve ser `OWNER` ou `EMPLOYEE`.
* Todo usuário deve estar vinculado a uma empresa.
* Usuários inativos não podem fazer login.
* Usuários que já registraram vendas ou movimentações não devem ser excluídos.
* Para remover o acesso de um usuário, o campo `active` deve ser alterado para `false`.

---

## Relacionamento entre Business e User

```txt
Business 1 ─── N User
```

Uma empresa pode possuir vários usuários, mas cada usuário pertence a apenas uma empresa.
