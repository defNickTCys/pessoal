# 03. Família Gemma 4 (Uso Gratuito e Limitações Estritas)

- **Contexto Geral:** Consulte [`./INDEX.md`](./INDEX.md).
- **Dataset de Referência:** [`./gemma_open_models.yaml`](./gemma_open_models.yaml)

## Diretriz de Roteamento para IA
Os modelos `google/gemma-4-26b-a4b-it` e `google/gemma-4-31b-it` **não geram cobrança por token**, mesmo quando chamados dentro do projeto pago `OpenCode2` (Tier 1). Contudo, **não devem ser usados como modelo principal de sessão no OpenCode**.

## Travas Operacionais Obrigatórias
1. **Barreira de Contexto por Minuto (16K TPM):**
   - O limite total de tokens de entrada por minuto é de apenas **16.000 tokens**.
   - Como o prompt de sistema + árvore de diretórios do OpenCode frequentemente ultrapassa 10K–15K tokens, uma única requisição longa estoura o limite de TPM imediato.
2. **Cadência Baixa (30 RPM):**
   - Permite no máximo 1 chamada a cada 2 segundos, inviabilizando loops rápidos de leitura de múltiplos arquivos.
3. **Ausência de Grounding:**
   - Sem suporte às cotas de Pesquisa Google ou Google Maps do painel.