# 05. Modelos Exclusivos do Painel e Integração no Harness OpenCode

- **Contexto Geral:** Consulte [`./INDEX.md`](./INDEX.md).
- **Escopo:** Modelos listados na UI do projeto `OpenCode2` que estão fora da lista base de 22 modelos, suas especialidades e como acoplá-los ao **OpenCode** como harness principal.

## 1. Matriz de Limites (Tier 1) e Especialidade Técnica

| Grupo Funcional | Modelos no Painel | RPM / TPM / RPD (Tier 1) | Especialidade e Aplicação Específica |
| :--- | :--- | :--- | :--- |
| **Agentes** | `Antigravity` | `30` / `200K` / `1K` | Agente autônomo de engenharia de software multi-etapa. |
| **Texto Alta Vazão** | `Gemini 2 Flash`<br>`Gemini 2 Flash Lite` | `2K` / `4M` / `Ilimitado`<br>`4K` / `4M` / `Ilimitado` | Sumarização massiva de logs, compactação de histórico de sessão e tarefas de background sem teto diário (RPD ilimitado) e até 100 lotes simultâneos na API Batch (`Gemini 2 Flash`). |
| **Embeddings** | `Gemini Embedding 1`<br>`Gemini Embedding 2` | `3K` / `1M` / `Ilimitado` | Indexação vetorial de repositórios locais (RAG semântico de código e documentação). |
| **Imagem (Geração/Edição)** | `Nano Banana (2.5 Flash Image)`<br>`Nano Banana Pro (3 Pro Image)`<br>`Nano Banana 2 (3.1 Flash Image)`<br>`Nano Banana 2 Lite`<br>`Gemini Nano Banana 2.1` | `500` / `500K` / `2K`<br>`20` / `100K` / `250`<br>`100` / `200K` / `1K`<br>`150` / `100K` / `1K`<br>`100` / `200K` / `1K` | Criação e edição de assets visuais de front-end, ícones, placeholders, mockups e diagramas arquiteturais. |
| **Síntese de Voz (TTS)** | `Gemini 2.5 Flash TTS`<br>`Gemini 2.5 Pro TTS`<br>`Gemini 3.1 Flash TTS`<br>`Gemini 3.8 Flash Lite TTS`<br>`Gemini 3.8 Flash TTS` | `10` / `10K` / `100`<br>*(2.5 Pro TTS: 50 RPD)* | Síntese de fala a partir de texto para geração de assets de áudio de aplicações ou leitura audível de changelogs/relatórios. |
| **Música e SFX** | `Lyria 3 Clip`<br>`Lyria 3 Pro` | `150` / `4M` / `500` | Geração de trilhas instrumentais e efeitos sonoros para jogos e interfaces interativas. |
| **Multimodal Pesado** | `Gemini Omni 1.1 Flash`<br>`Gemini Omni 1.2 Flash` | `4` / `Ilimitado` / `20`<br>`8` / `Ilimitado` / `40` | Análise holística de gravações de tela longas, vídeo + áudio + trace de código sem limite de TPM. |
| **API Live (Tempo Real)** | `Gemini 3.5 Transcribe` (10/10K/100)<br>`2.5 Flash Native Audio Dialog`<br>`Gemini 3 Flash Live`<br>`Gemini 3.5 Live Translate`<br>`Gemini 3.5 Transcribe Live`<br>`Gemini 3.8 Live`<br>`3.8 Live Extended Thinking` | `Ilimitado` RPM / `100K–1M` TPM / `Ilimitado` RPD | Streaming bidirecional via WebSockets para comandos de voz em tempo real, transcrição contínua e pair-programming falado com raciocínio estendido (`Extended Thinking`). |

---

## 2. Arquitetura de Complementos (Harness & Instalação) para o OpenCode

O core do **OpenCode** processa texto/código via chamadas REST/SSE padrão (`@ai-sdk/google`). Para utilizar modelos não-textuais ou via WebSockets dentro do OpenCode, aplique os seguintes acoplamentos:

### A. Uso Nativo Direto (`Gemini 2 Flash` e `Gemini 2 Flash Lite`)
- **Complemento:** Nenhum. Suportados nativamente pelo provider Google do OpenCode.
- **Configuração recomendada:** Definir `google/gemini-2.0-flash-lite` como `small_model` no `opencode.json`.

### B. Acoplamento de Busca Semântica (`Gemini Embedding 1` e `2`)
- **Complemento necessário:** Servidor MCP de Indexação Vetorial (ex.: *Codebase Memory / LanceDB MCP*) registrado na chave `mcp` do `opencode.json`.
- **Instalação de pacotes base:**
  ```powershell
  npm install -g @google/genai @modelcontextprotocol/sdk
  ```
### C. Acoplamento de Geração de Mídia (`Nano Banana *`, `* TTS`, `Lyria 3 *`)

* **Complemento necessário:** Wrapper MCP de Mídia Generativa que intercepta a tool call do OpenCode, requisita o payload binário na API Gemini, converte o buffer base64/PCM e grava o arquivo físico no diretório do projeto.
* **Instalação de pacotes base (Windows / WSL):**
```powershell
winget install Gyan.FFmpeg ImageMagick.ImageMagick
npm install -g @google/genai

```

### D. Acoplamento de Tempo Real (`API Live` / WebSockets)

* **Complemento necessário:** Bridge WebSocket (`BidiGenerateContent` Client). Modelos Live não funcionam no endpoint HTTP `generateContent`; exigem túnel persistente `wss://` que captura áudio local, envia ao modelo Live e despacha o texto transcrito ou código de volta para o prompt do OpenCode.
* **Instalação de pacotes base:**

```powershell
npm install -g ws @google/genai node-record-lpcm16

```
