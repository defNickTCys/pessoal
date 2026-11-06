# TASK-003 — Executar onboarding, aprovação e dry-run

## Papel no plano

Aplicar a proposta somente no ambiente real que possui CLIs, autenticação e quota.

## Pré-requisitos

- TASK-001 concluída;
- TASK-002 concluída;
- aprovação explícita do usuário para providers, modelos, billing, tiers e modo passivo.

## Objetivo e definição de pronto

- rodar `/alethe setup`;
- conferir inventory, profiles e perguntas de billing;
- aprovar somente após revisar a tabela de tiers;
- executar `doctor`;
- executar `playbook --pick` e `playbook <id>`;
- gerar um plano e executar `validate`/`dry-run`;
- confirmar que nenhum OpenCode aparece como read-only ou com modelo implícito.

## Escopo

Alterar somente a playbooks copy e `alethe-prices.json` no diretório de configuração do usuário,
conforme o onboarding determinar. Não sobrescrever arquivos existentes sem confirmação.

## Checks

```bash
python skills/alethe/scripts/orchestrate.py doctor
python skills/alethe/scripts/orchestrate.py playbook --pick
python skills/alethe/scripts/orchestrate.py models
python skills/alethe/scripts/orchestrate.py usage
python skills/alethe/scripts/orchestrate.py dry-run <plan.json>
```

## Critério de bloqueio

Parar e pedir decisão se o OpenCode não puder fixar o modelo, se a quota estiver desconhecida
para uma rota que seria usada automaticamente, ou se o dry-run resolver provider/modelo diferente
do aprovado.
