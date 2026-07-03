# Mybar Spring

Aplicacao web simples para gerenciamento de contas, itens de menu e pedidos de um bar/restaurante. O projeto foi desenvolvido com Spring Boot, JPA e MySQL, expondo uma API REST e paginas HTML estaticas para operacoes basicas.

## Funcionalidades

- Criar, listar, buscar, fechar e remover contas.
- Cadastrar, buscar e remover itens do menu.
- Registrar pedidos vinculados a uma conta e a um item do menu.
- Listar pedidos de uma conta.
- Gerar extrato da conta pelo frontend estatico.

## Tecnologias

- Java 17
- Spring Boot 3.4.1
- Spring Web
- Spring Data JPA
- Spring Boot Actuator
- Thymeleaf
- Lombok
- Hibernate Validator
- MySQL
- Maven Wrapper
- HTML, CSS e JavaScript

## Estrutura do projeto

```text
.
|-- README.md
`-- banco
    |-- pom.xml
    |-- mvnw
    `-- src
        |-- main
        |   |-- java/com/example/poo/banco
        |   |   |-- controller
        |   |   |-- DTO
        |   |   |-- model
        |   |   |-- repository
        |   |   `-- service
        |   `-- resources
        |       |-- application.properties
        |       `-- static
        `-- test
```

## Como executar

### Pre-requisitos

- Java 17 ou superior
- MySQL em execucao
- Git

### 1. Clone o repositorio

```bash
git clone https://github.com/SoaresDavidson/Mybar-Spring.git
cd Mybar-Spring/banco
```

### 2. Configure o banco de dados

Crie um banco MySQL chamado `bar`:

```sql
CREATE DATABASE bar;
```

Depois, confira as credenciais em `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/bar
spring.datasource.username=root
spring.datasource.password=sua_senha
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

> Observacao: o arquivo atual do projeto possui uma senha local configurada. Para rodar em outra maquina, altere `spring.datasource.username` e `spring.datasource.password` conforme o seu MySQL.

### 3. Execute a aplicacao

No Linux/macOS:

```bash
./mvnw spring-boot:run
```

No Windows:

```bash
mvnw.cmd spring-boot:run
```

A aplicacao ficara disponivel em:

```text
http://localhost:8080
```

## Paginas disponiveis

As paginas HTML ficam em `src/main/resources/static` e sao servidas automaticamente pelo Spring Boot.

| Pagina | Descricao |
| --- | --- |
| `/` ou `/index.html` | Menu inicial |
| `/criar-conta.html` | Cadastro de conta |
| `/add-item.html` | Cadastro de item do menu |
| `/add-pedido.html` | Cadastro de pedido |
| `/listar-pedidos.html` | Extrato/listagem de pedidos por conta |

## API REST

### Contas

| Metodo | Endpoint | Descricao |
| --- | --- | --- |
| `POST` | `/api/contas/create-conta` | Cria uma conta |
| `GET` | `/api/contas` | Lista todas as contas |
| `GET` | `/api/contas/{num_conta}` | Busca uma conta pelo numero |
| `PUT` | `/api/contas/{num_conta}/fechar-conta` | Fecha uma conta |
| `PUT` | `/api/contas/{num_conta}/registrarPagamento` | Registra pagamento |
| `DELETE` | `/api/contas/{num_conta}` | Remove uma conta |

Exemplo de criacao de conta:

```json
{
  "numConta": 1,
  "cpf": 123456789,
  "nomeCliente": "Maria Silva",
  "contaFechada": false,
  "valorPago": 0,
  "gorjetaComida": 0,
  "gorjetaBebida": 0
}
```

Exemplo de registro de pagamento:

```text
PUT /api/contas/1/registrarPagamento?pagamento=50&valorDaConta=120
```

### Menu

| Metodo | Endpoint | Descricao |
| --- | --- | --- |
| `POST` | `/api/menu/add-item` | Cadastra um item do menu |
| `GET` | `/api/menu/{numItem}` | Busca um item pelo numero |
| `DELETE` | `/api/menu/remove/{num}` | Remove um item |

Exemplo de item:

```json
{
  "num": 10,
  "nome": "Hamburguer",
  "preco": 25.9,
  "tipo": 1
}
```

### Pedidos

| Metodo | Endpoint | Descricao |
| --- | --- | --- |
| `POST` | `/api/pedidos/add-pedido` | Cria um pedido para uma conta |
| `GET` | `/api/pedidos/{numConta}` | Lista os pedidos de uma conta |

Exemplo de pedido:

```json
{
  "num_conta": 1,
  "num_item": 10,
  "quant": 2
}
```

## Modelos principais

### Conta

| Campo | Tipo | Descricao |
| --- | --- | --- |
| `numConta` | `int` | Numero identificador da conta |
| `cpf` | `int` | CPF do cliente |
| `nomeCliente` | `String` | Nome do cliente |
| `contaFechada` | `boolean` | Indica se a conta foi fechada |
| `valorPago` | `double` | Valor pago |
| `gorjetaComida` | `double` | Gorjeta relacionada a comida |
| `gorjetaBebida` | `double` | Gorjeta relacionada a bebida |

### Menu

| Campo | Tipo | Descricao |
| --- | --- | --- |
| `num` | `int` | Codigo do item |
| `nome` | `String` | Nome do item |
| `preco` | `double` | Preco do item |
| `tipo` | `int` | Tipo/categoria do item |

### Pedido

| Campo | Tipo | Descricao |
| --- | --- | --- |
| `id` | `int` | Identificador gerado automaticamente |
| `num_conta` | `int` | Numero da conta vinculada |
| `num_item` | `int` | Codigo do item do menu |
| `quant` | `int` | Quantidade solicitada |

## Testes

Para executar os testes:

```bash
./mvnw test
```

## Observacoes

- O Hibernate esta configurado com `spring.jpa.hibernate.ddl-auto=update`, entao as tabelas sao atualizadas automaticamente conforme as entidades.
- O frontend estatico faz requisicoes para `http://localhost:8080`, portanto a API precisa estar rodando nessa porta.
- O projeto usa MySQL; sem o banco configurado corretamente, a aplicacao nao conseguira iniciar.
