# Estado — Squad Alethe Onboarding

## Estado atual

`PROPOSAL_ONLY`

## Evidências disponíveis

- Inventário local Google: `api_google/modelos_listados_opencode`
- Inventário local NVIDIA: `nvidia_models`
- Inventário local OpenCode: `opencode_custom_free_models`
- PRD e fontes: `docs/prd/PRD-alethe-onboarding-opencode.md`
- Input de preparação: `docs/alethe/onboarding-input.yaml`

## Bloqueios

- Não há evidência local nesta sessão de que `opencode`, `claude`, `codex` e `agy` estejam
  instalados e autenticados no ambiente que executará o onboarding.
- Os modelos e limites locais precisam ser comparados com os comandos atuais e os painéis oficiais.
- A playbooks copy do Alethe ainda não foi aprovada.
- O rate limiter NVIDIA ainda não foi implementado.

## Decisões pendentes

- models concretos para `cheap`, `standard` e `strong`;
- ordem final dos providers por profile;
- billing confirmado por provider;
- manter passive padrão, limitar ao projeto ou desligar;
- necessidade e desenho do wrapper NVIDIA;
- qualquer uso permitido de subagentes OpenCode.

## Próximo passo

Executar `TASK-001` no ambiente real e, depois, `/alethe setup`. Não iniciar workers OpenCode antes
de revisar o `dry-run`.
