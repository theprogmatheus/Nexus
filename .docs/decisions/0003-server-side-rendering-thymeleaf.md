# ADR-0003: Adoção de Server-Side Rendering (Thymeleaf) para Telas de Autenticação

## Status
`ACCEPTED`

## Data
2026-09-28

---

## Contexto
O Nexus é o ponto central onde os usuários finais realizam interações de segurança críticas:
- Login com credenciais primárias (e-mail e senha);
- Consentimento de escopos OIDC (ex: *"Deseja permitir que o Produto A acesse seu e-mail e perfil?"*);
- Recuperação e redefinição de senha;
- Fluxos futuros de MFA (inserção de código OTP / verificação de segurança).

Era necessário decidir a estratégia de apresentação para essas interfaces:
1. Uma **Single Page Application (SPA)** desacoplada (ex: React, Vue), comunicando-se via chamadas REST com o backend; ou
2. **Server-Side Rendering (SSR)** nativo com **Thymeleaf**, acompanhado de estilos modernos em CSS (Tailwind) e componentes reativos leves quando necessário.

---

## Decisão
Adotar **Server-Side Rendering com Thymeleaf** (`spring-boot-starter-thymeleaf`) para todas as telas interativas de identidade do Nexus (login, logout, consentimento, erros e recuperação de conta).

A estilização visual utilizará **Tailwind CSS** para compor uma interface moderna, limpa e responsiva, mantendo a experiência do usuário equivalente à de uma aplicação web de alto padrão. Caso sejam necessários comportamentos dinâmicos pontuais no navegador (ex: validação visual de força de senha ou temporizador de expiração de código), serão utilizados componentes JavaScript minimalistas e declarativos (como **Alpine.js** ou **HTMX**), sem a introdução de um ecossistema complexo de SPA no repositório do IdP.

---

## Alternativas Consideradas

### Alternativa 1: SPA Independente (React / Vue)
- **Prós**: Familiaridade para desenvolvedores frontend puros; possibilidade de reutilizar componentes de design system já escritos em React.
- **Contras**:
  - Complexidade severa na interceptação do fluxo de autorização (`/oauth2/authorize`): o Spring Security armazena o `SavedRequest` na sessão do navegador e espera uma submissão de formulário padrão para efetuar o redirecionamento HTTP com o código de autorização. Com SPA, é necessário construir uma ponte manual para lidar com a troca de credenciais e restaurar a query string original.
  - Aumento da superfície de ataque a XSS (vazamento potencial de estados em memória).
  - Sobrecarga de manutenção de infraestrutura (Node.js, Vite/Webpack, dependências NPM, CORS e alinhamento de rotas de segurança).
- **Motivo da rejeição**: Para a infraestrutura de um IdP, a robustez, a resiliência e a segurança nativa do modelo de redirecionamento HTTP com SSR superam amplamente os benefícios de um SPA.

---

## Consequências

### Impactos Positivos
- **Segurança Nativa**: Total integração com as proteções do Spring Security (CSRF tokens em formulários, cookies de sessão `HttpOnly`, `Secure` e `SameSite`).
- **Performance Instantânea**: Sem tempo de download de pacotes JS pesados para exibir uma tela de login simples; carregamento rápido e direto do HTML renderizado.
- **Simplicidade Operacional**: A aplicação permanece um único artefato Spring Boot (`.jar`), sem dependência de processos de build paralelos com Node/NPM no pipeline de CI/CD.
- **Suporte Nativo ao Spring Authorization Server**: O framework do Spring já possui pontos de extensão projetados especificamente para templates MVC/Thymeleaf na renderização das páginas de login e consentimento.

### Impactos Negativos e Débito Aceito
- A dependência `spring-boot-starter-thymeleaf` deve ser incluída no `pom.xml`.
- Desenvolvedores que atuarem na customização visual das telas do IdP precisam interagir com a sintaxe de atributos do Thymeleaf (`th:*`).

---

## Decisões Relacionadas
- [ADR-0001: Bootstrap Tecnológico Inicial](0001-initial-tech-stack-bootstrap.md)
- [ADR-0002: Adoção do Spring Authorization Server](0002-spring-authorization-server-adoption.md)
