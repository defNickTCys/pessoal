# Squad — Alethe Onboarding

## Missão

Preparar e validar o onboarding do Alethe para providers nativos e APIs via OpenCode sem permitir
substituição silenciosa de provider, modelo ou tier.

## Limites

Este Squad é uma organização de planejamento do projeto. O Alethe continua sendo a autoridade
de execução, seleção de tier, aprovação de waves e integração de resultados.

## Papéis

| Papel | Responsabilidade | Provider recomendado |
|---|---|---|
| `lead` | decisões, aprovação e integração | Claude, Codex ou Agy nativo |
| `inventory-scout` | comandos, modelos e fontes | Claude/Codex/Agy read-only |
| `policy-designer` | adapters, billing, tiers e profiles | Claude/Codex/Agy standard |
| `rate-limit-engineer` | wrapper NVIDIA e testes de janela | provider nativo; escrita isolada |
| `spec-validator` | critérios do PRD e dry-run | Codex/Claude/Agy read-only |
| `quality-reviewer` | riscos, fallback, corrida e subagentes | Codex/Claude/Agy read-only |

OpenCode pode executar uma unidade de API isolada, mas não deve atuar como autoridade do Squad
nem iniciar outro runner Alethe.

## Ordem de execução

1. `TASK-001` — inventário e fontes.
2. `TASK-002` — proposta de profiles, tiers e adapters.
3. aprovação interativa do onboarding.
4. `TASK-003` — doctor, dry-run e validação.
5. somente depois: implementação opcional do rate limiter NVIDIA.

## Contrato de cada worker

Cada brief deve indicar objetivo testável, arquivos próprios, arquivos proibidos, decisões já
resolvidas e comandos de verificação. O worker deve terminar com:

```text
STATUS: DONE | DONE_WITH_CONCERNS | BLOCKED | NEEDS_CONTEXT
SUMMARY: ...
```

## Estado

Consulte `STATE.md`. A ausência de aprovação significa que a política não está pronta.
