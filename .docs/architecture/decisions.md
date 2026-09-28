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
| **Servidor OAuth2/OIDC** | Spring Authorization Server (`spring-security-oauth2-authorization-server`) | `[Decisão Adotada]` | [ADR-0002](../decisions/0002-spring-authorization-server-adoption.md) |
| **Interface do IdP (UI)** | Server-Side Rendering (Thymeleaf + Tailwind CSS) | `[Decisão Adotada]` | [ADR-0003](../decisions/0003-server-side-rendering-thymeleaf.md) |
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
2. **Motor OAuth2/OIDC com Spring Authorization Server**: Construção sob medida utilizando o framework oficial do Spring ([ADR-0002](../decisions/0002-spring-authorization-server-adoption.md)).
3. **UI de Identidade com Thymeleaf (SSR)**: Páginas de login, consentimento e recuperação renderizadas no servidor com foco em segurança nativa e performance ([ADR-0003](../decisions/0003-server-side-rendering-thymeleaf.md)).
4. **Identificador Imutável (`sub`)**: UUID v4 emitido pelo Nexus como chave de correlação global.
5. **Obrigatoriedade de PKCE**: Nenhuma aplicação pública (SPA ou Mobile) utilizará Authorization Code sem PKCE.
6. **Proibição de ROPC e Implicit Flow**: Os fluxos legados Resource Owner Password Credentials e Implicit Grant não serão suportados.

---

## 3. Principais Questões Abertas (TBD)

1. **Ferramenta de Versionamento de Schema (Database Migrations)**:
   - *Opção A*: Flyway.
   - *Opção B*: Liquibase.
   - *Status*: Pendente de definição antes de criar a primeira entidade JPA.
2. **Estratégia de Rotação de Chaves Criptográficas (JWK)**:
   - Definição do armazenamento de chaves assimétricas (KMS, Vault ou chave em banco criptografado).
3. **Provedor de Sessão Distribuída**:
   - Definição entre sessão local de Tomcat ou Spring Session com Redis para clusters em alta disponibilidade.

Para consultar as decisões registradas detalhadamente, acesse o diretório [`.docs/decisions/`](../decisions/README.md).
