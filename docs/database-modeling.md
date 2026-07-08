# Modelagem inicial do banco de dados

Este documento descreve as entidades iniciais do banco de dados do **DuoStock**, seus campos, tipos, relacionamentos e principais regras de negócio.

O DuoStock é um sistema de gerenciamento de estoque e vendas para pequenos negócios. A modelagem foi pensada para permitir que uma empresa tenha usuários, produtos, categorias, fornecedores, vendas e movimentações de estoque.

---

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
* `updatedAt` deve ser atualizado automaticamente sempre que o registro for alterado.

### Relacionamentos

Uma empresa pode possuir:

* vários usuários;
* várias categorias;
* vários fornecedores;
* vários produtos;
* várias vendas;
* várias movimentações de estoque.

### Regras

* O nome da empresa é obrigatório.
* O documento é opcional.
* Caso informado, o documento deve ser único.
* A empresa deve ser inativada em vez de excluída quando possuir dados vinculados.
* Usuários, produtos, categorias, fornecedores, vendas e movimentações devem estar associados a uma empresa.

### Relacionamentos principais

```txt
Business 1 ─── N User
Business 1 ─── N Category
Business 1 ─── N Supplier
Business 1 ─── N Product
Business 1 ─── N Sale
Business 1 ─── N StockMovement
```

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
| `role`       | UserRole |         Sim | Perfil do usuário no sistema                       |
| `businessId` | UUID     |         Sim | Identificador da empresa à qual o usuário pertence |
| `active`     | Boolean  |         Sim | Indica se o usuário está ativo                     |
| `createdAt`  | DateTime |         Sim | Data de criação do registro                        |
| `updatedAt`  | DateTime |         Sim | Data da última atualização                         |

### Valores padrão

* `id` deve ser gerado automaticamente como UUID.
* `active` deve iniciar como `true`.
* `createdAt` deve ser preenchido automaticamente.
* `updatedAt` deve ser atualizado automaticamente sempre que o registro for alterado.

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
* Usuários que já registraram vendas ou movimentações não devem ser excluídos definitivamente.
* Para remover o acesso de um usuário, o campo `active` deve ser alterado para `false`.

### Relacionamentos principais

```txt
Business 1 ─── N User
User 1 ─── N Sale
User 1 ─── N StockMovement
```

---

## Category

Representa uma categoria de produtos dentro de uma empresa.

Exemplos de categorias:

* Bebidas
* Alimentos
* Limpeza
* Roupas
* Eletrônicos

### Campos

| Campo        | Tipo     | Obrigatório | Descrição                                            |
| ------------ | -------- | ----------: | ---------------------------------------------------- |
| `id`         | UUID     |         Sim | Identificador único da categoria                     |
| `name`       | String   |         Sim | Nome da categoria                                    |
| `businessId` | UUID     |         Sim | Identificador da empresa à qual a categoria pertence |
| `active`     | Boolean  |         Sim | Indica se a categoria está ativa                     |
| `createdAt`  | DateTime |         Sim | Data de criação do registro                          |
| `updatedAt`  | DateTime |         Sim | Data da última atualização                           |

### Valores padrão

* `id` deve ser gerado automaticamente como UUID.
* `active` deve iniciar como `true`.
* `createdAt` deve ser preenchido automaticamente.
* `updatedAt` deve ser atualizado automaticamente sempre que o registro for alterado.

### Relacionamentos

* Cada categoria pertence a uma empresa.
* Uma empresa pode possuir várias categorias.
* Uma categoria poderá estar vinculada a vários produtos.

### Regras

* O nome da categoria é obrigatório.
* A categoria deve estar sempre vinculada a uma empresa.
* O ideal é impedir categorias duplicadas dentro da mesma empresa.
* Categorias inativas não devem aparecer como opção no cadastro de novos produtos.
* Categorias com produtos vinculados não devem ser excluídas definitivamente.
* Para remover uma categoria do uso do sistema, o campo `active` deve ser alterado para `false`.

### Relacionamentos principais

```txt
Business 1 ─── N Category
Category 1 ─── N Product
```

---

## Supplier

Representa um fornecedor vinculado a uma empresa.

O fornecedor pode ser usado para identificar de onde vêm os produtos cadastrados no estoque.

### Campos

| Campo        | Tipo     | Obrigatório | Descrição                                             |
| ------------ | -------- | ----------: | ----------------------------------------------------- |
| `id`         | UUID     |         Sim | Identificador único do fornecedor                     |
| `name`       | String   |         Sim | Nome do fornecedor                                    |
| `email`      | String   |         Não | E-mail de contato do fornecedor                       |
| `phone`      | String   |         Não | Telefone de contato do fornecedor                     |
| `document`   | String   |         Não | CPF ou CNPJ do fornecedor                             |
| `notes`      | String   |         Não | Observações adicionais sobre o fornecedor             |
| `businessId` | UUID     |         Sim | Identificador da empresa à qual o fornecedor pertence |
| `active`     | Boolean  |         Sim | Indica se o fornecedor está ativo                     |
| `createdAt`  | DateTime |         Sim | Data de criação do registro                           |
| `updatedAt`  | DateTime |         Sim | Data da última atualização                            |

