# Registro de Decisões Arquiteturais (ADRs)

Este diretório contém os **Architecture Decision Records (ADRs)** do projeto **Nexus**. Cada ADR documenta uma escolha técnica relevante, seu contexto, as alternativas avaliadas e suas consequências.

---

## 1. O que é um ADR?

Um ADR é um documento curto que captura uma decisão arquitetural significativa que afeta a estrutura, as dependências, os contratos ou a segurança do sistema.

### Estados Possíveis de um ADR:
- `PROPOSED`: Proposta submetida para avaliação técnica e discussão.
- `ACCEPTED`: Decisão aprovada e adotada pela equipe de engenharia.
- `REJECTED`: Proposta avaliada, mas rejeitada (mantida no histórico com justificativa).
- `DEPRECATED`: Decisão anteriormente aceita que não é mais recomendada.
- `SUPERSEDED`: Decisão substituída por um ADR posterior (com link para a nova decisão).

---

## 2. Índice de Decisões

| ID | Título | Data | Status |
| :---: | :--- | :---: | :---: |
| [0001](0001-initial-tech-stack-bootstrap.md) | Bootstrap Tecnológico Inicial (Java 21, Spring Boot 4.1.1, Maven) | 2026-09-28 | `ACCEPTED` |
| [0002](0002-spring-authorization-server-adoption.md) | Adoção do Spring Authorization Server como Motor de OAuth2/OIDC | 2026-09-28 | `ACCEPTED` |
| [0003](0003-server-side-rendering-thymeleaf.md) | Adoção de Server-Side Rendering (Thymeleaf) para Telas de Autenticação | 2026-09-28 | `ACCEPTED` |

---

## 3. Como Criar uma Nova Decisão

1. Copie o arquivo [`template.md`](template.md).
2. Nomeie o novo arquivo sequencialmente: `XXXX-titulo-da-decisao.md` (ex: `0002-spring-authorization-server-adoption.md`).
3. Preencha todas as seções obrigatórias: Contexto, Decisão, Alternativas e Consequências.
4. Adicione o novo registro à tabela de índice acima.
