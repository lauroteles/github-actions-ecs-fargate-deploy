# Java Spring Boot CI/CD Workflow - TaxHub

Este repositório contém um exemplo de **workflow GitHub Actions** para automação de CI/CD de um projeto Java Spring Boot. O pipeline realiza build, testes unitários, criação de imagem Docker, push para Amazon ECR e redeploy no ECS.

---

## 🚀 Funcionalidades

- Build do projeto Spring Boot usando Maven.
- Execução de testes unitários.
- Cache de dependências Maven para acelerar builds.
- Configuração de credenciais AWS via OIDC.
- Build e push de imagem Docker para Amazon ECR.
- Redeploy automático do serviço ECS com a nova imagem.

---

## ⚙️ Variáveis e Secrets

O workflow utiliza **secrets do GitHub** para proteger credenciais sensíveis:

| Variável / Secret               | Descrição                                 |
|--------------------------------|-------------------------------------------|
| `USERNAME`                      | Usuário do banco de dados                 |
| `PASSWORD`                      | Senha do banco de dados                   |
| `DIALECT`                       | Dialeto do banco (ex: `org.hibernate.dialect.PostgreSQLDialect`) |
| `URL`                           | URL de conexão do banco de dados          |
| `DDL`                           | Configuração de DDL do Spring (`update`, `validate`, etc.) |
| `AWS_ROLE_TO_ASSUME`             | Role AWS para OIDC que permitirá acesso ao ECR e ECS |

---

## 📦 Estrutura do Workflow


.github/workflows/java-spring-boot-ci.yml
### Jobs
build-and-test

Ambiente: ubuntu-latest

Setup Java 21 com Temurin

Cache de dependências Maven

Build do projeto (mvn clean install)

Execução de testes unitários (mvn test)

### deploy

Dependente do job build-and-test

Login AWS via OIDC

Testa acesso AWS (aws sts get-caller-identity)

Login no Amazon ECR

Build da imagem Docker

Tag e push da imagem para ECR

Redeploy do serviço ECS (aws ecs update-service --force-new-deployment)