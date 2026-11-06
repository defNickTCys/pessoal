# PRD — Onboarding Alethe com OpenCode, Google AI Studio e NVIDIA NIM

## Status

Proposta de onboarding. Não representa uma política Alethe aprovada.

## Objetivo

Preparar o onboarding do Alethe para:

- manter Claude, Codex e Agy nos adaptadores nativos;
- usar OpenCode apenas como adapter customizado para Google AI Studio e NVIDIA NIM;
- impedir que um agente substitua silenciosamente o provider ou modelo escolhido pelo Alethe;
- classificar tiers, billing, ordem de providers e isolamento antes da primeira execução;
- preservar evidência verificável para cada decisão.

## Decisões validadas

1. O Alethe possui adaptadores nativos para Codex, Claude Code e Agy.
2. OpenCode é suportado como CLI customizada quando `opencode --help` e `opencode run --help` confirmam um modo não interativo.
3. Adapters customizados não garantem read-only, resume, tokens ou quota; tarefas de escrita devem usar isolamento.
4. O onboarding deve listar CLIs, modelos, billing e quota antes de propor `base`, `savings` e `aggressive`, e só salvar após aprovação.
5. Alethe usa tiers `cheap`, `standard` e `strong` no playbook; novas escolhas devem ficar no playbooks copy, não em overrides legados.
6. Google e NVIDIA devem ser tratados como API/custom até que uma leitura de quota seja implementada e validada.
7. A cadeia OpenCode → subagentes não deve ser usada como worker Alethe sem uma política explícita, porque o Alethe não controla nem mede completamente esse consumo.

## Escopo

### Incluído

- inventário dos modelos presentes neste repositório;
- proposta de providers e adapters;
- proposta de billing e ordem de fallback;
- critérios de tier;
- proteção contra troca silenciosa de modelo/provider;
- plano de rate limiting para NVIDIA;
- briefs de TASK e Squad para executar o onboarding.

### Fora de escopo

- salvar uma política Alethe definitiva sem aprovação interativa;
- instalar, alterar ou reconfigurar credenciais;
- afirmar que Google AI Pro equivale automaticamente a quota da Gemini API;
- implementar neste PRD um rate limiter de produção;
- habilitar subagentes OpenCode dentro de workers Alethe.

## Arquitetura proposta

```text
Alethe (lead e política)
├── claude       -> Claude Pro/Claude Code, adapter nativo
├── codex        -> ChatGPT Plus/Codex, adapter nativo
├── agy          -> Antigravity, adapter nativo
├── opencode-google
│   └── opencode run --format json --model google/<modelo>
└── opencode-nvidia
    └── wrapper de rate limit -> opencode run --format json --model nvidia/<modelo>
```

O modelo deve ser fixado no adapter ou no tier aprovado. OpenCode não deve receber um
adapter genérico que permita escolher outro modelo por default.

## Inventário deste repositório

- Modelos Google/OpenCode: `api_google/modelos_listados_opencode`
- Modelos Google e regras de billing: `api_google/*.yaml`
- Modelos NVIDIA: `nvidia_models`
- Modelos gratuitos do catálogo OpenCode: `opencode_custom_free_models`
- Índice e governança Google: `api_google/INDEX.md`

O inventário é uma fonte de entrada do onboarding, não uma confirmação independente de preço,
quota ou disponibilidade. Modelos precisam ser conferidos por `opencode models`, pelo painel
do projeto e por fontes oficiais antes da aprovação.

## Proposta de classificação

| Provider | Billing proposto | Tier inicial | Capacidade |
|---|---|---|---|
| `claude` | `subscription` | `standard`/`strong` | leitura, escrita, revisão e resume nativos |
| `codex` | `subscription` | `cheap`/`standard` | leitura, escrita, revisão e quota nativos |
| `agy` | `subscription` | `standard`/`strong` | leitura, escrita, revisão e quota nativos |
| `opencode-google` | `api` | `cheap`/`standard` por modelo | somente escrita isolada; quota externa |
| `opencode-nvidia` | `api` | `cheap`/`standard` por modelo | somente escrita isolada; rate limit externo |

