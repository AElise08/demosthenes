# Demosthenes — Master the art of oratory

**Demosthenes** was one of the greatest Greek orators of ancient times. Overcoming a speech impediment, he mastered rhetoric through relentless practice and feedback. Today, this app honors his legacy by helping **you** improve your oratory skills through AI-powered analysis.

### What Demosthenes Does

Demosthenes is an AI coaching application that analyzes your speeches, presentations, and practice recordings—in any language—and provides structured feedback on:

- **Content & Structure**: Clarity, persuasion, logical flow
- **Vocabulary**: Suggestions for stronger word choices
- **Rhetoric**: Identified figures of speech and language patterns
- **Delivery** (audio only): Pacing, pause usage, intonation, and emotional tone

Simply submit your text or record your voice. Get instant, actionable feedback to refine your speaking skills.

### Flexible AI Provider Support

Choose the AI provider that works best for you. Switch providers anytime through the settings panel (⚙️) or environment variables:

| Provider | Type | Audio | Cost |
|---|---|---|---|
| **Google Gemini**  | Cloud | ✅ | Free tier generous |
| **OpenAI** | Cloud | ❌ (text only) | Paid |
| **OpenAI-compatible** (Groq, Together, Mistral, OpenRouter) | Cloud | ❌ (text only) | Varies |
| **Ollama** | **Local** | ✅ (multimodal models) | Free, offline |
| **Custom** (LM Studio, vLLM, llama.cpp server) | Local | ❌ (text only) | Free |

### Customize Your Version with AI

The repository includes a **`seed.md`** file that serves as a complete specification for this application. Any AI agent (GitHub Copilot, Claude, or other coding assistants) can use it to:

- Understand the architecture and design principles
- Make targeted changes or add features
- Debug provider issues
- Extend functionality while preserving core behavior

**To customize your Demosthenes instance:**

1. Share `seed.md` with your AI agent or load it in your IDE.
2. Describe what you want to change (e.g., "add support for Anthropic Claude", "change the prompt language", "add export functionality").
3. The agent will follow the specification to make safe, focused edits.

The seed.md ensures consistency and prevents accidental breaking changes, even across multiple AI-assisted iterations.

### Getting Started

**Requirements:** Node.js 18+.

```bash
npm install
cp .env.example .env.local
# Optional: edit .env.local (or configure everything in the app UI)
npm run dev
```

Runs at `http://localhost:3000`.

#### Local with Ollama (100% offline, no API key needed)

```bash
# Install Ollama: https://ollama.com
ollama serve                          # in one terminal
ollama pull llama3.2                  # or another model
# In app: ⚙️ → select "Ollama (local)" → model: llama3.2
```

#### Cloud with OpenAI / Groq / Together

```bash
# .env.local
VITE_OPENAI_API_KEY=sk-...
VITE_OPENAI_MODEL=llama-3.1-70b-versatile   # Groq example
VITE_OPENAI_BASE_URL=https://api.groq.com/openai/v1
```

Or configure in the app UI (⚙️ → "OpenAI / Groq / Together").

#### Local with LM Studio

```bash
# In LM Studio, enable local server (default port 1234)
# In app: ⚙️ → "Custom (LM Studio, vLLM...)" → base URL: http://localhost:1234/v1
```

### Deploy to Vercel

1. Import the repository to Vercel.
2. In **Environment Variables**, add `GEMINI_API_KEY` (or `OPENAI_API_KEY`, etc.).
3. Deploy. The `vercel.json` already configures SPA rewrites.

> ⚠️ Local providers (Ollama, LM Studio) do not work on Vercel because it is cloud-based. Use them locally or in development.

### Scripts

- `npm run dev` — dev server (port 3000)
- `npm run build` — production build to `dist/`
- `npm run preview` — serve build locally
- `npm run lint` — type-check with `tsc --noEmit`

  <img width="2940" height="1654" alt="image" src="https://github.com/user-attachments/assets/418273ed-4445-4183-bb85-fbef05ce6a15" />
<img width="2940" height="1654" alt="image" src="https://github.com/user-attachments/assets/07c3fd55-da58-4213-b5ae-a601d4ade45f" />
<img width="2938" height="1650" alt="image" src="https://github.com/user-attachments/assets/338a9d2d-8ebe-425e-8a9e-d9838f364802" />



---

## Português

**Demóstenes** foi um dos maiores oradores gregos da antiguidade. Superando um problema de fala, dominou a retórica através de prática rigorosa e feedback constante. Hoje, este app honra seu legado ajudando **você** a melhorar suas habilidades de oratória através de análise assistida por IA.

