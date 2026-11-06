# TASK-001 — Validar inventário e fontes

## Papel no plano

Estabelecer fatos verificáveis antes de editar a playbooks copy do Alethe.

## Objetivo e definição de pronto

- comparar os arquivos locais com `opencode models`;
- executar `doctor`, `opencode --help` e `opencode run --help`;
- registrar versões, providers detectados e modelos disponíveis;
- conferir billing/quota em fontes oficiais;
- registrar divergências em `docs/squads/SQUAD-alethe-onboarding/STATE.md`.

## Escopo

Pode alterar somente:

- `docs/squads/SQUAD-alethe-onboarding/STATE.md`
- `docs/sources/`

Não alterar credenciais, `opencode.json` ou políticas Alethe.

## Checks

```bash
python --version
python skills/alethe/scripts/orchestrate.py doctor
opencode --help
opencode run --help
opencode models
opencode models google
opencode models nvidia
```

## Evidência

Use as fontes listadas em `docs/prd/PRD-alethe-onboarding-opencode.md`. Não trate listas locais
como confirmação de preço ou quota.

## Handoff

Termine com `STATUS:` e `SUMMARY:`. Liste comandos executados, divergências e arquivos alterados.
