<h1 align="center">🧘 One Pilates</h1>

<p align="center">
  <b>Sistema completo de gerenciamento para estúdio de Pilates</b><br>
  Projeto de Extensão — São Paulo Tech School
</p>

<p align="center">
  <img src="https://skillicons.dev/icons?i=react,ts,js,vite,tailwind,sass,java,spring,mysql,rabbitmq,maven,terraform" alt="Stack do projeto" />
</p>

---

## 📖 Sobre o Projeto

O **One Pilates** é um sistema desenvolvido como **Projeto de Extensão Universitária da SPTECH** para o estúdio One Pilates. O objetivo é digitalizar e modernizar a gestão completa do estúdio, desde a captação de novos alunos até o gerenciamento interno do dia a dia.

O projeto é composto por **cinco repositórios** que trabalham em conjunto, formando um ecossistema completo:

| Repositório | Descrição | Tecnologias |
|-------------|-----------|-------------|
| [LandingPage](https://github.com/One-Pilates/LandingPage) | Site institucional público voltado à captação de alunos | React, TypeScript, Vite, Tailwind CSS |
| [Frontend](https://github.com/One-Pilates/Frontend) | Painel de gerenciamento interno do estúdio | React, Vite, Tailwind CSS, SASS |
| [Backend](https://github.com/One-Pilates/Backend) | API REST que centraliza toda a lógica de negócio | Java 17, Spring Boot 3, MySQL, RabbitMQ |
| [Database](https://github.com/One-Pilates/Database) | Scripts SQL de criação e população do banco de dados | MySQL |
| [Docs](https://github.com/One-Pilates/Docs) | Documentação técnica completa do projeto | Markdown |

---

## 🏗️ Arquitetura Geral

```
┌─────────────────────────────────────────────────────────┐
│                     INTERNET / USUÁRIO                  │
└───────────┬─────────────────────────────┬───────────────┘
            │                             │
            ▼                             ▼
┌───────────────────────┐   ┌─────────────────────────────┐
│      Landing Page     │   │     Frontend (SPA)          │
│  (Site Institucional) │   │  (Painel de Gerenciamento)  │
│                       │   │                             │
│  • Home               │   │  • Dashboard                │
│  • Serviços           │   │  • Agendamentos             │
│  • Equipamentos       │   │  • Gestão de Alunos         │
│  • Depoimentos        │   │  • Gestão de Professores    │
│  • Contato            │   │  • Relatórios e KPIs        │
│                       │   │  • Calendário Interativo    │
│  React + TypeScript   │   │                             │
│  Vite + Tailwind CSS  │   │  React + Vite + Tailwind    │
│  Framer Motion        │   │  FullCalendar + Highcharts  │
└───────────────────────┘   └──────────────┬──────────────┘
                                           │ HTTP (REST API / Axios)
                                           ▼
                            ┌─────────────────────────────┐
                            │       Backend (API REST)    │
                            │                             │
                            │  • Autenticação JWT         │
                            │  • Gestão de Agendamentos   │
                            │  • Gestão de Funcionários   │
                            │  • Gestão de Alunos         │
                            │  • Upload de Imagens        │
                            │  • Filas com RabbitMQ       │
                            │  • Swagger/OpenAPI          │
                            │                             │
                            │  Java 17 + Spring Boot 3    │
                            │  Spring Security + JPA      │
                            └──────────┬──────────────────┘
                                       │
                  ┌────────────────────┼────────────────────┐
                  │                    │                    │
                  ▼                    ▼                    ▼
        ┌─────────────┐      ┌──────────────┐    ┌──────────────┐
        │    MySQL    │      │   RabbitMQ   │    │  Arquivos /  │
        │  (Database) │      │  (Mensageria)│    │  Imagens     │
        └─────────────┘      └──────────────┘    └──────────────┘
```

---

## 🔗 Como os Projetos se Interligam

### 1. 🌐 Landing Page → Estúdio
A **Landing Page** é o ponto de entrada público do estúdio. Visitantes conhecem os serviços, equipamentos e depoimentos, e podem entrar em contato via **WhatsApp** ou **formulário de e-mail (EmailJS)**. É totalmente independente do Backend — não requer autenticação.

### 2. 🖥️ Frontend → Backend
O **Frontend** (painel de gerenciamento) consome a **API REST do Backend** via **Axios**. Toda a comunicação é autenticada com **JWT**:

- Ao fazer login, o Backend valida as credenciais e retorna um **token JWT**.
- O Frontend armazena o token e o envia em cada requisição no header `Authorization: Bearer <token>`.
- O controle de acesso é feito por **perfis** (`ADMINISTRADOR`, `PROFESSOR`, `SECRETARIA`), que determinam quais rotas e funcionalidades cada usuário pode acessar.

### 3. ⚙️ Backend → Database
O **Backend** gerencia automaticamente a estrutura do banco via **Spring Data JPA / Hibernate**:

- Em desenvolvimento, `ddl-auto=create` recria as tabelas ao iniciar.
- Em produção, `ddl-auto=update` atualiza o schema sem perder dados.
- O repositório **Database** contém o script `dados_iniciais.sql` para popular o banco com usuários e dados de exemplo.

### 4. 📨 Backend → RabbitMQ
O Backend utiliza **RabbitMQ** para processamento assíncrono de mensagens (notificações, eventos de agendamento), desacoplando operações que não precisam ser síncronas.

---

## 👥 Perfis de Usuário

| Perfil | Descrição | Acesso |
|--------|-----------|--------|
| **Administrador** | Controle total do sistema | Dashboard geral, todos os relatórios, gestão completa |
| **Secretária** | Gestão operacional | Agendamentos, cadastro de alunos e professores, calendário |
| **Professor** | Visão de suas aulas | Calendário pessoal, dashboard com suas turmas |

---

## 🗄️ Modelo de Dados Principal

O banco de dados é gerenciado pelo Backend (Spring JPA) e contém as seguintes entidades principais:

```
Funcionário (herança)
  ├── Administrador
  ├── Professor ──── Especialidade(s)
  └── Secretária

Aluno ──── Agendamento ──── Aula
                │
              Sala ──── Especialidade

Endereço (relacionado a Funcionário e Aluno)
```

**Especialidades disponíveis:**
- Pilates Clássico
- Pilates Aéreo
- Pilates Funcional
- Pilates Terapêutico
- Pilates para Gestantes

**Salas:**
- Sala 1 — Mat Pilates (10 alunos, 2 equipamentos PCD)
- Sala 2 — Equipamentos (8 alunos, 3 equipamentos PCD)
- Sala 3 — Aéreo (6 alunos, 1 equipamento PCD)
- Sala 4 — Terapêutica (5 alunos, 2 equipamentos PCD)

---

## 🚀 Como Rodar o Projeto Completo

### Pré-requisitos

| Ferramenta | Versão mínima |
|------------|---------------|
| Node.js | 18+ |
| npm | 9+ |
| Java | 17+ |
| Maven | 3.8+ |
| MySQL | 8.0+ |
| RabbitMQ | 3.12+ |

### 1. 🗄️ Banco de Dados

```bash
# 1. Inicie o MySQL e crie o banco
mysql -u root -p -e "CREATE DATABASE onePilates;"

# 2. Inicie o RabbitMQ
rabbitmq-server
```

### 2. ⚙️ Backend

```bash
git clone https://github.com/One-Pilates/Backend.git
cd Backend

# Configure src/main/resources/application.properties
# (veja o README do Backend para as variáveis necessárias)

mvn spring-boot:run
# API disponível em http://localhost:8080
# Swagger UI em http://localhost:8080/swagger-ui.html
```

### 3. 📦 Popular o Banco (opcional)

```bash
git clone https://github.com/One-Pilates/Database.git
cd Database
mysql -u root -p onePilates < dados_iniciais.sql
```

### 4. 🖥️ Frontend (Painel de Gerenciamento)

```bash
git clone https://github.com/One-Pilates/Frontend.git
cd Frontend
npm install
npm run dev
# Aplicação em http://localhost:5173
```

### 5. 🌐 Landing Page

```bash
git clone https://github.com/One-Pilates/LandingPage.git
cd LandingPage
npm install
npm run dev
# Aplicação em http://localhost:5173
```

> ⚠️ Se rodar Frontend e Landing Page ao mesmo tempo, configure portas diferentes (`--port 5174` para um deles).

---

## 📦 Repositórios

| Repositório | Link | Status |
|-------------|------|--------|
| 🌐 Landing Page | [One-Pilates/LandingPage](https://github.com/One-Pilates/LandingPage) | ativo |
| 🖥️ Frontend | [One-Pilates/Frontend](https://github.com/One-Pilates/Frontend) | ativo |
| ⚙️ Backend | [One-Pilates/Backend](https://github.com/One-Pilates/Backend) | ativo |
| 🗄️ Database | [One-Pilates/Database](https://github.com/One-Pilates/Database) | ativo |
| 📚 Docs | [One-Pilates/Docs](https://github.com/One-Pilates/Docs) | ativo |

---

## 🛠️ Stack Completa

<p align="center">
  <img src="https://skillicons.dev/icons?i=react,ts,js,vite,tailwind,sass,java,spring,mysql,rabbitmq,maven" alt="Stack do projeto" />
</p>

---

## 📄 Licença

Este projeto é distribuído sob a licença **MIT**.

---

<p align="center">
  Feito com ❤️ pela equipe One Pilates — SPTECH
</p>
