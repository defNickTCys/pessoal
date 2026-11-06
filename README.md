# Alethe Onboarding Config

Documentação e artefatos de preparação para o onboarding do Alethe, com providers nativos
(Claude, Codex e Agy) e adapters OpenCode para APIs Google e NVIDIA.

## Estrutura

### Planejamento e execução

- [PRD do onboarding](docs/prd/PRD-alethe-onboarding-opencode.md)
- [Input de preparação do Alethe](docs/alethe/onboarding-input.yaml)
- [TASK-001 — validar inventário e fontes](docs/tasks/TASK-001-validate-inventory-and-sources.md)
- [TASK-002 — preparar profiles, adapters e tiers](docs/tasks/TASK-002-prepare-alethe-profiles.md)
- [TASK-003 — executar onboarding e dry-run](docs/tasks/TASK-003-run-onboarding-and-dry-run.md)
- [Squad Alethe Onboarding](docs/squads/SQUAD-alethe-onboarding/README.md)
- [Estado atual do Squad](docs/squads/SQUAD-alethe-onboarding/STATE.md)
- [Índice de fontes](docs/sources/README.md)

### Inventários de modelos

O inventário principal da API Google deve ficar em uma pasta irmã deste repositório no workspace
do agente. A partir da raiz deste repositório, o caminho relativo esperado é:

- [API Google e levantamento de modelos](../api_google/INDEX.md)

Os inventários auxiliares que já acompanham este repositório são:

- [Modelos Google listados pelo OpenCode](api_google/modelos_listados_opencode)
- [Modelos NVIDIA](nvidia_models)
- [Modelos gratuitos do OpenCode](opencode_custom_free_models)

Se `api_google` permanecer dentro deste checkout em vez de ser criada como pasta irmã, use
[`api_google/INDEX.md`](api_google/INDEX.md) como fallback local.

## Como usar

1. Revise o [PRD](docs/prd/PRD-alethe-onboarding-opencode.md) e o
   [estado do Squad](docs/squads/SQUAD-alethe-onboarding/STATE.md).
2. Execute `TASK-001` no ambiente que contém os CLIs e autenticações.
3. Rode `/alethe setup` e aprove providers, modelos, billing, tiers e profile.
4. Execute `doctor`, `playbook --pick` e `dry-run` antes da primeira wave.

O conteúdo deste repositório é uma proposta de preparação. A política Alethe só deve ser
considerada aprovada após o onboarding interativo e a validação no ambiente real.
