# 01. Modelos Estritamente Pagos (Sem Camada Gratuita)

- **Contexto Geral e Regras de Billing:** Consulte [`./INDEX.md`](./INDEX.md) e [`./billing_mechanics.yaml`](./billing_mechanics.yaml).
- **Dataset de Referência:** [`./always_paid_models_from_user_list.yaml`](./always_paid_models_from_user_list.yaml)

## Diretriz de Roteamento para IA
Os modelos listados no YAML vinculado **não possuem cota gratuita** em nenhuma circunstância. Se o ambiente estiver configurado apenas com chaves *Free Tier*, **aborte a chamada** para evitar erro de permissão/cota (`HTTP 403/429`).

## Comportamento Operacional por Classe
1. **Agentes de Pesquisa Profunda (`deep-research-preview-04-2026` e `deep-research-max-preview-04-2026`):**
   - Operam via **Interactions API** (execução assíncrona de longa duração).
   - Gargalo crítico de concorrência: **1 RPM** (apenas 1 tarefa iniciada por minuto), mas até **500K TPM** e **1.44K RPD**.
2. **Agente de Automação de Interface (`gemini-2.5-computer-use-preview-10-2025`):**
   - Alto consumo de tokens visuais (capturas de tela contínuas do desktop/browser).
   - Limite de **150 RPM** e **2M TPM**, permitindo loops rápidos de ação-observação no Tier 1.