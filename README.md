# API de Autenticação

Esse é um projeto de uma API construída em **Java** e **Spring Boot**, utilizando **Spring Security** e **JWT** para controle de autenticação e autorização, **PostgreSQL** como banco de dados e **Flyway** para gerenciamento de migrações.

---

## 📑 Sumário

- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Funcionalidades](#️-funcionalidades)
- [Autenticação e Autorização](#-autenticação-e-autorização)
- [Pré-requisitos](#-pré-requisitos)
- [Configuração do Banco de Dados](#️-configuração-do-banco-de-dados)
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
- Geração e validação de tokens JWT
- Controle de acesso por perfis de usuário (USUÁRIO e ADMIN)
- Proteção de endpoints via Spring Security
- Gerenciamento de versões do banco de dados com Flyway

---

## 🔐 Autenticação e Autorização

O projeto define dois perfis de acesso:

- **USUÁRIO** → Função padrão para usuários autenticados.
- **ADMIN** → Função administrativa, utilizada por parceiros gerentes (ex: registro de novos parceiros).

Para acessar endpoints protegidos como usuário **ADMIN**, é necessário fornecer as credenciais de autenticação apropriadas no cabeçalho da requisição (token JWT via header `Authorization: Bearer <token>`).

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

## ▶️ Como Rodar o Projeto

1. Clone o repositório:
```bash
   git clone https://github.com/gu1gor/auth.git
   cd auth
```

2. Configure o banco de dados conforme a seção anterior.

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

| Método | Endpoint              | Descrição                              | Autenticação |
|--------|------------------------|------------------------------------------|---------------|
| POST   | `/auth/register`       | Cadastra um novo usuário                 | Não           |
| POST   | `/auth/login`          | Autentica um usuário e retorna o token JWT | Não         |
| GET    | `/usuarios`            | Lista usuários cadastrados               | Sim (ADMIN)   |

> Ajuste essa tabela conforme os endpoints reais implementados nos seus Controllers.

Exemplo de requisição de login com `curl`:
```bash
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "usuario@email.com", "senha": "123456"}'
```

Exemplo de requisição autenticada:
```bash
curl -X GET http://localhost:8080/usuarios \
  -H "Authorization: Bearer SEU_TOKEN_JWT"
```

## 👤 Autor

Desenvolvido por **Gustavo Igor**

[![GitHub](https://img.shields.io/badge/GitHub-gu1gor-181717?style=flat&logo=github)](https://github.com/gu1gor)
