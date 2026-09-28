# ADR-0002: Adoção do Spring Authorization Server como Motor de OAuth2 e OpenID Connect

## Status
`ACCEPTED`

## Data
2026-09-28

---

## Contexto
O Nexus tem como objetivo fundamental atuar como um Identity Provider (IdP) centralizado, exigindo suporte rigoroso aos padrões **OAuth 2.0 (RFC 6749, RFC 7636 - PKCE)** e **OpenID Connect 1.0 (Core, Discovery e JWKS)**.

Era necessário decidir se o projeto utilizaria uma solução pronta de prateleira (como Keycloak ou Zitadel), implementaria os endpoints de especificação manualmente, ou adotaria um framework oficial integrado ao ecossistema Spring.

A premissa do projeto é construir uma solução sob medida, desenhada especificamente para as necessidades do ecossistema de produtos da organização, mantendo total controle e soberania sobre o código-fonte, modelo relacional de banco e fluxos de integração.

---

## Decisão
Adotar o **Spring Authorization Server** (`org.springframework.security:spring-security-oauth2-authorization-server`) como o framework oficial para os protocolos OAuth 2.0 e OIDC no Nexus.

Com essa decisão:
1. Os endpoints padronizados de autorização (`/oauth2/authorize`, `/oauth2/token`, `/oauth2/jwks`, `/oauth2/revoke`, `/oauth2/introspect` e `/.well-known/openid-configuration`) serão providos pelo Spring Authorization Server.
2. A persistência de clientes (`RegisteredClient`), autorizações (`OAuth2Authorization`) e consentimentos (`OAuth2AuthorizationConsent`) será mapeada diretamente para o banco relacional via Spring Data JPA.
3. A equipe mantém controle total sobre o design do modelo de dados (`NexusUser`), claims adicionais nos tokens e regras de ciclo de vida de contas.

---

## Alternativas Consideradas

### Alternativa 1: Soluções Prontas (Turnkey Appliances) — Keycloak ou Zitadel
- **Prós**: Funcionalidades completas prontas out-of-the-box (MFA, painel administrativo, federação).
- **Contras**: Complexidade operacional elevada, consumo substancial de memória (no caso do Keycloak), menor flexibilidade para desenhar o modelo de dados de identidade sob medida e acoplamento a um produto de terceiros.
- **Motivo da rejeição**: A visão estratégica do projeto prioriza uma solução customizada, desenhada conforme as necessidades específicas da organização, sem a rigidez ou a sobrecarga operacional de um appliance pronto.

### Alternativa 2: Implementação Manual de Endpoints OAuth2/OIDC
- **Prós**: Zero bibliotecas de terceiros para autorização.
- **Contras**: Risco crítico de segurança; complexidade exorbitante para implementar e certificar fluxos criptográficos de chaves JWK, tokens JWT (RFC 9068, RFC 7519), validação de PKCE e specs do OpenID Connect.
- **Motivo da rejeição**: Nunca reinventar protocolos criptográficos ou mecanismos de segurança quando existe um framework oficial maduro e mantido pelo ecossistema Spring.

---

## Consequências

### Impactos Positivos
- Soberania total sobre o código, banco de dados e arquitetura da aplicação.
- Código 100% idiomático em Java 21 e Spring Boot 4.x.
- Garantia de conformidade com as RFCs de OAuth2 e OIDC mantida pela equipe do Spring Security.
- Ausência de infraestruturas pesadas adicionais: o Nexus roda como uma aplicação Spring Boot direta e leve.

### Impactos Negativos e Débito Aceito
- Telas de consentimento, fluxos de registro e painéis administrativos de clientes precisarão ser codificados pela equipe (não vêm prontos em uma interface gráfica administrativa como no Keycloak).
- A dependência `spring-security-oauth2-authorization-server` precisará ser adicionada formalmente ao `pom.xml` e configurada via `@Configuration`.

---

## Decisões Relacionadas
- [ADR-0001: Bootstrap Tecnológico Inicial](0001-initial-tech-stack-bootstrap.md)
- [ADR-0003: Adoção de Server-Side Rendering com Thymeleaf](0003-server-side-rendering-thymeleaf.md)
