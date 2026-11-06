# 04. Modelos da Lista Base Ausentes na Tabela do Painel

- **Contexto Geral:** Consulte [`./INDEX.md`](./INDEX.md).
- **Dataset de Referência:** [`./user_listed_models_missing_from_panel_table.yaml`](./user_listed_models_missing_from_panel_table.yaml)

## Diretriz de Resolução de Identificadores para IA
Ao configurar o `opencode.json` ou rotear chamadas de API, trate os 7 identificadores ausentes na tabela visual conforme sua classificação no YAML:

1. **Ponteiros Dinâmicos (`gemini-flash-latest`, `gemini-flash-lite-latest`, `gemini-pro-latest`):**
   - **Ação:** Podem ser chamados normalmente na API. Eles consomem a cota exata do modelo estável correspondente documentado em [`./02_commercial_free_tier_models.md`](./02_commercial_free_tier_models.md).
2. **Variante de Ferramentas (`gemini-3.1-pro-preview-customtools`):**
   - **Ação:** Utilize este ID no OpenCode sempre que priorizar chamada estrita de ferramentas customizadas (bash/edit/mcp) com a família 3.1 Pro. Ele consome da mesma piscina de cota do `Gemini 3.1 Pro` (**25 RPM / 250 RPD**).
3. **Revisões Não Provisionadas no Projeto (`deep-research-max-preview-04-2026`, `gemini-3.5-flash`, `gemini-3.7-flash`):**
   - **Ação:** Não tente invocá-los diretamente no projeto `OpenCode2` sem antes validar disponibilidade via endpoint `models.list`, pois o painel provisionou apenas `Deep Research Pro Preview`, `Gemini 3.5 Flash Lite` e saltou de `3.6 Flash` para `3.8 Flash`.