### Valores padrão

* `id` deve ser gerado automaticamente como UUID.
* `active` deve iniciar como `true`.
* `createdAt` deve ser preenchido automaticamente.
* `updatedAt` deve ser atualizado automaticamente sempre que o registro for alterado.

### Relacionamentos

* Cada fornecedor pertence a uma empresa.
* Uma empresa pode possuir vários fornecedores.
* Um fornecedor poderá estar vinculado a vários produtos.
* O vínculo entre fornecedor e produto será opcional, pois nem todo produto precisa ter fornecedor cadastrado.

### Regras

* O nome do fornecedor é obrigatório.
* O fornecedor deve estar sempre vinculado a uma empresa.
* E-mail, telefone, documento e observações são opcionais.
* O documento do fornecedor, quando informado, deve ser único dentro da mesma empresa.
* Fornecedores inativos não devem aparecer como opção no cadastro de novos produtos.
* Fornecedores com produtos vinculados não devem ser excluídos definitivamente.
* Para remover um fornecedor do uso do sistema, o campo `active` deve ser alterado para `false`.

### Relacionamentos principais

```txt
Business 1 ─── N Supplier
Supplier 1 ─── N Product
```

---

## Product

Representa um item vendido ou armazenado no estoque da empresa.

O produto é uma das principais entidades do DuoStock, pois será usado no controle de estoque, nas vendas e nas movimentações.

### Campos

| Campo          | Tipo     | Obrigatório | Descrição                                          |
| -------------- | -------- | ----------: | -------------------------------------------------- |
| `id`           | UUID     |         Sim | Identificador único do produto                     |
| `name`         | String   |         Sim | Nome do produto                                    |
| `description`  | String   |         Não | Descrição ou observações sobre o produto           |
| `sku`          | String   |         Não | Código interno do produto                          |
| `costPrice`    | Decimal  |         Sim | Preço de custo do produto                          |
| `salePrice`    | Decimal  |         Sim | Preço de venda do produto                          |
| `currentStock` | Int      |         Sim | Quantidade atual disponível em estoque             |
| `minimumStock` | Int      |         Sim | Quantidade mínima para alerta de estoque baixo     |
| `categoryId`   | UUID     |         Sim | Identificador da categoria do produto              |
| `supplierId`   | UUID     |         Não | Identificador do fornecedor principal do produto   |
| `businessId`   | UUID     |         Sim | Identificador da empresa à qual o produto pertence |
| `active`       | Boolean  |         Sim | Indica se o produto está ativo                     |
| `createdAt`    | DateTime |         Sim | Data de criação do registro                        |
| `updatedAt`    | DateTime |         Sim | Data da última atualização                         |

### Valores padrão

* `id` deve ser gerado automaticamente como UUID.
* `currentStock` deve iniciar como `0`, caso não seja informado.
* `minimumStock` deve iniciar como `0`, caso não seja informado.
* `active` deve iniciar como `true`.
* `createdAt` deve ser preenchido automaticamente.
* `updatedAt` deve ser atualizado automaticamente sempre que o registro for alterado.

### Relacionamentos

* Cada produto pertence a uma empresa.
* Cada produto pertence obrigatoriamente a uma categoria.
* Cada produto pode ter um fornecedor principal.
* Uma empresa pode possuir vários produtos.
* Uma categoria pode possuir vários produtos.
* Um fornecedor pode estar vinculado a vários produtos.
* Um produto poderá aparecer em vários itens de venda.
* Um produto poderá possuir várias movimentações de estoque.

### Regras

* O nome do produto é obrigatório.
* O preço de custo não pode ser negativo.
* O preço de venda deve ser maior que zero.
* O estoque atual não pode ficar negativo.
* O estoque mínimo não pode ser negativo.
* Todo produto deve pertencer a uma empresa.
* Todo produto deve pertencer a uma categoria.
* O fornecedor do produto é opcional.
* Produtos inativos não devem aparecer como opção para novas vendas.
* Produtos inativos não devem aparecer como opção para novas movimentações de estoque.
* Produtos vinculados a vendas ou movimentações não devem ser excluídos definitivamente.
* Para remover um produto do uso do sistema, o campo `active` deve ser alterado para `false`.
* O estoque atual não deve ser alterado diretamente pela edição do produto.
* O estoque deve ser alterado por meio de uma movimentação de estoque ou venda.
* O código `sku`, quando informado, deve ser único dentro da mesma empresa.

