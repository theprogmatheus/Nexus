# Arquitetura — Registro e Panorama de Decisões

Este documento consolida o estado atual das decisões arquiteturais tomadas para o **Nexus**, separando com clareza o que já está acordado do que ainda depende de deliberação técnica.

---

## 1. Quadro de Decisões Arquiteturais

| Domínio Técnico | Assunto | Status | Detalhes / Referência |
| :--- | :--- | :---: | :--- |
| **Pilha Base** | Java 21 LTS + Spring Boot 4.1.1 + Maven | `[Implementado]` | [ADR-0001](../decisions/0001-initial-tech-stack-bootstrap.md) |
| **Paradigma Web** | Servlet síncrono (`spring-boot-starter-webmvc`) | `[Implementado]` | [ADR-0001](../decisions/0001-initial-tech-stack-bootstrap.md) |
| **Persistência Base** | Spring Data JPA com driver PostgreSQL em runtime | `[Implementado]` | `pom.xml` inicial |
| **Protocolo de Identidade** | Adoção de OAuth 2.0 e OpenID Connect 1.0 | `[Decisão Adotada]` | [Princípios](../architecture/principles.md) |
| **Desacoplamento de Domínio** | Separação entre identidade (`sub`) e regras de produtos | `[Decisão Adotada]` | [Fronteiras](../architecture/boundaries.md) |
| **Servidor OAuth2/OIDC** | Spring Authorization Server vs implementação customizada | `[Questão Aberta]` | Avaliando inclusão no `pom.xml` |
| **Banco de Dados em Produção** | Definição final do PostgreSQL gerenciado e migrações | `[Hipótese]` | Driver presente; Flyway vs Liquibase TBD |
| **Algoritmo de Hashing** | BCrypt vs Argon2id para senhas | `[Questão Aberta]` | Recomendação técnica: Argon2id |
| **Formato dos Access Tokens** | JWT assinado (Self-contained) vs Opaque Tokens | `[Decisão Adotada]` | JWT com assinatura RS256/ES256 |
| **Armazenamento de Sessões** | Sessão HTTP local em memória vs Redis distribuído | `[Questão Aberta]` | Necessário para alta disponibilidade |
| **Mecanismo de MFA** | TOTP (RFC 6238) / WebAuthn FIDO2 | `[Planejado]` | Prioridade para Fase 2 |

---

## 2. Decisões Implementadas vs Decisões Adotadas

### Decisões Implementadas (Fatos de Código)
1. **Linguagem Java 21**: Escolha oficial como base tecnológica para suporte a virtual threads, records, pattern matching e suporte LTS.
2. **Spring Boot 4.1.1**: Framework estrutural para injeção de dependência e ciclo de vida do serviço.
3. **Build com Apache Maven**: Utilização do Maven Wrapper (`mvnw`) versão 3.9.16 para builds reproduzíveis.
4. **Starter Web MVC & Security**: Configuração inicial de Servlet container (Tomcat embutido) e cadeia de filtros Spring Security.

### Decisões Adotadas (Diretrizes Arquiteturais Aprovadas)
1. **Nexus é estritamente um IdP**: Não comportará regras de negócio dos produtos consumidores.
2. **Identificador Imutável (`sub`)**: UUID v4 ou similar emitido pelo Nexus como chave de correlação global.
3. **Obrigatoriedade de PKCE**: Nenhuma aplicação pública (SPA ou Mobile) utilizará Authorization Code sem PKCE.
4. **Proibição de ROPC e Implicit Flow**: Os fluxos legados Resource Owner Password Credentials e Implicit Grant não serão suportados.

---

## 3. Principais Questões Abertas (TBD)

1. **Framework para OAuth2 Authorization Server**:
   - *Opção A*: Adicionar `org.springframework.security:spring-security-oauth2-authorization-server` ao `pom.xml`.
   - *Opção B*: Implementação sob medida sobre os filtros do Spring Security (Desencorajada pela complexidade criptográfica e risco de segurança).
   - *Status*: Recomendada Opção A via ADR formal.
2. **Ferramenta de Versionamento de Schema (Database Migrations)**:
   - *Opção A*: Flyway.
   - *Opção B*: Liquibase.
   - *Status*: Pendente de definição antes de criar a primeira entidade JPA.
3. **Estratégia de Rotação de Chaves Criptográficas (JWK)**:
   - Definição do armazenamento de chaves assimétricas (KMS, Vault ou chave em banco criptografado).

Para consultar as decisões registradas detalhadamente, acesse o diretório [`.docs/decisions/`](../decisions/README.md).
