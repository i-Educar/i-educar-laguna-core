# i-Educar Laguna — Core (PE-77/2026)

Distribuição de referência do **core i-Educar 2.12** para o Pregão Eletrônico 77/2026 (Laguna/SC).

Este repositório **não substitui** o código-fonte do core. Ele define:

1. Qual branch/commit base usar  
2. Quais pacotes (módulos) entram na PoC Laguna  
3. Como o monorepo deve ficar organizado  
4. Checklist de conformidade das ondas  
5. Comandos de setup/demo e padrão de UI  

Atualizado em **2026-11-25**.

## Core base

| Item | Valor |
| --- | --- |
| Aplicação | [i-Educar/i-educar](https://github.com/i-Educar/i-educar) |
| Branch | `2.12` |
| Upstream Portábilis | [portabilis/i-educar](https://github.com/portabilis/i-educar) |
| PHP | ≥ 8.3 |
| Laravel | 13.x |
| Banco | PostgreSQL |
| Cache/fila | Redis |

```bash
git clone -b 2.12 git@github.com:i-Educar/i-educar.git i-educar-laguna
cd i-educar-laguna
```

> Instalação completa (Docker + módulos): use o repositório irmão  
> **[i-Educar/i-educar-laguna-install](https://github.com/i-Educar/i-educar-laguna-install)**.

No monorepo já instalado:

```bash
php artisan laguna:setup          # migrate + configs + demos
php artisan laguna:povoar-cidade  # rede ampla + demos de módulos
php artisan laguna:check          # auditoria de ambiente / catálogo
```

Pacote orquestrador: [i-educar-laguna-ambiente-package](https://github.com/i-Educar/i-educar-laguna-ambiente-package).

## Pacotes Laguna (mapa TR → repo)

Ver [`manifest/modules.yml`](./manifest/modules.yml) e [`manifest/repos.md`](./manifest/repos.md).

### Já demonstráveis (Ondas 0–1)

| TR | Módulo | Repositório |
| --- | --- | --- |
| 4.x | Exportação unificada | [i-educar-exportacao-unificada-package](https://github.com/i-Educar/i-educar-exportacao-unificada-package) |
| 5.3 | Aderência escolar | [i-educar-escolar-aderencia-package](https://github.com/i-Educar/i-educar-escolar-aderencia-package) |
| 5.4 | Transporte v2 | [i-educar-transporte-escolar-v2-package](https://github.com/i-Educar/i-educar-transporte-escolar-v2-package) |
| 5.8 | Pré-matrícula | Adaptar [Portabilis](https://github.com/portabilis/pre-matricula-digital) |
| 5.10 | Educacenso import | [JaderGabriel/…](https://github.com/JaderGabriel/i-educar-educacenso-import-package) (espelhar na org) |

### Onda 2 — motores + UI SME

Alimentação, almoxarifado, servidor, ACT, horário, eventos, atendimento, biblioteca, solicitação, **busca ativa** e **apps-api** — sob [github.com/i-Educar](https://github.com/i-Educar) (busca ativa ainda só no monorepo).

**i-Diário:** https://github.com/i-Educar/i-diario (branch `laguna-docs`).

Demos Artisan (rodam no `laguna:setup` / `laguna:povoar-cidade`):

| Comando | Módulo |
| --- | --- |
| `alimentacao:demo` | Alimentação |
| `transporte:simula` | Transporte |
| `busca-ativa:demo` | Busca ativa |
| `solicitacao:demo` | Chamados |
| `atendimento:demo` | Atendimento |
| `eventos:demo` | Eventos / formação |
| `biblioteca:demo` | Biblioteca |
| `almoxarifado:demo` | Almoxarifado |
| `horario:demo` | Horário |
| `act:demo` | Contratações temporárias |
| `exportacao:demo` / `exportacao:demo-criticos` | Exportação |

### Onda 3 — Apps API

Sanctum (`familia` \| `professor`), endpoints reais, OpenAPI e handoff lojas:  
https://github.com/i-Educar/i-educar-apps-api-package/tree/main/docs/onda-3

### Onda 4 — Endurecimento / ensaio PoC

Roteiro demo, ensaio binário e plano ≤15%: monorepo `packages/buriti/docs/onda-4/`.

### Onda 5 — Governança de apps

Suspender usuário / bloquear município: monorepo `packages/buriti/docs/onda-5/` + comandos `apps-api:*`.

## Layout esperado no monorepo

```text
i-educar/
├── packages/
│   ├── buriti/
│   │   ├── i-educar-exportacao-unificada-package/
│   │   ├── i-educar-escolar-aderencia-package/
│   │   ├── i-educar-transporte-escolar-v2/
│   │   ├── i-educar-educacenso-import-package/
│   │   ├── i-educar-busca-ativa-package/
│   │   ├── i-educar-laguna-ambiente-package/
│   │   └── … (demais TR)
│   └── portabilis/
│       ├── pre-matricula-digital/
│       └── i-diario/
├── packages/plug-and-play.json
├── docker-compose.yml
└── .env
```

Padrão técnico dos pacotes Buriti: `PADRAO-PACOTE-2.12.md` no monorepo.

## Padrão de UI (módulos SME)

Contrato em `packages/buriti/docs/UI-MODULOS-LAGUNA.md` e partial `buriti-ui::topo`:

1. **Topo:** botões de acesso (Início · i-Educar · i-Diário) + **nome e descrição só no centro** + ações.  
2. **Lateral:** só ícone/brasão na `.marca` + grupos do `MenuFluxo` — **sem** repetir nome/descrição do módulo.  
3. **Cabeça da tela:** título da página (não do sistema) + lead + ações.

## Gates

| Onda | Gate | Status referência |
| ---: | --- | --- |
| 0 | Export ≥3 formatos + ambiente com escolas/turmas/alunos | Finalizada |
| 1 | 5.3 / 5.8 / 5.4 / 5.10 com evidência ≥85% | Finalizada (motores + matrizes; ensaio UI no roteiro R1) |
| 2 | Trilhas A/B/C + UI SME + demos | Entregue (motores + layouts + demos Laguna) |
| 3 | Apps Sanctum/OpenAPI/handoff 5.13–5.14 | Finalizada (apps-api `docs/onda-3/`) |
| 4 | Endurecimento + ensaio PoC | MVP (`docs/onda-4/`) |
| 5 | Governança auth apps | Finalizada (`docs/onda-5/`) |

## Pendências conhecidas

- Publicar **busca ativa** na org i-Educar.  
- Espelhar **educacenso-import** de JaderGabriel → i-Educar.  
- Atualizar **i-educar-laguna-install** (`clone-modules.sh`) com demos novos e busca ativa.

## Licença

GPL-2.0-or-later (alinhado ao i-Educar).
