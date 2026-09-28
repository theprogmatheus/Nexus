# Desenvolvimento — Convenções de Código e Engenharia

Este documento estabelece as convenções de estilo de código, práticas idiomáticas de Java 21, padrões de API e diretrizes de commits para o **Nexus**.

---

## 1. Padrões de Java 21 Moderno

A base tecnológica adota Java 21 LTS. O código deve priorizar os seguintes recursos da linguagem:

### 1.1. Uso de Java Records
Utilize `record` para todos os objetos de transferência de dados (DTOs), comandos, eventos e payloads imutáveis:

```java
public record UserRegistrationRequest(
    @NotBlank @Email String email,
    @NotBlank @Size(min = 12) String password,
    @NotBlank String displayName
) {}
```

### 1.2. Injeção de Dependências por Construtor
- **Proibido**: Injeção por campo utilizando `@Autowired private MyService myService;`.
- **Obrigatório**: Injeção via construtor com atributos `private final`. O uso de `@RequiredArgsConstructor` do Lombok é incentivado para reduzir boilerplate em classes Spring.

### 1.3. Tratamento de Nulos
- Preferir `Optional<T>` como retorno de consultas em repositórios e serviços de busca.
- Evitar o uso de `Optional` como parâmetro de métodos ou campos de classe.

---

## 2. Padrões de API HTTP e Respostas de Erro

### 2.1. Problem Details para Erros HTTP (RFC 7807 / RFC 9457)
Todas as respostas de erro da API do Nexus devem seguir a especificação Problem Details nativa do Spring Boot:

```json
{
  "type": "https://nexus.auth/errors/invalid-credentials",
  "title": "Unauthorized",
  "status": 401,
  "detail": "Credenciais de autenticação inválidas.",
  "instance": "/oauth2/token"
}
```

### 2.2. Prevenção de Informações Excessivas
- Respostas de erro em ambientes de produção **nunca** devem retornar stack traces, nomes de tabelas SQL ou detalhes internos da JVM.

---

## 3. Padrão de Mensagens de Commit (Conventional Commits)

Os commits no repositório devem seguir a especificação [Conventional Commits](https://www.conventionalcommits.org/):

```
<tipo>(<escopo>): <descrição no imperativo e minúscula>

[corpo opcional explicando o porquê e impactos]

[rodapé opcional com referências a issues ou breaking changes]
```

### Tipos Permitidos:
- `feat`: Nova funcionalidade no sistema.
- `fix`: Correção de bug.
- `docs`: Alterações exclusivamente em documentação (`.docs/`, `README.md`, `AGENTS.md`).
- `refactor`: Refatoração de código que não altera comportamento público nem corrige bug.
- `test`: Adição ou modificação de testes automatizados.
- `chore`: Atualizações de build, dependências ou tarefas de infraestrutura.
- `sec`: *(Opcional)* Melhorias ou correções específicas de endurecimento de segurança.

---

## 4. Regra de Atualização Documental

Sempre que um commit introduzir nova configuração em `application.properties`, novo endpoint público ou alterar uma decisão técnica, o arquivo de documentação correspondente em [`.docs/`](../README.md) **deve** ser alterado no mesmo commit.
