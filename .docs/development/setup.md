# Desenvolvimento — Guia de Configuração Local (Setup)

Este documento orienta a configuração do ambiente de desenvolvimento local para compilar, testar e executar o **Nexus**.

---

## 1. Pré-Requisitos de Ambiente

Para trabalhar no Nexus, são necessárias as seguintes ferramentas instaladas:

| Ferramenta | Versão Mínima | Descrição |
| :--- | :--- | :--- |
| **Java Development Kit (JDK)** | **Java 21 LTS** | Eclipse Temurin, GraalVM, OpenJDK ou Amazon Corretto. |
| **Apache Maven** | 3.9.x | Opcional se utilizar o Maven Wrapper incluído (`./mvnw`). |
| **Git** | 2.x+ | Controle de versão. |
| **Docker / Podman** | 24.x+ | *(Recomendado para execução futura do banco de dados PostgreSQL)*. |

---

## 2. Configuração de IDE (Processamento de Anotações / Lombok)

O projeto utiliza **Lombok** para redução de código boilerplate (`@Getter`, `@RequiredArgsConstructor`, etc.).

### IntelliJ IDEA:
1. Abra as preferências (`Settings` / `Preferences`).
2. Navegue até **Build, Execution, Deployment** -> **Compiler** -> **Annotation Processors**.
3. Marque a opção: `Enable annotation processing`.

### VS Code:
1. Instale o pacote de extensões: `Extension Pack for Java` da Microsoft.
2. Instale a extensão: `Lombok Annotations Support for VS Code`.

---

## 3. Compilação e Execução

### 3.1. Compilando o Projeto
Para verificar se as dependências foram baixadas e o código compila corretamente:

No Linux / macOS / Git Bash:
```bash
./mvnw clean compile
```

No Windows PowerShell / CMD:
```powershell
.\mvnw.cmd clean compile
```

### 3.2. Executando a Suíte de Testes
```bash
./mvnw test
```

### 3.3. Executando a Aplicação
```bash
./mvnw spring-boot:run
```

> [!WARNING]
> **Aviso de Execução Atual**: O projeto inclui a dependência `spring-boot-starter-data-jpa` e `postgresql`, mas o arquivo `application.properties` ainda não define uma URL de conexão ativa (`spring.datasource.url`). Para inicializar a aplicação localmente sem um banco PostgreSQL ativo, consulte o guia de [Troubleshooting](troubleshooting.md).
