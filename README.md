# API de Autenticação
Esse é um projeto de uma API construida em Java, Spring, Flyway Migration, PostgresSQL com banco de dados e Spring security e JWT para controle de autenticação.

Nessa API configurei a Autenticação e Autorização de um Login de usuário na aplicação Spring usando Spring Security.

# Autenticação
USUÁRIO -> Função de usuário padrão para usuários logados. 

ADMIN -> Função administrativa para parceiros gerentes (registro de novos parceiros).

Para acessar endpoints protegidos como usuário ADMIN, forneça as credenciais de autenticação apropriadas no cabeçalho da solicitação.

# Base de dados
O projeto utiliza PostgresSQL como banco de dados. As migrações de banco de dados necessárias são gerenciadas usando Flyway.