### Relacionamentos principais

```txt
Business 1 ─── N Product
Category 1 ─── N Product
Supplier 1 ─── N Product
Product 1 ─── N SaleItem
Product 1 ─── N StockMovement
```

---

## Sale

Representa uma operação de venda realizada dentro de uma empresa.

A venda registra o total da operação, o usuário responsável e os itens vendidos. No MVP inicial, o sistema pode começar trabalhando apenas com vendas concluídas. O cancelamento pode ser implementado depois.

### Campos

| Campo         | Tipo       | Obrigatório | Descrição                                        |
| ------------- | ---------- | ----------: | ------------------------------------------------ |
| `id`          | UUID       |         Sim | Identificador único da venda                     |
| `totalAmount` | Decimal    |         Sim | Valor total da venda                             |
| `status`      | SaleStatus |         Sim | Status atual da venda                            |
| `businessId`  | UUID       |         Sim | Identificador da empresa à qual a venda pertence |
| `userId`      | UUID       |         Sim | Identificador do usuário que registrou a venda   |
| `createdAt`   | DateTime   |         Sim | Data de criação do registro                      |
| `updatedAt`   | DateTime   |         Sim | Data da última atualização                       |

### Valores padrão

* `id` deve ser gerado automaticamente como UUID.
* `status` pode iniciar como `COMPLETED`.
* `createdAt` deve ser preenchido automaticamente.
* `updatedAt` deve ser atualizado automaticamente sempre que o registro for alterado.

### Relacionamentos

* Cada venda pertence a uma empresa.
* Cada venda é registrada por um usuário.
* Uma venda possui um ou mais itens.
* Uma venda pode gerar uma ou mais movimentações de estoque.

### Regras

* A venda deve estar sempre vinculada a uma empresa.
* A venda deve estar sempre vinculada a um usuário.
* A venda deve possuir pelo menos um item.
* O total da venda deve ser a soma dos subtotais dos itens.
* Uma venda concluída deve reduzir o estoque dos produtos vendidos.
* Uma venda concluída deve gerar movimentação de estoque do tipo `SALE`.
* A venda não deve ser excluída definitivamente.
* Caso uma venda seja cancelada, o sistema deve gerar devolução do estoque.
* No MVP inicial, o sistema pode começar apenas com vendas concluídas.

### Relacionamentos principais

```txt
Business 1 ─── N Sale
User 1 ─── N Sale
Sale 1 ─── N SaleItem
Sale 1 ─── N StockMovement
```

---

## SaleItem

Representa cada produto vendido dentro de uma venda.

Exemplo:

```txt
Venda 10
- 2 refrigerantes
- 1 pacote de arroz
```

Cada linha da venda representa um item da venda.

### Campos

| Campo       | Tipo     | Obrigatório | Descrição                                     |
| ----------- | -------- | ----------: | --------------------------------------------- |
| `id`        | UUID     |         Sim | Identificador único do item da venda          |
| `saleId`    | UUID     |         Sim | Identificador da venda à qual o item pertence |
| `productId` | UUID     |         Sim | Identificador do produto vendido              |
| `quantity`  | Int      |         Sim | Quantidade vendida do produto                 |
| `unitPrice` | Decimal  |         Sim | Preço unitário praticado no momento da venda  |
| `subtotal`  | Decimal  |         Sim | Valor total do item                           |
| `createdAt` | DateTime |         Sim | Data de criação do registro                   |

### Valores padrão

* `id` deve ser gerado automaticamente como UUID.
* `createdAt` deve ser preenchido automaticamente.

### Relacionamentos

* Cada item pertence a uma venda.
* Cada item referencia um produto.
* Uma venda pode possuir vários itens.
* Um produto pode aparecer em vários itens de venda.

### Regras

* A quantidade deve ser maior que zero.
* A quantidade vendida não pode ser maior que o estoque disponível.
* O subtotal deve ser calculado como `quantity × unitPrice`.
* O item deve estar sempre vinculado a uma venda.
* O item deve estar sempre vinculado a um produto.
* Itens de venda não devem ser editados ou apagados depois da conclusão da venda.
* O campo `unitPrice` deve guardar o preço praticado no momento da venda.
* Se o preço do produto mudar depois, a venda antiga deve continuar exibindo o preço original.

### Relacionamentos principais

```txt
Sale 1 ─── N SaleItem
Product 1 ─── N SaleItem
```

---

## StockMovement

Representa toda movimentação que aumenta ou diminui o estoque de um produto.

Essa entidade registra entradas, vendas, saídas manuais, ajustes, perdas e devoluções. Ela é essencial para manter o histórico do estoque.

### Campos