### O que Demosthenes faz

Demosthenes é um aplicativo de coaching por IA que analisa seus discursos, apresentações e gravações de prática—em qualquer idioma—e fornece feedback estruturado sobre:

- **Conteúdo & Estrutura**: Clareza, persuasão, fluxo lógico
- **Vocabulário**: Sugestões para escolhas de palavras mais fortes
- **Retórica**: Figuras de linguagem e padrões de linguagem identificados
- **Entrega** (áudio apenas): Ritmo, uso de pausas, entonação e tom emocional

Simplesmente envie seu texto ou grave sua voz. Receba feedback instantâneo e acionável para refinar suas habilidades de fala.

### Suporte Flexível a Providers de IA

Escolha o provedor de IA que funciona melhor para você. Alterne entre provedores a qualquer momento pelo painel de configurações (⚙️) ou por variáveis de ambiente:

| Provedor | Tipo | Áudio | Custo |
|---|---|---|---|
| **Google Gemini** (`gemini-2.5-flash`, `gemini-2.5-pro`) | Cloud | ✅ | Free tier generoso |
| **OpenAI** (`gpt-4o-mini`, `gpt-4o`) | Cloud | ❌ (apenas texto) | Pago |
| **Compatível OpenAI** (Groq, Together, Mistral, OpenRouter) | Cloud | ❌ (apenas texto) | Varia |
| **Ollama** (`llama3.2`, `qwen2.5`, `mistral`, etc.) | **Local** | ✅ (modelos multimodais) | Grátis, offline |
| **Custom** (LM Studio, vLLM, llama.cpp server) | Local | ❌ (apenas texto) | Grátis |

### Customize sua Versão com IA

O repositório inclui um arquivo **`seed.md`** que funciona como uma especificação completa desta aplicação. Qualquer agente de IA (GitHub Copilot, Claude, ou outros assistentes de código) pode usá-lo para:

- Entender a arquitetura e princípios de design
- Fazer mudanças direcionadas ou adicionar funcionalidades
- Debugar problemas de providers
- Estender funcionalidades preservando o comportamento central

**Para customizar sua instância de Demosthenes:**

1. Compartilhe o `seed.md` com seu agente de IA ou carregue-o na sua IDE.
2. Descreva o que você quer mudar (ex: "adicione suporte para Anthropic Claude", "mude o idioma do prompt", "adicione funcionalidade de exportação").
3. O agente seguirá a especificação para fazer edições seguras e focadas.

O seed.md garante consistência e previne mudanças que quebram funcionalidades acidentalmente, mesmo através de múltiplas iterações assistidas por IA.

### Começando

**Pré-requisitos:** Node.js 18+.

```bash
npm install
cp .env.example .env.local
# Opcional: edite .env.local (ou configure tudo pela UI do app)
npm run dev
```

Roda em `http://localhost:3000`.

#### Local com Ollama (100% offline, sem chave de API)

```bash
# Instale o Ollama: https://ollama.com
ollama serve                          # em um terminal
ollama pull llama3.2                  # ou outro modelo
# No app: ⚙️ → selecione "Ollama (local)" → modelo: llama3.2
```

#### Cloud com OpenAI / Groq / Together

```bash
# .env.local
VITE_OPENAI_API_KEY=sk-...
VITE_OPENAI_MODEL=llama-3.1-70b-versatile   # Groq, por exemplo
VITE_OPENAI_BASE_URL=https://api.groq.com/openai/v1
```

Ou configure pela UI do app (⚙️ → "OpenAI / Groq / Together").

#### Local com LM Studio

```bash
# No LM Studio, ative o servidor local (porta 1234 padrão)
# No app: ⚙️ → "Custom (LM Studio, vLLM...)" → base URL: http://localhost:1234/v1
```

### Deploy na Vercel

1. Importe o repositório na Vercel.
2. Em **Environment Variables**, adicione `GEMINI_API_KEY` (ou `VITE_OPENAI_API_KEY`, etc.).
3. Deploy. O `vercel.json` já configura o rewrite para SPA.

> ⚠️ Provedores locais (Ollama, LM Studio) **não funcionam na Vercel** porque ela é cloud. Use-os em dev ou localmente.

### Scripts

- `npm run dev` — dev server (porta 3000)
- `npm run build` — build de produção em `dist/`
- `npm run preview` — serve a build localmente
- `npm run lint` — type-check com `tsc --noEmit`
