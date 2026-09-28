# ADR-0001: Bootstrap Tecnológico Inicial (Java 21, Spring Boot 4.1.1, Maven)

## Status
`ACCEPTED`

## Data
2026-09-28

---

## Contexto
O Nexus é projetado como um Identity Provider (IdP) centralizado e autoridade de segurança para um ecossistema de produtos e microsserviços independentes. Para iniciar o desenvolvimento de forma padronizada, robusta e compatível com as melhores práticas corporativas, foi gerada uma base inicial utilizando o **Spring Initializr**.

Era necessário definir a versão da linguagem, o framework corporativo de sustentação, o mecanismo de build e os pacotes estruturais de persistência e segurança.

---

## Decisão
Adotar a seguinte composição tecnológica inicial como baseline do repositório:

1. **Linguagem & Runtime**: **Java 21 LTS**. Permite o uso de recursos modernos da linguagem (Records para DTOs imutáveis, Pattern Matching, Sealed Interfaces e suporte nativo a Virtual Threads para alta concorrência).
2. **Framework Principal**: **Spring Boot 4.1.1** (via `spring-boot-starter-parent`), provendo injeção de dependências, ciclo de vida de beans e autoconfiguração.
3. **Gerenciador de Dependências e Build**: **Apache Maven** com **Maven Wrapper (`mvnw`)** versão 3.9.16, assegurando builds idênticos em qualquer ambiente sem necessidade de instalação prévia do Maven.
4. **Camada Web**: `spring-boot-starter-webmvc` utilizando a arquitetura clássica baseada em Servlets (Tomcat embutido).
5. **Segurança**: `spring-boot-starter-security` provendo a cadeia básica de filtros de segurança (`SecurityFilterChain`), gerenciamento de contexto de segurança e codificadores de credenciais.
6. **Persistência**: `spring-boot-starter-data-jpa` provendo abstração de repositórios JPA/Hibernate.
7. **Driver de Banco**: `org.postgresql:postgresql` no escopo `runtime`.
8. **Utilitários de Produtividade**: `org.projectlombok:lombok` para redução de código repetitivo de getters, setters e construtores.
9. **Testes**: Starters modulares (`spring-boot-starter-data-jpa-test`, `spring-boot-starter-security-test`, `spring-boot-starter-webmvc-test`) e JUnit 5.

---

## Alternativas Consideradas

### Alternativa 1: Arquitetura Reativa (Spring WebFlux + R2DBC)
- **Prós**: Alta escalabilidade com menor uso de threads do sistema operacional para I/O massivo não bloqueante.
- **Contras**: Complexidade cognitiva elevada em depuração; ecossistema de autorização e filtros de autenticação do Spring Security é historicamente mais maduro e amplamente testado na pilha Servlet; a introdução de Virtual Threads no Java 21 atenua consideravelmente a necessidade de reatividade pura para serviços de I/O.
- **Motivo da rejeição**: A pilha Servlet padrão (`WebMVC`) é mais estável e compatível com as extensões de Identity Provider.

### Alternativa 2: Gradle como Sistema de Build
- **Prós**: Maior flexibilidade e velocidade incremental em builds multimodulares grandes.
- **Contras**: Configuração mais suscetível a desvios de padrão em DSL Groovy/Kotlin.
- **Motivo da rejeição**: Maven provê rigidez declarativa e ampla conformidade em pipelines corporativos.

---

## Consequências

### Impactos Positivos
- Plataforma estável, moderna e com suporte de longo prazo (Java 21 LTS).
- Ecossistema de segurança maduro e amplamente auditado (Spring Security).
- Facilidade de onboarding de novos desenvolvedores através de convenções conhecidas.

### Impactos Negativos e Débitos Aceitos
- **Autoconfiguração do DataSource**: A presença de `spring-boot-starter-data-jpa` e do driver `postgresql` sem propriedades de conexão ativas em `application.properties` impede a inicialização imediata via `spring-boot:run` até que um banco de dados seja configurado ou perfis sejam criados.
- **Servidor OAuth2 Pendente**: O `pom.xml` inicial inclui `spring-boot-starter-security`, mas ainda não inclui `spring-security-oauth2-authorization-server`. Essa inclusão deverá ser objeto de uma decisão dedicada.
- **Ferramenta de Migração Pendente**: Nenhuma ferramenta de schema migration (Flyway ou Liquibase) está configurada ainda.

---

## Decisões Relacionadas
- Base fundacional para todos os componentes do sistema.
