# Desenvolvimento — Estrutura do Projeto

Este documento detalha o layout de diretórios atual do repositório, o pacote base do Spring Boot e a arquitetura em camadas planejada para as futuras classes do **Nexus**.

---

## 1. Estrutura Atual de Diretórios no Repositório

O projeto segue a estrutura padrão de diretórios do Apache Maven gerada pelo Spring Initializr:

```
Nexus/
├── .mvn/                          # Binários e propriedades do Maven Wrapper
│   └── wrapper/
│       └── maven-wrapper.properties
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── io/github/theprogmatheus/nexus/
│   │   │       └── NexusApplication.java   # Classe principal de bootstrap (@SpringBootApplication)
│   │   └── resources/
│   │       ├── static/                     # Assets estáticos (vazio inicialmente)
│   │       ├── templates/                  # Templates HTML de login (vazio inicialmente)
│   │       └── application.properties     # Configurações do Spring Boot
│   └── test/
│       └── java/
│           └── io/github/theprogmatheus/nexus/
│               └── NexusApplicationTests.java # Teste básico de carregamento de contexto
├── .docs/                         # Memória técnica, arquitetura e decisões (Base de Conhecimento)
├── .gitattributes                 # Configuração de normalização de finais de linha Git
├── .gitignore                     # Arquivos e pastas ignorados pelo controle de versão
├── AGENTS.md                      # Regras de operação para agentes de IA
├── HELP.md                        # Documento de ajuda gerado pelo Spring Initializr
├── mvnw                           # Script executável do Maven Wrapper (Unix/Linux/macOS)
├── mvnw.cmd                       # Script executável do Maven Wrapper (Windows)
├── pom.xml                        # Descritor do projeto, dependências e plugins Maven
└── README.md                      # Documento principal de apresentação do projeto
```

---

## 2. Pacote Base e Convenção de Nomenclatura

- **Group ID**: `io.github.theprogmatheus`
- **Artifact ID**: `nexus`
- **Pacote Raiz**: `io.github.theprogmatheus.nexus`

---

## 3. Arquitetura em Camadas Planejada

À medida que as funcionalidades de identidade forem implementadas, o código sob `io.github.theprogmatheus.nexus` deverá ser organizado nas seguintes camadas conceituais:

```
io.github.theprogmatheus.nexus/
├── config/                        # Configurações do Spring Security, OIDC, Web e Criptografia
│   ├── SecurityConfig.java
│   └── JwkConfiguration.java
├── domain/                        # Entidades centrais de identidade e regras de negócio puras
│   ├── model/                     # NexusUser, ClientRegistration, AccountStatus
│   └── repository/                # Interfaces de persistência (Spring Data JPA)
├── application/                   # Casos de uso e orquestração de serviços
│   ├── service/                   # UserRegistrationService, PasswordResetService
│   └── dto/                       # Records Java para transporte interno
├── infrastructure/                # Implementações técnicas, clientes externos e adaptadores
│   ├── persistence/               # Entidades JPA de mapeamento de banco e queries
│   └── security/                  # Argon2PasswordEncoder, custom TokenCustomizers
└── web/                           # Controladores HTTP e endpoints públicos
    ├── controller/                # Endpoints de login, registro, consentimento e userinfo
    └── exception/                 # GlobalExceptionHandler com Problem Details (RFC 7807/9457)
```

---

## 4. Regras de Fronteira entre Camadas

1. **Domínio Independente**: A camada `domain` deve concentrar as invariantes de identidade sem acoplamento a bibliotecas HTTP externas.
2. **DTOs Imutáveis**: Utilizar `record` do Java 21 para DTOs de entrada e saída.
3. **Sem Acoplamento Circular**: Camadas inferiores nunca devem depender diretamente de camadas superiores (ex: `domain` nunca importa classes de `web`).
