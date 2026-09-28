# Nexus — Base de Conhecimento Técnico (`.docs/`)

Bem-vindo à documentação técnica e arquitetural do **Nexus**, o Identity Provider (IdP) centralizado para o ecossistema de produtos independentes.

Este diretório funciona como a memória viva do projeto, orientando decisões de engenharia e fornecendo contexto contínuo para desenvolvedores humanos e agentes autônomos de IA.

---

## 1. Estrutura da Documentação

A documentação está organizada por domínios funcionais e técnicos:

```
.docs/
├── README.md                      # Este índice geral e guia de navegação
├── architecture/                  # Fundamentos de arquitetura e fronteiras do sistema
│   ├── overview.md                # Visão macro, componentes e topologia do IdP
│   ├── principles.md              # Princípios arquiteturais norteadores
│   ├── boundaries.md              # Limites entre Nexus e produtos consumidores
│   └── decisions.md               # Panorama do estado de decisões técnicas
│
├── identity/                      # Modelo de identidade, credenciais e sessões
│   ├── identity-model.md          # Conceito de Nexus User e correlação com produtos
│   ├── authentication.md          # Fluxos de autenticação e ciclo de credenciais
│   ├── authorization.md           # Escopos de identidade vs permissões de negócio
│   └── sessions.md                # Gestão de sessões no IdP e logout global
│
├── protocols/                     # Protocolos abertos de autenticação e tokens
│   ├── oauth2.md                  # Papéis, fluxos e concessões (grants) OAuth 2.0
│   ├── openid-connect.md          # Discovery, UserInfo e emissão de ID Tokens
│   └── tokens.md                  # Estrutura de JWTs, ciclo de vida e rotação de chaves
│
├── security/                      # Modelo de ameaças, segurança de credenciais e auditoria
│   ├── security-model.md          # Princípios de defesa em profundidade e confiança zero
│   ├── threat-model.md            # Análise STRIDE aplicada ao IdP
│   ├── secrets.md                 # Gestão de chaves criptográficas e segredos
│   ├── logging.md                 # Auditoria de segurança e prevenção de vazamento
│   └── security-checklist.md      # Lista de verificação obrigatória para mudanças
│
├── development/                   # Guias práticos para desenvolvimento e engenharia
│   ├── setup.md                   # Pré-requisitos e configuração do ambiente local
│   ├── project-structure.md       # Layout do projeto Maven e padrão de pacotes
│   ├── conventions.md             # Padrões de código Java 21, API REST e commits
│   ├── testing.md                 # Estratégia de testes e pirâmide de cobertura
│   └── troubleshooting.md         # Diagnóstico de problemas recorrentes
│
├── operations/                    # Operação em runtime, observabilidade e deploy
│   ├── configuration.md           # Externalização de propriedades e perfis
│   ├── deployment.md              # Empacotamento de artefatos e imagens OCI
│   ├── observability.md           # Métricas, health checks e rastreabilidade
│   └── backup-and-recovery.md     # Continuidade de negócio e resiliência de dados
│
└── decisions/                     # Architecture Decision Records (ADRs)
    ├── README.md                  # Processo e governança de decisões arquiteturais
    ├── template.md                # Modelo oficial para novas ADRs
    └── 0001-initial-tech-stack-bootstrap.md # Bootstrap da pilha base
```

---

## 2. Taxonomia de Maturidade dos Documentos

Para assegurar **honestidade técnica** e evitar que suposições sejam interpretadas como código pronto, cada conceito e funcionalidade documentada utiliza rigorosamente a seguinte convenção:

| Status | Definição |
| :--- | :--- |
| `[Implementado]` | Código ou configuração existente, funcional e verificável no repositório. |
| `[Decisão Adotada]` | Decisão arquitetural aprovada, com implementação planejada para os próximos ciclos. |
| `[Planejado]` | Funcionalidade prevista no roadmap arquitetural. |
| `[Hipótese]` | Alternativa sob avaliação técnica que ainda carece de validação ou prova de conceito. |
| `[Questão Aberta]` | Ponto de indefinição arquitetural aguardando deliberação explícita. |

---

## 3. Matriz de Leitura Rápida por Papel / Objetivo

| Se você precisa... | Consulte prioritariamente... |
| :--- | :--- |
| Compreender as responsabilidades do Nexus vs Produtos | [`architecture/boundaries.md`](architecture/boundaries.md) e [`architecture/principles.md`](architecture/principles.md) |
| Entender como funciona o usuário e mapeamento de contas | [`identity/identity-model.md`](identity/identity-model.md) |
| Integrar um produto consumidor ao Nexus via OAuth2/OIDC | [`protocols/oauth2.md`](protocols/oauth2.md) e [`protocols/openid-connect.md`](protocols/openid-connect.md) |
| Conhecer a política de tokens e chaves criptográficas | [`protocols/tokens.md`](protocols/tokens.md) e [`security/secrets.md`](security/secrets.md) |
| Avaliar postura de segurança, riscos e auditoria | [`security/security-model.md`](security/security-model.md) e [`security/threat-model.md`](security/threat-model.md) |
| Configurar a máquina e compilar o projeto | [`development/setup.md`](development/setup.md) e [`development/project-structure.md`](development/project-structure.md) |
| Propor ou consultar decisões arquiteturais formais | [`decisions/README.md`](decisions/README.md) |

---

## 4. Como Manter a Documentação

1. **Atualização atômica**: Mudanças no comportamento de autenticação, dependências ou propriedades devem ser acompanhadas da atualização imediata do documento relevante correspondente em `.docs/`.
2. **Registro de decisões**: Decisões com impacto sistêmico relevante (escolha de banco, biblioteca OIDC, modelo de sessão) devem ser registradas no diretório [`decisions/`](decisions/README.md) utilizando o template [`decisions/template.md`](decisions/template.md).
3. **Preservação de contexto**: Nenhuma justificativa arquitetural deve ser removida sem que haja uma ADR formal explicando a motivação de sua substituição.
