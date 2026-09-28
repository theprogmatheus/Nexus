# Segurança — Gestão de Segredos e Chaves

Este documento define as regras compulsórias para manipulação, armazenamento, injeção e rotação de segredos e chaves criptográficas no **Nexus**.

---

## 1. Classificação de Segredos

| Classe | Descrição | Criticidade | Local de Armazenamento |
| :--- | :--- | :---: | :--- |
| **Chave Privada de Assinatura (JWK)** | Chave assimétrica (RSA/EC) que assina tokens JWT emitidos. | **Extrema** | KMS / Vault / HSM ou env var criptografada em runtime. |
| **Credenciais de Banco de Dados** | Usuário e senha do banco PostgreSQL. | **Alta** | Injetado via Variável de Ambiente (`SPRING_DATASOURCE_PASSWORD`). |
| **Segredos de Clientes (Client Secrets)** | Senhas de autenticação de clientes confidenciais OAuth2. | **Alta** | Armazenados no banco do Nexus **sempre hasheados** (BCrypt/Argon2id). |
| **Chaves de Criptografia Simétrica** | Chaves usadas para criptografar dados sensíveis em repouso. | **Alta** | KMS / Secret Manager dedicado. |

---

## 2. Invariantes de Gestão de Segredos

> [!CAUTION]
> **REGRA DE ZERO SECRETS NO CÓDIGO**:
> Nenhum segredo, senha real, certificado privado ou client secret em texto claro deve ser versionado no Git. Arquivos de configuração versionados (`application.properties`, `.yaml`) devem conter exclusivamente referências a variáveis de ambiente ou placeholders seguros.

1. **Client Secrets são Hasheados**:
   - Assim como as senhas de usuários humanos, os segredos de clientes confidenciais **nunca são salvos em texto claro no banco de dados do Nexus**.
   - O segredo é exibido **uma única vez** ao administrador no momento do cadastro do cliente, e persistido no banco na forma de hash seguro.
2. **Isolamento de Chaves Privadas**:
   - A chave privada que assina os tokens nunca deve trafegar na rede desprotegida, nem ser exportada em endpoints HTTP administrativos.
3. **Injeção via Ambiente**:
   - Em conformidade com os princípios da [12-Factor App](https://12factor.net/config), todas as credenciais de infraestrutura devem ser providas via variáveis de ambiente no container de execução.

---

## 3. Estado Atual no Repositório

- **Verificação**: O arquivo [`src/main/resources/application.properties`](file:///C:/Users/matheus.ferreira/Documents/Nexus/src/main/resources/application.properties) contém apenas `spring.application.name=nexus`. Não há senhas, strings de conexão ou credenciais commitadas.
- **Gitignore**: O arquivo [`.gitignore`](file:///C:/Users/matheus.ferreira/Documents/Nexus/.gitignore) já ignora arquivos de build e IDEs. Recomenda-se adicionar padrões para arquivos locais como `.env`, `*.pem`, `*.p12` e `*.jks`.
