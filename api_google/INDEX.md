# Índice de Governança e Limites de Taxa — API Google Gemini (OpenCode)

> **Propósito para Agentes de IA:** Este documento é o nó raiz (*entrypoint*) da documentação de limites de taxa e faturamento da API Gemini no projeto `OpenCode2`. **Não duplique regras gerais nos subdocumentos.** Utilize a tabela de roteamento abaixo para carregar apenas o contexto necessário para a tarefa atual.

## 1. Fonte de Dados Global (Mecânica de Faturamento)
- **Dataset estruturado:** [`./billing_mechanics.yaml`](./billing_mechanics.yaml)
- **Projeto Auditado:** `OpenCode2` (Status ativo: **Nível 1 / Tier 1** com Cloud Billing vinculado).

## 2. Regra Geral e Comum a Todos os Modelos (Gatilho Gratuito vs. Pago)
A transição entre uso gratuito (*Free Tier*) e uso cobrado (*Pay-as-you-go*) na API Gemini **não ocorre por franquia de consumo diário dentro da mesma chave**, mas sim pela **natureza do projeto Google Cloud** ao qual a `GEMINI_API_KEY` pertence:

1. **Chave em Projeto Sem Faturamento (Free Tier):**
   - Custo: **$0.00**.
   - Comportamento no limite: Ao atingir o teto de RPM (Requisições por Minuto) ou RPD (Requisições por Dia), a API retorna `HTTP 429 (Resource Exhausted)`. Nunca há cobrança automática.
   - Privacidade: Dados de entrada/saída **são utilizados** pela Google para treinamento.
2. **Chave em Projeto Nível 1 / Tier 1 (Caso do projeto `OpenCode2`):**
   - Custo: **Cobrança imediata desde a requisição #1** por milhão de tokens em qualquer modelo comercial.
   - Comportamento: Limites expandidos de produção; dados **não são utilizados** para treinamento.

## 3. Mapa de Navegação para Relatórios Especializados

| Relatório Especializado (`.md`) | Fonte de Dados Vinculada (`.yaml`) | Escopo de Decisão para a IA |
| :--- | :--- | :--- |
| [`./01_always_paid_models.md`](./01_always_paid_models.md) | [`./always_paid_models_from_user_list.yaml`](./always_paid_models_from_user_list.yaml) | Modelos da lista de auditoria que **exigem Tier 1+** e jamais rodam em projetos gratuitos. |
| [`./02_commercial_free_tier_models.md`](./02_commercial_free_tier_models.md) | [`./commercial_models_with_free_tier_option.yaml`](./commercial_models_with_free_tier_option.yaml) | Modelos comerciais que aceitam *Free Tier* (em projeto sem Billing) vs. suas cotas pagas no `OpenCode2`. |
| [`./03_gemma_open_models.md`](./03_gemma_open_models.md) | [`./gemma_open_models.yaml`](./gemma_open_models.yaml) | Regras e gargalos severos de TPM/RPM dos modelos abertos **Gemma 4** (custo zero mesmo no Tier 1). |
| [`./04_missing_and_alias_models.md`](./04_missing_and_alias_models.md) | [`./user_listed_models_missing_from_panel_table.yaml`](./user_listed_models_missing_from_panel_table.yaml) | Resolução de ponteiros `*-latest`, variantes `-customtools` e modelos não provisionados na tabela. |
| [`./05_panel_exclusive_opencode_integration.md`](./05_panel_exclusive_opencode_integration.md) | *N/A (Exclusivos da UI do Painel)* | Modelos presentes no painel (fora da lista base) e quais complementos MCP/pacotes instalar para usá-los no **OpenCode**. |