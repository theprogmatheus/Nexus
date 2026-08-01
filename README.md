# Nexus

> **Nexus** é uma plataforma moderna de gestão interna (Intranet) desenvolvida com foco em arquitetura limpa, aplicações server-driven e boas práticas de Engenharia de Software.

> **⚠️ Nome provisório:** "Nexus" é apenas um codinome utilizado durante o desenvolvimento. O nome oficial do projeto será definido futuramente.

---

## Objetivos

O projeto nasceu com dois propósitos principais:

* Desenvolver uma intranet moderna utilizando o ecossistema Spring.
* Servir como projeto de referência para estudo, experimentação e demonstração de boas práticas de desenvolvimento backend.

Ao contrário de aplicações SPA tradicionais, o Nexus adota uma arquitetura **Server-Driven UI**, onde o servidor permanece responsável pela maior parte da renderização da interface, reduzindo a complexidade do frontend sem abrir mão de uma experiência moderna para o usuário.

---

## Filosofia do Projeto

O Nexus busca equilibrar simplicidade, produtividade e manutenibilidade.

Alguns princípios adotados durante o desenvolvimento:

* Arquitetura modular (Modular Monolith)
* Backend como principal fonte de verdade
* Interface renderizada no servidor
* Pouco JavaScript, apenas quando agrega valor
* Componentes reutilizáveis
* Forte separação de responsabilidades
* Código limpo e altamente documentado
* Evolução incremental orientada por documentação

---

# Stack Tecnológica

## Backend

* Java 21
* Spring Boot 4
* Spring MVC
* Spring Security
* Spring Data JPA
* Bean Validation

## Frontend

* Thymeleaf
* HTMX
* Alpine.js
* Tailwind CSS
* daisyUI

## Banco de Dados

* PostgreSQL

## Ferramentas

* Maven
* Lombok
* Git

---

# Arquitetura

O projeto será desenvolvido utilizando uma abordagem de **Monólito Modular (Modular Monolith)**.

Ao invés de organizar o código apenas por camadas (`controller`, `service`, `repository`), cada módulo possuirá sua própria estrutura interna.

Exemplo:

```text
users/
├── controller/
├── service/
├── repository/
├── domain/
├── dto/
└── mapper/
```

Essa abordagem reduz acoplamento entre funcionalidades e facilita a evolução do sistema ao longo do tempo.

---

# Objetivos Técnicos

Durante o desenvolvimento serão explorados diversos conceitos modernos do ecossistema Spring, incluindo:

* autenticação e autorização
* controle de permissões
* renderização server-side
* atualizações parciais com HTMX
* componentes reutilizáveis com Thymeleaf
* formulários e validações
* persistência com JPA
* modelagem relacional
* auditoria
* workflows
* testes automatizados
* documentação técnica
* deploy com Docker

---

# Funcionalidades Planejadas

O escopo inicial contempla funcionalidades comuns em sistemas corporativos.

Entre elas:

* autenticação
* gestão de usuários
* departamentos
* perfis e permissões
* dashboard
* comunicados
* solicitações internas
* anexos
* histórico de alterações
* auditoria
* notificações
* configurações do sistema

O backlog completo será documentado na pasta `docs/`.

---

# Estrutura do Projeto

> Em evolução.

A organização seguirá o conceito de módulos independentes dentro de um único monólito.

---

# Status

🚧 Projeto em desenvolvimento.

Atualmente encontra-se na fase de definição arquitetural e preparação da infraestrutura inicial.

---

# Roadmap

* [ ] Estrutura inicial do projeto
* [ ] Configuração do PostgreSQL
* [ ] Integração com Flyway
* [ ] Configuração do Tailwind CSS + daisyUI
* [ ] Layout base com Thymeleaf
* [ ] Sistema de autenticação
* [ ] Dashboard
* [ ] Primeiro módulo funcional
* [ ] Testes automatizados
* [ ] Docker
* [ ] Pipeline de CI

---

# Licença

Este projeto está licenciado sob a licença MIT.

---

# Autor

**Matheus Aguiar**

Java Backend Developer • Estudante de Inteligência Artificial

GitHub: https://github.com/theprogmatheus

