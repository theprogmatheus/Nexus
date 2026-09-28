# Segurança — Auditoria e Diretrizes de Logging

Este documento define os requisitos de auditoria de segurança e as regras mandatórias de sanitização para evitar o vazamento inadvertido de dados sensíveis e credenciais nos logs do **Nexus**.

---

## 1. O que DEVE ser Registrado (Auditoria de Segurança)

Para garantir rastreabilidade em investigações de incidentes e conformidade regulatória, os seguintes eventos devem ser logados em nível `INFO` ou `WARN` com formato estruturado:

| Evento | Nível | Dados Permitidos no Log |
| :--- | :---: | :--- |
| **Tentativa de Login Bem-sucedida** | `INFO` | `timestamp`, `event=AUTHN_SUCCESS`, `user_sub`, `client_id`, `ip_address`, `user_agent` |
| **Tentativa de Login Falha** | `WARN` | `timestamp`, `event=AUTHN_FAILURE`, `identifier_hash` (ou e-mail), `reason=INVALID_CREDENTIALS`, `ip_address` |
| **Bloqueio de Conta** | `WARN` | `timestamp`, `event=ACCOUNT_LOCKED`, `user_sub`, `ip_address` |
| **Emissão de Token OAuth2** | `INFO` | `timestamp`, `event=TOKEN_ISSUED`, `grant_type`, `client_id`, `user_sub` (se aplicável), `token_id_jti` |
| **Revogação de Token** | `INFO` | `timestamp`, `event=TOKEN_REVOKED`, `token_type`, `client_id` |
| **Alerta de Reuso de Refresh Token** | `ERROR` | `timestamp`, `event=REFRESH_TOKEN_REUSE_DETECTED`, `client_id`, `ip_address`, `family_id` |
| **Alteração de Senha / Recuperação** | `INFO` | `timestamp`, `event=PASSWORD_RESET_COMPLETED`, `user_sub`, `ip_address` |
| **Cadastro / Modificação de Cliente** | `INFO` | `timestamp`, `event=CLIENT_UPDATED`, `client_id`, `admin_actor_sub` |

---

## 2. O que NUNCA Deve ser Registrado (Regras de Redação / Máscara)

> [!CAUTION]
> **PROIBIÇÃO EXPRESSA DE LOG DE CREDENCIAIS E TOKENS**:
> É estritamente vedado registrar o conteúdo de qualquer dado confidencial em qualquer nível de log (`DEBUG`, `INFO`, `WARN`, `ERROR`):

1. **Senhas**: Nunca logar a senha recebida em texto claro nem o hash da senha gerado.
2. **Tokens Completos**: Nunca registrar em log o valor completo de Access Tokens, Refresh Tokens ou ID Tokens (apenas seu identificador único `jti`, caso necessário).
3. **Authorization Codes**: Nunca logar o código temporário emitido no fluxo de autorização.
4. **Client Secrets**: Nunca registrar senhas ou segredos de clientes OAuth2.
5. **Dados de Cartão / Pagamento**: O Nexus não processa pagamentos e não deve receber esses dados.

---

## 3. Padrão Técnico Recomendado

- **Formato Estruturado (JSON)**: Recomendada a utilização de layout JSON em ambientes de produção para ingestão direta por ferramentas de observabilidade (ex: OpenSearch, Datadog, CloudWatch).
- **Rastreabilidade com MDC (Mapped Diagnostic Context)**:
  - Injetar em cada requisição HTTP um identificador único de correlação: `correlation_id`.
  - Garantir propagação do `correlation_id` em todas as linhas de log disparadas durante o ciclo de vida da requisição.
