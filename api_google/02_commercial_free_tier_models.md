# 02. Modelos Comerciais com Opção Free Tier vs. Produção Tier 1

- **Contexto Geral e Regras de Billing:** Consulte [`./INDEX.md`](./INDEX.md) e [`./billing_mechanics.yaml`](./billing_mechanics.yaml).
- **Dataset de Referência:** [`./commercial_models_with_free_tier_option.yaml`](./commercial_models_with_free_tier_option.yaml)

## Diretriz de Roteamento para IA
Para usufruir destes modelos **sem custo**, o harness precisa alternar a variável de ambiente para uma chave de API de um projeto **sem Cloud Billing**. Caso utilize a chave do projeto `OpenCode2`, serão aplicados os limites de Nível 1 do arquivo YAML sob cobrança imediata.

## Regras de Escalonamento de Limites (Tier 1 vs. Free)
- **Linha Lite (`2.5-flash-lite`, `3.1-flash-lite`, `3.5-flash-lite`):**
  - Maior vazão do catálogo: **4.000 RPM** e **4M TPM** no Tier 1 (com RPD ilimitado no 2.5 Lite e 150K RPD nos demais).
  - Ideal para *sub-agents* de varredura rápida, indexação e tarefas paralelas no OpenCode.
- **Linha Flash Padrão (`2.5-flash`, `3-flash`, `3.6-flash`, `3.8-flash`):**
  - Equilíbrio padrão para codificação: **1.000 RPM**, **1M a 2M TPM** e **10.000 RPD**.
- **Linha Pro e Fronteira (`2.5-pro`, `3.1-pro-preview`, `4-argon`):**
  - `gemini-3.1-pro-preview` possui trava severa de volume no Tier 1: **apenas 25 RPM e 250 RPD**. Reservar exclusivamente para planejamento arquitetural complexo.
  - `gemini-4-argon` entrega capacidade de ponta com **1.000 RPM**, **2M TPM** e **RPD Ilimitado** no Tier 1.
- **Limites de Ferramentas Nativas (Grounding):**
  - Fundamentação do Google Maps: **25.000 RPD** em toda a linha listada no YAML.
  - Pesquisa Google (Search Grounding): **5.000 RPD** na geração 2.5 e **1.500 RPD** na geração 3.0 ou superior.