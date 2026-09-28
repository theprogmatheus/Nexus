# Operações — Configuração e Variáveis de Ambiente

Este documento especifica o modelo de configuração do **Nexus**, as variáveis de ambiente esperadas e o mapeamento de perfis de execução (Spring Profiles).

---

## 1. Filosofia de Configuração (12-Factor App)

O Nexus adota o princípio de **separação estrita entre código e configuração**. Nenhum parâmetro dependente de ambiente (URLs de banco, segredos, portas) deve ser embutido no artefato compilado.

O Spring Boot resolve propriedades na seguinte ordem de precedência:
1. Variáveis de Ambiente do Sistema Operacional / Container.
2. Argumentos de linha de comando (`--property=value`).
3. Arquivo `application-{profile}.properties` específico do perfil ativo.
4. Arquivo padrão [`src/main/resources/application.properties`](file:///C:/Users/matheus.ferreira/Documents/Nexus/src/main/resources/application.properties).

---

## 2. Estado Atual de Configuração

Atualmente, o arquivo `src/main/resources/application.properties` contém apenas a definição do nome da aplicação:

```properties
spring.application.name=nexus
```

---

## 3. Matriz de Propriedades e Variáveis de Ambiente Planejadas

À medida que os componentes de persistência e segurança forem implementados, as seguintes variáveis de ambiente serão consumidas:

| Propriedade Spring | Variável de Ambiente | Descrição | Exemplo |
| :--- | :--- | :--- | :--- |
| `server.port` | `SERVER_PORT` | Porta HTTP da aplicação. | `8080` |
| `spring.datasource.url` | `SPRING_DATASOURCE_URL` | URL JDBC do banco de dados PostgreSQL. | `jdbc:postgresql://postgres:5432/nexus` |
| `spring.datasource.username` | `SPRING_DATASOURCE_USERNAME` | Usuário do banco de dados. | `nexus_app` |
| `spring.datasource.password` | `SPRING_DATASOURCE_PASSWORD` | Senha do banco de dados. | `(Injetada via secret)` |
| `nexus.issuer.url` | `NEXUS_ISSUER_URL` | URL canônica pública do IdP (claim `iss`). | `https://auth.nexus.internal` |
| `nexus.jwk.key-id` | `NEXUS_JWK_KEY_ID` | Identificador da chave ativa de assinatura. | `nexus-key-2026a` |

---

## 4. Perfis Recomendados (Spring Profiles)

- **`local`**: Configurado para desenvolvedores locais, podendo utilizar banco local ou Docker Compose com logs detalhados em console.
- **`test`**: Ativado automaticamente pela suíte de testes com banco em memória ou Testcontainers.
- **`prod`**: Endurecido para produção: logs formatados em JSON, TLS obrigatório, endpoints de diagnóstico restritos.
