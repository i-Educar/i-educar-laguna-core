# i-Educar Laguna — Core (PE-77/2026)

Distribuição de referência do **core i-Educar 2.12** para o Pregão Eletrônico 77/2026 (Laguna/SC).

Este repositório **não substitui** o código-fonte do core. Ele define:

1. Qual branch/commit base usar  
2. Quais pacotes (módulos) entram na PoC Laguna  
3. Como o monorepo deve ficar organizado  
4. Checklist de conformidade Onda 0 e Onda 1  

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

## Pacotes Laguna (mapa TR → repo)

Ver [`manifest/modules.yml`](./manifest/modules.yml) e [`manifest/repos.md`](./manifest/repos.md).

### Já demonstráveis (Ondas 0–1)

| TR | Módulo | Repositório |
| --- | --- | --- |
| 4.x | Exportação unificada | [i-educar-exportacao-unificada-package](https://github.com/i-Educar/i-educar-exportacao-unificada-package) |
| 5.3 | Aderência escolar | [i-educar-escolar-aderencia-package](https://github.com/i-Educar/i-educar-escolar-aderencia-package) |
| 5.4 | Transporte v2 | [i-educar-transporte-escolar-v2-package](https://github.com/i-Educar/i-educar-transporte-escolar-v2-package) |
| 5.8 | Pré-matrícula | Adaptar [Portabilis](https://github.com/portabilis/pre-matricula-digital) / fork Serventec |
| 5.10 | Educacenso import | Espelho Buriti / JaderGabriel (import) + Portabilis educacenso |

### Onda 2 — motores entregues

Alimentação, almoxarifado, servidor, ACT, horário, eventos, atendimento, biblioteca, solicitação e **apps-api** (com [roadmap Android](https://github.com/i-Educar/i-educar-apps-api-package/blob/main/docs/ROADMAP-ANDROID.md)) — sob [github.com/i-Educar](https://github.com/i-Educar).

**i-Diário:** https://github.com/i-Educar/i-diario (branch `laguna-docs`).

### Onda 3 — Apps API

Sanctum (`familia`|`professor`), endpoints reais filhos/frequência/notas/turmas, OpenAPI e handoff lojas:
https://github.com/i-Educar/i-educar-apps-api-package/tree/main/docs/onda-3


## Layout esperado no monorepo

```text
i-educar/
├── packages/
│   ├── buriti/
│   │   ├── i-educar-exportacao-unificada-package/
│   │   ├── i-educar-escolar-aderencia-package/
│   │   ├── i-educar-transporte-escolar-v2/
│   │   ├── i-educar-educacenso-import-package/
│   │   └── … (demais TR)
│   └── portabilis/
│       ├── pre-matricula-digital/
│       └── i-diario/
├── packages/plug-and-play.json
├── docker-compose.yml
└── .env
```

Padrão técnico dos pacotes Buriti: ver docs no monorepo (`PADRAO-PACOTE-2.12.md`).

## Gates

| Onda | Gate | Status referência |
| ---: | --- | --- |
| 0 | Export ≥3 formatos + ambiente com escolas/turmas/alunos | Finalizada |
| 1 | 5.3 / 5.8 / 5.4 / 5.10 com evidência ≥85% | Finalizada (motores + matrizes; ensaio UI no roteiro R1) |
| 2 | Trilhas A/B/C + apps-api | Motores entregues (`docs/onda-2/STATUS.md`) |
| 3 | Apps Sanctum/OpenAPI/handoff 5.13–5.14 | Finalizada (ver apps-api `docs/onda-3/`) |
| 4 | Endurecimento + ensaio PoC | MVP ( no monorepo + alimentação/transporte/apps/export) |

## Licença

GPL-2.0-or-later (alinhado ao i-Educar).
