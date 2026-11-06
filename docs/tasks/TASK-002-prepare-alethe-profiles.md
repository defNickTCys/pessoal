# TASK-002 — Preparar proposta de providers, adapters e tiers

## Papel no plano

Transformar o inventário validado em uma proposta revisável pelo onboarding.

## Objetivo e definição de pronto

- atualizar `docs/alethe/onboarding-input.yaml` apenas com dados confirmados;
- manter Claude, Codex e Agy como providers nativos;
- manter Google e NVIDIA como adapters OpenCode separados;
- fixar modelos explicitamente onde possível;
- manter adapters OpenCode como escrita isolada;
- registrar `billing`, tiers, ordem e regras de quota;
- não marcar a proposta como aprovada.

## Escopo

Pode alterar somente:

- `docs/alethe/onboarding-input.yaml`
- `docs/prd/PRD-alethe-onboarding-opencode.md`

## Restrições

- não usar `policy.modelTiers` legado;
- não criar worker read-only customizado;
- não inserir segredos;
- não afirmar quota desconhecida;
- não instalar ou alterar providers.

## Checks

```bash
python - <<'PY'
import yaml
with open("docs/alethe/onboarding-input.yaml", encoding="utf-8") as f:
    yaml.safe_load(f)
print("yaml ok")
PY
```

Se PyYAML não estiver disponível, validar a estrutura manualmente e registrar a limitação.

## Handoff

Termine com `STATUS:` e `SUMMARY:` e aponte quais valores ainda exigem aprovação no `/alethe setup`.