Os tiers são uma proposta. O `/alethe setup` deve confirmar modelos concretos, preços,
billing e ordem antes de salvá-los.

## Regras de segurança e seleção

- Workers read-only devem usar `claude`, `codex` ou `agy`.
- Workers OpenCode devem usar `mode: write` e `isolate: true`.
- Nenhum worker deve iniciar outro runner Alethe.
- Não usar `--auto`, `--yolo` ou equivalente sem autorização explícita.
- Validar com `dry-run` que cada worker mostra o provider e modelo esperados.
- Qualquer troca de provider deve ser registrada como decisão do lead, não deixada ao worker.
- Para NVIDIA, limitar concorrência e usar wrapper compartilhado com janela de 60 segundos.
- Para Google, monitorar limites por projeto e billing no AI Studio/Google Cloud.

## Critérios de aceite

- [ ] `doctor` detecta os CLIs esperados e registra ausências sem inventar providers.
- [ ] `opencode --help`, `opencode run --help` e `opencode models` foram executados.
- [ ] Cada modelo aprovado tem origem, data de verificação e provider explícito.
- [ ] `opencode-google` e `opencode-nvidia` aparecem como adapters separados.
- [ ] `claude`, `codex` e `agy` permanecem providers nativos.
- [ ] Tiers `cheap`, `standard` e `strong` estão definidos na playbooks copy.
- [ ] Billing API/subscription está registrado somente após confirmação.
- [ ] O dry-run não contém worker OpenCode read-only nem modelo implícito.
- [ ] A política não é considerada pronta antes da aprovação interativa do onboarding.

## Fontes primárias e repositórios

### Alethe

- Repositório: https://github.com/Kc1t/alethe-orchestrate-skill
- Onboarding: https://github.com/Kc1t/alethe-orchestrate-skill/blob/main/skills/alethe/references/onboarding.md
- Playbooks, providers, tiers e quota: https://github.com/Kc1t/alethe-orchestrate-skill/blob/main/skills/alethe/references/playbooks.md
- Adapters customizados e limitações: https://github.com/Kc1t/alethe-orchestrate-skill/blob/main/skills/alethe/references/providers.md
- Briefs, reviews e Squad operacional: https://github.com/Kc1t/alethe-orchestrate-skill/blob/main/skills/alethe/references/briefs.md
- Operações, dry-run, budgets e quota: https://github.com/Kc1t/alethe-orchestrate-skill/blob/main/skills/alethe/references/operations.md
- Integração do editor com provider customizado OpenCode: https://github.com/Kc1t/alethe-orchestrate-skill/pull/1

### OpenCode

- CLI e `opencode run --format json --model`: https://dev.opencode.ai/docs/cli/
- Agents e subagents: https://opencode.ai/docs/agents/
- Providers e credenciais: https://opencode.ai/docs/providers/
- Repositório de integração Alethe/OpenCode: https://github.com/Kc1t/alethe-agents

### Google

- Planos Google AI e AI Studio: https://aistudio.google.com/docs/google-ai-plans
- Rate limits por projeto, RPM/TPM/RPD: https://aistudio.google.com/docs/rate-limits
- Billing da Gemini API e limites de conta: https://ai.google.dev/gemini-api/docs/billing

### NVIDIA

- API Catalog Quickstart: https://docs.api.nvidia.com/nim/docs/api-quickstart
- FAQ oficial do API Catalog, incluindo limite de até 40 RPM na camada gratuita: https://build.nvidia.com/api/runtime/content/help-center-faq.yaml
- Documentação geral NIM: https://docs.nvidia.com/nvidia-nim-document-center/index.html

## Adequação desta sessão

Esta sessão é adequada para pesquisa, validação documental, criação do PRD, preparação de
briefs e organização do repositório. Ela não é suficiente para concluir o onboarding operacional:
o `/alethe setup` precisa rodar no ambiente que contém os CLIs, autenticações, modelos e quotas
locais, e precisa receber a aprovação do usuário antes de salvar a política.