| Campo        | Tipo              | Obrigatório | Descrição                                               |
| ------------ | ----------------- | ----------: | ------------------------------------------------------- |
| `id`         | UUID              |         Sim | Identificador único da movimentação                     |
| `type`       | StockMovementType |         Sim | Tipo da movimentação de estoque                         |
| `quantity`   | Int               |         Sim | Quantidade movimentada                                  |
| `reason`     | String            |         Não | Motivo ou observação da movimentação                    |
| `productId`  | UUID              |         Sim | Identificador do produto movimentado                    |
| `userId`     | UUID              |         Sim | Identificador do usuário que realizou a ação            |
| `businessId` | UUID              |         Sim | Identificador da empresa à qual a movimentação pertence |
| `saleId`     | UUID              |         Não | Identificador da venda relacionada, quando existir      |
| `createdAt`  | DateTime          |         Sim | Data de criação do registro                             |

### Valores padrão

* `id` deve ser gerado automaticamente como UUID.
* `createdAt` deve ser preenchido automaticamente.

### Relacionamentos

* Cada movimentação pertence a uma empresa.
* Cada movimentação está vinculada a um produto.
* Cada movimentação é registrada por um usuário.
* Uma movimentação pode estar vinculada a uma venda.
* Um produto pode possuir várias movimentações.
* Um usuário pode registrar várias movimentações.
* Uma venda pode gerar uma ou mais movimentações.

### Regras

* A quantidade deve ser sempre maior que zero.
* Movimentações do tipo `ENTRY` aumentam o estoque.
* Movimentações do tipo `SALE` diminuem o estoque.
* Movimentações do tipo `MANUAL_OUTPUT` diminuem o estoque.
* Movimentações do tipo `RETURN` aumentam o estoque.
* Movimentações do tipo `LOSS` diminuem o estoque.
* Movimentações do tipo `ADJUSTMENT` devem informar no backend se o ajuste aumenta ou diminui o estoque.
* Nenhuma movimentação pode deixar o estoque negativo.
* Toda venda concluída deve gerar uma movimentação do tipo `SALE`.
* Toda movimentação deve registrar o usuário responsável pela ação.
* Movimentações não devem ser apagadas definitivamente.
* Caso uma movimentação tenha sido feita incorretamente, o ideal é criar uma nova movimentação de ajuste.

### Relacionamentos principais

```txt
Business 1 ─── N StockMovement
Product 1 ─── N StockMovement
User 1 ─── N StockMovement
Sale 1 ─── N StockMovement
```

---

## Enums

### UserRole

Representa o perfil do usuário dentro do sistema.

| Valor      | Descrição              |
| ---------- | ---------------------- |
| `OWNER`    | Dono do negócio        |
| `EMPLOYEE` | Funcionário do negócio |

### SaleStatus

Representa o status de uma venda.

| Valor       | Descrição       |
| ----------- | --------------- |
| `COMPLETED` | Venda concluída |
| `CANCELLED` | Venda cancelada |

### StockMovementType

Representa o tipo de movimentação realizada no estoque.

| Valor           | Descrição                           | Efeito no estoque         |
| --------------- | ----------------------------------- | ------------------------- |
| `ENTRY`         | Entrada de produtos no estoque      | Aumenta                   |
| `SALE`          | Saída causada por uma venda         | Diminui                   |
| `MANUAL_OUTPUT` | Saída manual sem relação com venda  | Diminui                   |
| `ADJUSTMENT`    | Ajuste de inventário                | Pode aumentar ou diminuir |
| `RETURN`        | Devolução de produto ao estoque     | Aumenta                   |
| `LOSS`          | Perda, dano, vencimento ou descarte | Diminui                   |

---

## Regras gerais de exclusão e inativação

* `Business`, `User`, `Product`, `Category` e `Supplier` devem ser inativados, não excluídos definitivamente.
* `Sale`, `SaleItem` e `StockMovement` não devem ser apagados, pois representam histórico do sistema.
* Produtos, categorias e fornecedores inativos não devem aparecer em novos cadastros, vendas ou movimentações.
* Usuários inativos não devem conseguir fazer login.
* Vendas canceladas devem manter histórico e gerar devolução de estoque.
* Movimentações incorretas devem ser compensadas com uma nova movimentação de ajuste, não apagadas.
* O sistema deve preservar o histórico de vendas, itens vendidos e movimentações mesmo que produtos, usuários ou fornecedores sejam inativados.

---

## Resumo dos relacionamentos

```txt
Business 1 ─── N User
Business 1 ─── N Category
Business 1 ─── N Supplier
Business 1 ─── N Product
Business 1 ─── N Sale
Business 1 ─── N StockMovement

Category 1 ─── N Product
Supplier 1 ─── N Product

User 1 ─── N Sale
User 1 ─── N StockMovement

Product 1 ─── N SaleItem
Product 1 ─── N StockMovement

Sale 1 ─── N SaleItem
Sale 1 ─── N StockMovement
```
