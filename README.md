# API de Autenticação

Esse é um projeto de uma API construída em **Java** e **Spring Boot**, utilizando **Spring Security** e **JWT** para controle de autenticação e autorização, **PostgreSQL** como banco de dados e **Flyway** para gerenciamento de migrações.

---

## 📑 Sumário

- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Funcionalidades](#️-funcionalidades)
- [Autenticação e Autorização](#-autenticação-e-autorização)
- [Pré-requisitos](#-pré-requisitos)
- [Configuração do Banco de Dados](#️-configuração-do-banco-de-dados)
- [Configuração do JWT](#-configuração-do-jwt)
- [Como Rodar o Projeto](#️-como-rodar-o-projeto)
- [Endpoints da API](#-endpoints-da-api)
- [Autor](#-autor)

---

## 🚀 Tecnologias Utilizadas

- Java 17
- Spring Boot 3.2.5
- Spring Data JPA
- Spring Web
- Spring Security
- Flyway Migration
- PostgreSQL
- JWT (java-jwt - Auth0)
- Lombok
- Spring Boot DevTools
- Maven

---

## ⚙️ Funcionalidades

- Cadastro e autenticação de usuários
- Geração e validação de tokens JWT (expiração de 2 horas)
- Controle de acesso por perfis de usuário (`ADMIN` e `USER`)
- Cadastro de produtos (endpoint restrito a `ADMIN`)
- Proteção de endpoints via filtro de segurança customizado (`SecurityFilter`)
- Gerenciamento de versões do banco de dados com Flyway

---

## 🔐 Autenticação e Autorização

O fluxo de autenticação funciona da seguinte forma:

1. O usuário se cadastra via `/auth/register`, informando `login`, `password` e `role` (`ADMIN` ou `USER`).
2. Ao efetuar login via `/auth/login`, a API retorna um **token JWT** válido por **2 horas**.
3. Para acessar endpoints protegidos, o token deve ser enviado no cabeçalho da requisição:
Authorization: Bearer SEU_TOKEN_JWT
4. Um filtro de segurança (`SecurityFilter`) intercepta cada requisição, valida o token e autentica o usuário no contexto do Spring Security.

Perfis disponíveis:

- **USER** → recebe a permissão `ROLE_USER`.
- **ADMIN** → recebe as permissões `ROLE_ADMIN` e `ROLE_USER` (acumula os dois níveis de acesso).

O endpoint `POST /product` é restrito a usuários com a role **ADMIN**. Os demais endpoints (fora login/registro) exigem apenas que o usuário esteja autenticado.

---

## ✅ Pré-requisitos

Antes de rodar o projeto, você precisa ter instalado:

- [JDK 17](https://adoptium.net/) ou superior
- Maven 3.8+ (opcional, o projeto já inclui o wrapper `mvnw`)
- [PostgreSQL](https://www.postgresql.org/download/) instalado e em execução
- Uma IDE de sua preferência (IntelliJ, Eclipse, VS Code, etc.)

---

## 🗄️ Configuração do Banco de Dados

1. Crie um banco de dados no PostgreSQL, por exemplo:
```sql
   CREATE DATABASE auth_db;
```

2. Configure as credenciais de acesso no arquivo `src/main/resources/application.properties` (ou `application.yml`):
```properties
   spring.datasource.url=jdbc:postgresql://localhost:5432/auth_db
   spring.datasource.username=SEU_USUARIO
   spring.datasource.password=SUA_SENHA
   spring.flyway.enabled=true
```

3. As migrações do Flyway serão executadas automaticamente na primeira inicialização da aplicação.

---

## 🔑 Configuração do JWT

A geração e validação dos tokens depende de uma chave secreta definida na propriedade `api.security.token.secret`. Configure-a no `application.properties`:

```properties
api.security.token.secret=SUA_CHAVE_SECRETA_AQUI
```

> ⚠️ Nunca suba essa chave real para o repositório. O recomendado é usar uma variável de ambiente:
> ```properties
> api.security.token.secret=${JWT_SECRET}
> ```

---

## ▶️ Como Rodar o Projeto

1. Clone o repositório:
```bash
   git clone https://github.com/gu1gor/auth.git
   cd auth
```

2. Configure o banco de dados e a chave do JWT conforme as seções anteriores.

3. Rode o projeto usando o Maven Wrapper:
```bash
   # Linux/Mac
   ./mvnw spring-boot:run

   # Windows
   mvnw.cmd spring-boot:run
```

   Ou execute diretamente a classe principal da aplicação pela sua IDE.

4. A API estará disponível em:
http://localhost:8080

---

## 🔌 Endpoints da API

| Método | Endpoint         | Descrição                                | Autenticação |
|--------|------------------|--------------------------------------------|---------------|
| POST   | `/auth/register` | Cadastra um novo usuário                   | Não           |
| POST   | `/auth/login`    | Autentica um usuário e retorna o token JWT | Não           |
| POST   | `/product`       | Cadastra um produto                        | Sim (ADMIN)   |

### Registrar usuário

**Corpo da requisição:**
```json
{
  "login": "usuario123",
  "password": "senha123",
  "role": "ADMIN"
}
```

```bash
curl -X POST http://localhost:8080/auth/register \
  -H "Content-Type: application/json" \
  -d '{"login": "usuario123", "password": "senha123", "role": "ADMIN"}'
```

### Login

**Corpo da requisição:**
```json
{
  "login": "usuario123",
  "password": "senha123"
}
```

**Resposta esperada:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9..."
}
```

```bash
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"login": "usuario123", "password": "senha123"}'
```

### Cadastrar produto (requer ADMIN)

**Corpo da requisição:**
```json
{
  "name": "Produto Exemplo",
  "price": 9990
}
```

```bash
curl -X POST http://localhost:8080/product \
  -H "Authorization: Bearer SEU_TOKEN_JWT" \
  -H "Content-Type: application/json" \
  -d '{"name": "Produto Exemplo", "price": 9990}'
```

> ⚠️ Note que `price` é do tipo `Integer` no modelo (não é decimal/BigDecimal), então valores como preço devem ser representados como número inteiro (ex: centavos, se essa for a convenção usada no projeto).

---

## 👤 Autor

Desenvolvido por **Gustavo Igor**
