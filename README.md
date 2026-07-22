<div align="center">

# 🛡️ ASPM Pride Security — Telemetry Dataset

**Public · Sanitized · ECS-formatted security telemetry**

[![License: MIT](https://img.shields.io/badge/License-MIT-informational.svg)](LICENSE)
[![Purpose: Educational](https://img.shields.io/badge/Purpose-Educational%20%2F%20Study-8b5cf6.svg)](#-english)
[![Format: ECS](https://img.shields.io/badge/Schema-Elastic%20Common%20Schema-005571.svg)](https://www.elastic.co/guide/en/ecs/current/index.html)
[![Challenge](https://img.shields.io/badge/Challenge-FIAP%202026-10b981.svg)](#-english)
[![Stack](https://img.shields.io/badge/Source-Java%20·%20Quarkus%20·%20Redis%20Streams-b91c1c.svg)](https://github.com/carmipa/code-with-quarkus-pride)

🇬🇧 [English](#-english) · 🇧🇷 [Português](#-português)

</div>

> [!IMPORTANT]
> ⚠️ **Educational / study project.** This dataset comes from **ASPM Pride Security**, an academic
> project built for the **FIAP 2026 Challenge**. It is published for learning and research on
> security telemetry (SIEM / ML) — **not** a production security feed. No real users, IPs or
> machine data are exposed.

---

## 🏗️ Architecture

How each snapshot is produced — from a live security event to a sanitized public commit:

```mermaid
flowchart LR
    A["🛡️ ASPM Pride Security<br/>(Java · Quarkus)"] -->|"security events (ECS)"| B[("📡 SPN Telemetry<br/>Redis Streams")]
    B -->|"listarRecentes()"| C["🧹 TelemetryDatasetService<br/>sanitize + aggregate"]
    C -->|"strip IPs · anonymize · drop paths"| C
    C -->|"pretty JSON"| D["📦 metrics/<br/>pride-telemetria-dataset.json"]
    C -->|"git add · commit · push"| E[("🌐 GitHub<br/>this dataset repo")]
    F["👤 Operator clicks<br/>'Publicar Dataset'"] -.->|"POST /dataset/publicar"| C
```

| Stage | What happens |
|------|--------------|
| 📡 **Ingest** | The app emits ECS security events to a **Redis Stream** (`XADD`) as it scans, authenticates and audits. |
| 🧹 **Sanitize** | `TelemetryDatasetService` removes source IPs, anonymizes identities and erases disk paths from free-text fields. |
| 📊 **Aggregate** | KPIs by category / type / outcome, threat & failure counts, latency (avg + p95). |
| 📦 **Snapshot** | A pretty-printed JSON is written to `metrics/pride-telemetria-dataset.json`. |
| 🌐 **Publish** | One click → `git commit + push`. Each commit is a snapshot; the Git history is the public timeline. |

---

<a id="-english"></a>

## 🇬🇧 English

Operational **security-telemetry** dataset from [**ASPM Pride Security**](https://github.com/carmipa/code-with-quarkus-pride) — a reactive Java/Quarkus platform that scans source code for OWASP signatures, adds local & remote AI analysis, and streams **ECS (Elastic Common Schema)** events through **Redis Streams**.

This repository exposes **reproducible security metrics and neutral ECS events**, not payloads. Each commit is a dataset snapshot.

### 📁 Repository layout

```text
├── README.md
├── LICENSE                 # MIT
├── .gitignore
└── metrics/
    └── pride-telemetria-dataset.json
```

### 🧬 Data format

Custom UTF-8 JSON with `versaoFormato` for schema evolution.

#### 🖥️ `ambienteExecucao` — execution environment (benchmark context)

| Field | Meaning |
|------|---------|
| `fabricante` / `modeloMaquina` | Generic manufacturer & machine model from the OS |
| `cpu` | Public CPU name |
| `gpuPrincipal` / `gpusDetectadas` | Dedicated GPU auto-selected + all GPUs reported by the OS |
| `ramTotalGb` | Rounded total physical RAM (GB) |
| `sistemaOperacional` / `arquitetura` | Platform — **no** username, hostname, paths, IPs or device IDs |
| `hardwareColetadoAutomaticamente` | Whether values were collected automatically |

#### 📊 `resumo` — aggregate KPIs

| Field | Meaning |
|------|---------|
| `totalEventos` | Events in this snapshot |
| `ameacas` / `criticos` / `falhas` | Threat-category, critical-severity and failure-outcome counts |
| `duracaoMediaMs` / `duracaoP95Ms` | Event latency (ms) — average and 95th percentile |
| `porCategoria` / `porTipo` / `porResultado` | Event counts grouped by ECS `event.category` / `event.type` / `event.outcome` |

#### 🧾 `eventos[]` — sanitized ECS events

Neutral ECS fields only: `@timestamp`, `trace.id`, `span.id`, `event.category`, `event.type`, `event.action`, `event.outcome`, `security.severity`, `rule.id`, `rule.name`, `event.duration`. Source IPs are removed, identities are `anonymous`, and any disk path in free text becomes `<path>`.

### 🔒 Privacy & anonymization

This dataset does **not** publish source IPs, usernames, hostnames, MAC addresses, serial numbers, device identifiers, machine paths, credentials, tokens or API keys. A `pre-commit` guardian blocks accidental secret leakage. Only neutral security metrics and ECS event metadata are shared — aligned with **LGPD / GDPR** minimization.

### ⚙️ Generation

Generated from the **Telemetry panel** of ASPM Pride Security via the **“Publicar Dataset”** button (next to *Imprimir*). The app sanitizes the current telemetry, writes `metrics/pride-telemetria-dataset.json`, commits the snapshot and pushes it here.

### 📜 License

[MIT](LICENSE) — free use with attribution.

---

<a id="-português"></a>

## 🇧🇷 Português

Dataset de **telemetria de segurança** do [**ASPM Pride Security**](https://github.com/carmipa/code-with-quarkus-pride) — uma plataforma Java/Quarkus reativa que varre código-fonte por assinaturas OWASP, adiciona análise por IA local e remota, e transmite eventos **ECS (Elastic Common Schema)** via **Redis Streams**.

Este repositório expõe **métricas de segurança reprodutíveis e eventos ECS neutros**, não payloads. Cada commit é um snapshot do dataset; o histórico Git é a linha do tempo pública.

### 📁 Estrutura do repositório

```text
├── README.md
├── LICENSE                 # MIT
├── .gitignore
└── metrics/
    └── pride-telemetria-dataset.json
```

### 🧬 Formato dos dados

JSON próprio em UTF-8, com `versaoFormato` para evolução do schema.

#### 🖥️ `ambienteExecucao` — ambiente de execução (contexto de benchmark)

| Campo | Significado |
|------|-------------|
| `fabricante` / `modeloMaquina` | Fabricante e modelo genéricos reportados pelo SO |
| `cpu` | Nome público do processador |
| `gpuPrincipal` / `gpusDetectadas` | GPU dedicada selecionada automaticamente + todas as GPUs do SO |
| `ramTotalGb` | RAM física total arredondada (GB) |
| `sistemaOperacional` / `arquitetura` | Plataforma — **sem** usuário, hostname, caminhos, IPs ou IDs de dispositivo |
| `hardwareColetadoAutomaticamente` | Se os valores foram coletados automaticamente |

#### 📊 `resumo` — KPIs agregados

| Campo | Significado |
|------|-------------|
| `totalEventos` | Eventos neste snapshot |
| `ameacas` / `criticos` / `falhas` | Contagens de categoria-ameaça, severidade-crítica e resultado-falha |
| `duracaoMediaMs` / `duracaoP95Ms` | Latência dos eventos (ms) — média e percentil 95 |
| `porCategoria` / `porTipo` / `porResultado` | Contagens por ECS `event.category` / `event.type` / `event.outcome` |

#### 🧾 `eventos[]` — eventos ECS sanitizados

Apenas campos ECS neutros: `@timestamp`, `trace.id`, `span.id`, `event.category`, `event.type`, `event.action`, `event.outcome`, `security.severity`, `rule.id`, `rule.name`, `event.duration`. IPs de origem são removidos, identidades viram `anonymous`, e qualquer caminho de disco no texto livre vira `<path>`.

### 🔒 Privacidade e anonimização

Este dataset **não** publica IPs de origem, nomes de usuário, hostnames, endereços MAC, números de série, identificadores de dispositivo, caminhos de máquina, credenciais, tokens ou chaves de API. Um guardião `pre-commit` bloqueia vazamento acidental de segredos. Só métricas de segurança neutras e metadados de eventos ECS são compartilhados — alinhado à minimização da **LGPD / GDPR**.

### ⚙️ Geração

Gerado pelo **painel de Telemetria** do ASPM Pride Security pelo botão **“Publicar Dataset”** (ao lado de *Imprimir*). O app sanitiza a telemetria atual, escreve `metrics/pride-telemetria-dataset.json`, commita o snapshot e faz push para cá.

### 📜 Licença

[MIT](LICENSE) — uso livre com atribuição.

---

<div align="center">

🎓 **Projeto educacional — Challenge FIAP 2026** · Feito com Java ☕ + Quarkus ⚡ + Redis 🧠

</div>
