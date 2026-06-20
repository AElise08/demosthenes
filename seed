# Demosthenes Seed Specification

Status: Draft v1 (application-specific)

Purpose: Define a practical implementation guide for AI agents that need to change, extend, or debug the Demosthenes application.

## Normative Language

The key words `MUST`, `MUST NOT`, `REQUIRED`, `SHOULD`, `SHOULD NOT`, `RECOMMENDED`, `MAY`, and `OPTIONAL` in this document are to be interpreted as described in RFC 2119.

`Implementation-defined` means the behavior belongs to the implementation contract, but this specification does not prescribe a universal policy. Implementations MUST document the selected behavior.

## 1. Problem Statement

Demosthenes is a React + Vite application that analyzes speeches, presentations, and practice recordings using AI providers.

The application exists to help a user:

- submit text or audio for analysis;
- receive structured feedback about content, pacing, rhetoric, and delivery;
- compare and revisit previous analyses;
- practice speech delivery through guided modes;
- switch providers without changing the product workflow.

An AI agent working on this repository MUST preserve the product focus: speech analysis first, provider flexibility second, and visual clarity always.

## 2. Goals and Non-Goals

### 2.1 Goals

- Preserve the current speech-analysis workflow from input to results.
- Keep provider selection configurable through the existing UI and service layer.
- Maintain support for text analysis, audio analysis, and practice modes.
- Keep local history stable and safe.
- Prefer small, targeted edits over broad rewrites.
- Keep the application buildable and type-safe.

### 2.2 Non-Goals

- Replacing the app with a different product category.
- Introducing unnecessary state management or infrastructure.
- Removing support for existing providers without an explicit request.
- Overfitting the implementation to a single provider or a single deployment mode.
- Rewriting the UI language away from the current dark, focused presentation unless asked.

## 3. System Overview

### 3.1 Main Components

1. `App Shell`
	- Owns the top-level page flow.
	- Coordinates home, practice, history, results, and settings states.

2. `Input Section`
	- Accepts text and audio input for analysis.
	- Starts the analysis workflow.

3. `Results View`
	- Presents AI-generated feedback and derived metrics.

4. `History Layer`
	- Stores prior analyses locally.
	- Lists, selects, deletes, and clears saved items.

5. `Practice Modes`
	- Provide guided speech training flows.

6. `Provider Service Layer`
	- Resolves active provider configuration.
	- Calls the configured AI backend.

7. `Audio Recorder Hook`
	- Encapsulates microphone capture and audio handling.

8. `Settings Modal`
	- Lets the user switch provider and adjust runtime settings.

9. `Shared Types`
	- Define app state, history items, provider configuration, and analysis results.

### 3.2 Abstraction Levels

The repository is easiest to modify when kept in these layers:

1. `Presentation Layer`
	- Components under `components/`.
	- Pure UI, local interaction, and rendering logic.

2. `Domain Layer`
	- Shared application types in `types.ts`.
	- Data contracts used across the app.

3. `Service Layer`
	- Files under `services/`.
	- Provider resolution, API calls, and history persistence.

4. `Interaction Layer`
	- Hooks under `hooks/`.
	- Audio capture and other reusable behavior.

### 3.3 External Dependencies

- React 19.
- Vite.
- TypeScript.
- Tailwind CSS v4 through PostCSS.
- Lucide icons.
- Recharts for data visualization.
- AI providers reachable through the existing service abstraction.

## 4. Core Domain Model

### 4.1 Entities

#### 4.1.1 Analysis Result

Normalized output returned by the AI analysis pipeline.

The exact shape is defined in `types.ts`, but implementations MUST preserve the contract expected by `ResultsView` and history persistence.

#### 4.1.2 History Item

One saved analysis in local storage.

Fields are logically:

- `id` (string)
- `timestamp` (number)
- `result` (analysis result object)
- `preview` (string)

#### 4.1.3 Provider Configuration

Resolved runtime provider selection.

Fields are logically:

- provider id
- model
- base URL, where relevant
- API key or equivalent secret reference
- feature flags such as audio support, where relevant

#### 4.1.4 App State

Top-level UI and workflow state.

Examples:

- idle
- analyzing
- results
- error

### 4.2 Stable Identifiers and Normalization Rules

- History item IDs MUST remain string values suitable for local persistence.
- Provider IDs SHOULD remain stable across sessions.
- Preview text SHOULD be short and human-readable.
- Audio MIME type values SHOULD be preserved when available.
- Any normalized provider selection SHOULD remain compatible with the settings UI.

## 5. Repository Contract

### 5.1 Source of Truth

The repository’s current contract is expressed by the code in:

- `App.tsx`
- `components/`
- `services/`
- `hooks/`
- `types.ts`
- `README.md`

If this specification conflicts with working code that is already established and validated, the implementation SHOULD prefer the smallest safe change needed to restore the intended contract.

### 5.2 File-Responsibility Contract

- `App.tsx` MUST remain the top-level orchestration point unless a deliberate refactor requires otherwise.
- `services/` SHOULD own provider and persistence logic.
- `components/` SHOULD remain primarily presentational.
- `hooks/` SHOULD encapsulate reusable interactive behavior.

### 5.3 Architecture Boundary

This project is a client-side application.

It is not a scheduler, daemon, issue tracker, or worker orchestration system. Agents MUST NOT introduce backend-style assumptions unless the repository explicitly adds them later.

## 6. Configuration Specification

### 6.1 Configuration Sources

Configuration MAY come from:

1. environment variables;
2. the settings UI;
3. provider-specific defaults;
4. persisted browser state, where implemented.

Environment values MUST NOT silently override explicit user selections unless the repository already defines that behavior.

### 6.2 Provider Configuration

The application currently supports multiple providers, including cloud and local options.

Rules:

- Provider resolution MUST respect the selected provider in the UI.
- Gemini MUST NOT be treated as the only supported provider.
- Local providers SHOULD remain usable without cloud credentials when the provider supports that mode.
- A provider change SHOULD be reflected in the UI without forcing unrelated resets.

### 6.3 Audio Configuration

Audio support depends on the selected provider and runtime capabilities.

Rules:

- Audio capture SHOULD fail clearly when the browser or environment does not support it.
- Audio-specific behavior MUST remain compatible with text-only providers by falling back gracefully or restricting UI affordances.
- The app SHOULD preserve the chosen MIME type or encoding metadata when needed by the provider layer.

## 7. UI, Design, and Interaction Contract

This section describes the current product experience and the design rules agents MUST follow when changing it. It intentionally separates the implemented contract from optional future enhancements.

### 7.1 Product Experience Principles

- The app SHOULD feel like a focused speech-coaching workspace, not a marketing page.
- The primary job of every screen is to move the user from speech input to useful feedback with minimal friction.
- The interface MUST keep Portuguese (Brasil) microcopy for user-facing labels, helper text, errors, and actions.
- Provider flexibility SHOULD be visible but secondary; provider controls must not compete with the speech workflow.
- Visual changes SHOULD preserve the current dark, monochrome, high-contrast direction unless a request explicitly changes the brand direction.
- New UI SHOULD be practical, compact, and scannable. Avoid decorative complexity that does not improve analysis, practice, or review.

### 7.2 Current User Flow Map

The implemented app has five user-facing flows:

1. `Home`: headline, provider badge, text/audio input, professor mode, and analysis CTA.
2. `Results`: radar summary, metric cards, feedback, strengths, improvements, and optional rich analysis sections.
3. `History`: right-side drawer listing saved analyses, selection, deletion, and clear-all.
4. `Practice`: mode selector, timed practice, and section practice.
5. `Settings`: provider selection, base URL, API key, model selection, and provider-specific helper text.

The current top-level flow is:

```text
Home
  -> submit text/audio
  -> analyzing state
  -> Results
  -> Analisar Outro Discurso
  -> Home

Header: Home <-> Practice
Header: History drawer overlays current screen
Header/Provider badge: Settings modal overlays current screen
```

Rules:

- `App.tsx` MUST remain the source of truth for top-level view transitions unless a deliberate routing refactor is requested.
- Entering practice SHOULD not mutate stored history or provider settings.
- Opening history or settings MUST behave as an overlay and preserve the underlying screen state.
- Returning to Home from the header currently resets `appState` to `IDLE`; changes to this behavior MUST be intentional and documented.

### 7.3 Navigation and Layout

#### 7.3.1 Header

The header provides persistent orientation and primary navigation.

- Brand block: logo, `Demosthenes`, and `Treinador de Oratória`.
- Desktop navigation: `Início`, `Prática`, `Histórico`, settings icon.
- Mobile currently exposes settings only; adding mobile access to Home, Practice, and History is RECOMMENDED if the navigation is redesigned.
- Header styling SHOULD remain sticky, compact, dark, and separated by a subtle border.
- Icon buttons MUST have accessible labels or titles.

#### 7.3.2 Page Rhythm

- Main content SHOULD be horizontally constrained with `container mx-auto px-4`.
- Primary content should appear within the first viewport on desktop.
- Avoid nested cards. The app may use cards for the input console, results panels, metric cards, practice mode cards, history items, and modals.
- Rounded corners SHOULD stay within the existing range (`rounded-lg`, `rounded-xl`, `rounded-2xl`) unless a component already establishes another pattern.
- Large hero-scale type belongs only on the Home introduction. Tool panels, drawers, and modals need smaller, denser headings.

### 7.4 Home and Input Flow

#### 7.4.1 Home Layout

The Home screen is composed of:

- short headline focused on persuasive speaking;
- supporting text explaining text/audio analysis;
- active provider badge with model name and a `trocar` action;
- input console;
- three small benefit blurbs below the console.

Rules:

- The input console MUST remain the visual center of the Home flow.
- The provider badge SHOULD remain visible before analysis so the user knows what backend will run.
- Benefit blurbs are secondary and SHOULD NOT look like primary action cards.
- Background decoration MUST stay subtle and must not reduce text contrast.

#### 7.4.2 Input Console

Current capabilities:

- Textarea for speech text.
- Microphone recording using `MediaRecorder`.
- Audio preview state after capture.
- Recording timer.
- Section markers while recording: `Introdução`, `Desenvolvimento`, `Conclusão`.
- Professor mode toggle.
- Word count for text input.
- Submit button labeled `Analisar Discurso`.

Rules:

- Text and audio are mutually exclusive in the current UI: starting audio clears text, and having captured audio disables new recording until the audio is discarded.
- Submit MUST be disabled until text or audio exists.
- While analyzing, submit MUST show an in-progress state (`Analisando...`) and prevent duplicate submission.
- Microphone errors MUST be shown inline inside the input console and use Portuguese copy.
- The recording overlay MUST clearly distinguish recording, captured, and idle states.
- Marker actions MUST only be active while recording.
- Professor mode SHOULD be a small toggle-level control; it must not become a separate mode that hides the main input.

Recommended copy:

- Empty placeholder: `Digite o texto do seu discurso aqui, ou clique no microfone para gravar sua voz...`
- Recording status: `Gravando sua voz...`
- Captured state: `Áudio Capturado`
- Discard action: `Descartar gravação`
- Submit action: `Analisar Discurso`

### 7.5 Results Flow

#### 7.5.1 Results Layout

Current layout:

- top summary grid with a radar chart and overall score;
- metric cards for `Clareza`, `Persuasão`, `Estrutura`, `Vocabulário`, and `Tom`;
- optional `Análise Vocal Detalhada` for audio results;
- main feedback panel;
- rhetorical device tags;
- `Pontos Fortes`;
- `Áreas para Melhoria`;
- optional vocabulary suggestions;
- optional section analysis;
- reset CTA: `Analisar Outro Discurso`.

Rules:

- Results MUST prioritize scanability: score first, metrics second, prose third, details after that.
- `overallScore` and the five core metrics MUST remain on a 0-100 scale unless the shared type contract changes.
- Optional sections MUST render only when the data exists and is non-empty.
- The reset CTA MUST return the user to a clean Home input state.
- Results SHOULD NOT introduce sharing/export controls unless the feature is actually implemented end to end.
- Avoid celebratory or vague feedback copy in static UI. The AI feedback should carry the coaching detail.

#### 7.5.2 Metric Cards

Each metric card MUST:

- show a Portuguese label;
- display a numeric value;
- include a short explanation;
- use an icon from Lucide when an appropriate icon exists;
- stay compact enough for a dense dashboard grid;
- avoid relying on color alone to communicate quality.

The existing `MetricCard` component SHOULD be reused for new metrics when possible.

### 7.6 History Flow

Current behavior:

- History opens as a right-side drawer over the current screen.
- Items are listed newest first through `historyService`.
- Each item shows timestamp, preview, score, and a hover-revealed delete button.
- Selecting an item closes the drawer and shows the saved result.
- `Limpar Histórico` clears all local history.

Rules:

- History MUST remain local-only unless explicit cloud sync is requested.
- Deleting a history item MUST NOT open the item underneath.
- Clear-all SHOULD remain visually lower priority than individual item selection, but destructive styling must be clear.
- Empty state copy SHOULD remain short: `Nenhuma análise salva ainda.`
- If a confirmation flow is added for destructive actions, it MUST be keyboard accessible and not block non-destructive history browsing.

Future enhancement, not current contract:

- provider badges, input type labels, export, search, or filters MAY be added, but only with corresponding data support in `HistoryItem` or a backward-compatible migration.

### 7.7 Practice Flow

#### 7.7.1 Practice Selector

Current modes:

- `Desafio Cronometrado`: records a timed spoken response to a random topic.
- `Prática de Seção`: accepts pasted text for focused section feedback.

Rules:

- Practice cards SHOULD remain equal weight on desktop and stacked on mobile.
- Cards are action controls, so the entire card MAY be clickable, but focus and hover states must be clear.
- Practice mode naming SHOULD stay concise and action-oriented.

#### 7.7.2 Timed Practice

Current flow:

1. User enters `Desafio Cronometrado`.
2. App selects a random topic.
3. User chooses duration from `30`, `60`, or `120` seconds.
4. User starts recording.
5. Timer counts down and auto-stops at zero.
6. User can retry or submit audio for the normal analysis pipeline.

Rules:

- The topic must be visually prominent but not editable in the current implementation.
- `Novo Tópico` MUST be disabled while recording.
- Duration controls MUST be disabled while recording or after a recording is finished.
- The record button MUST be visually distinct from the stop button.
- After recording, the user MUST see both `Tentar Novamente` and `Analisar Discurso`.
- Timed practice analysis currently returns to the regular `ResultsView`; agents MUST NOT assume a separate practice result schema exists.

#### 7.7.3 Section Practice

Current flow:

1. User enters `Prática de Seção`.
2. User pastes a section of a speech.
3. Submit button `Analisar Seção` becomes enabled when text exists.
4. Text is sent through the normal analysis pipeline.

Rules:

- The section practice textarea SHOULD remain simple until file upload, templates, or automatic splitting are explicitly requested.
- Do not specify PDF/DOCX upload, template libraries, or history reuse as current features.
- Section practice analysis currently returns to the regular `ResultsView`; any section-specific result UI must still respect `AnalysisResult`.

### 7.8 Settings Flow

Current capabilities:

- Provider selection grid.
- Conditional `Base URL`.
- Conditional `API Key`.
- Model input.
- Ollama installed-model suggestions when available.
- Provider-specific helper copy.
- `Salvar` persists selected configuration.

Rules:

- Settings MUST open as a modal overlay and close without losing unsaved underlying screen state.
- Provider changes in the modal SHOULD update dependent defaults for model, base URL, and API key.
- Saving MUST provide visible feedback (`Salvo`).
- Secret inputs MUST use password fields.
- Provider labels may use English term `Provider` to match code, but new user-facing copy SHOULD prefer Portuguese where natural.
- Settings must not trigger analysis by itself.

### 7.9 Visual Design System

#### 7.9.1 Color

The current visual system is intentionally restrained:

- page background: black / zinc-950;
- surfaces: zinc-900, zinc-900/50, zinc-950;
- borders: zinc-800, zinc-900;
- primary action: white surface with black text;
- muted text: zinc-400, zinc-500, zinc-600;
- destructive/error: red accents;
- success/save: green accent;
- optional semantic accents already used in details: blue, purple, yellow, green.

Rules:

- Do not turn the app into a one-hue blue/purple gradient interface.
- Use white-on-black as the primary brand signal.
- Use accent colors sparingly for state, semantics, or optional analysis categories.
- Important information MUST NOT be communicated by color alone.

#### 7.9.2 Typography

- Headline: large, bold, tight tracking only on Home.
- Section headings: bold, compact, scannable.
- Body copy: zinc-muted, comfortable line-height.
- Numeric scores and timers SHOULD use mono/tabular styling when appropriate.
- Do not scale font sizes with viewport width.
- Avoid negative letter spacing except where already established for the Home headline.

#### 7.9.3 Controls

- Use Lucide icons in buttons where a matching icon exists.
- Icon-only buttons MUST include `title` or `aria-label`.
- Primary actions use white background and black text.
- Secondary actions use zinc surfaces and borders.
- Destructive actions use red text/accent and should not look like primary CTAs.
- Disabled states MUST visibly reduce contrast and prevent pointer ambiguity.

#### 7.9.4 Motion

- Existing fade/slide animations MAY be used for page entry and overlays.
- Recording can use pulse animation, but avoid excessive motion.
- Loading states SHOULD use one spinner or animated icon, not multiple competing animations.
- New animation SHOULD respect `prefers-reduced-motion` when feasible.

#### 7.9.5 Responsive Behavior

- Main grids SHOULD collapse to one column on mobile.
- Modals and drawers MUST fit within the viewport and allow internal scrolling.
- Touch targets SHOULD be at least 40px high where practical.
- Long labels must wrap or truncate cleanly; text MUST NOT overflow buttons or cards.
- Mobile navigation is currently limited; if expanded, it should use a compact menu or bottom-safe pattern without hiding settings.

### 7.10 Feedback, Errors, and Empty States

- Errors MUST be actionable and written in Portuguese.
- Provider errors SHOULD point to settings, credentials, network, model name, or unsupported audio when relevant.
- Empty states SHOULD be short and specific.
- Long-running analysis MUST keep the user on the current flow and show a disabled action state.
- Retry paths SHOULD preserve recoverable input where possible.
- Browser microphone permission denial MUST explain that microphone access is required for recording.

### 7.11 Accessibility Requirements

- Interactive elements MUST be reachable by keyboard.
- Buttons should be real `button` elements unless navigation semantics require links.
- Modals and drawers SHOULD manage focus predictably when improved.
- Text contrast MUST remain readable against dark surfaces.
- Form fields MUST have visible labels or clear accessible names.
- Audio recording controls MUST expose state through text, not only icon shape or color.

### 7.12 Current vs Future Features

Agents MUST NOT describe these as current behavior unless they are implemented:

- audio waveform playback/scrubbing;
- speech-to-text transcript editing;
- PDF/DOCX upload;
- practice drafts;
- cloud history sync;
- result export/share;
- separate practice result schema;
- provider health checks;
- mobile full navigation menu.

These features MAY be added later, but each requires code, type, persistence, and UX updates that match the rest of this specification.

## 8. Provider and Analysis Rules

### 8.1 Provider Selection

- The provider chosen in settings MUST be the one used for the next analysis request.
- Configuration changes SHOULD not require a full page refresh unless the implementation explicitly documents that limitation.

### 8.2 Analysis Execution

- The analysis path MUST continue to call the central service abstraction.
- Text input and audio input SHOULD share a consistent result contract.
- Failures from remote providers MUST surface as user-visible errors.

### 8.3 Error Handling

- Provider failures SHOULD not silently produce empty results.
- Unexpected runtime errors SHOULD be caught and presented in a controlled way.
- The application SHOULD preserve the current state long enough for the user to retry or recover.

### 8.4 AI Analysis Pipeline

The AI flow is intentionally narrow and MUST remain easy to reason about.

1. The user enters text or records audio in the UI.
2. The app collects the input and builds an `AnalyzeInput` payload.
3. The service layer resolves the active provider from persisted settings and runtime environment.
4. The selected provider receives the input through `analyze(input)`.
5. The provider returns a normalized `AnalysisResult` object.
6. The UI renders the result and stores it in history when appropriate.

Rules:

- The UI SHOULD NOT talk to provider SDKs directly when the service layer already owns that path.
- Text and audio inputs SHOULD converge into the same result contract.
- Providers that do not support audio MUST either reject audio clearly or force a text-only path, depending on their documented behavior.
- The application SHOULD treat `AnalysisResult` as the stable output contract for the results screen and history.
- Fields such as `overallScore`, `metrics`, `feedback`, `strengths`, `improvements`, `rhetoricalDevices`, `audioAnalysis`, `vocabularySuggestions`, and `sectionAnalysis` SHOULD remain compatible with existing consumers.

### 8.5 Response Shape Expectations

The analysis output is not an unstructured blob.

Implementations SHOULD preserve a response format that is useful to the UI and to the user:

- `overallScore` SHOULD summarize the whole analysis.
- `metrics` SHOULD break the evaluation into focused dimensions such as clarity, persuasion, structure, vocabulary, and tone.
- `feedback` SHOULD contain a concise overall explanation.
- `strengths` and `improvements` SHOULD remain human-readable lists.
- `rhetoricalDevices` SHOULD capture language patterns worth calling out.
- `audioAnalysis`, when present, SHOULD describe pacing, pauses, and intonation.
- `vocabularySuggestions`, when present, SHOULD give actionable replacements and rationale.
- `sectionAnalysis`, when present, SHOULD support segmented or professor-style review flows.

Any provider-specific prompt engineering, schema adaptation, or post-processing MUST still end in this shared shape unless the repository explicitly adds a new consumer contract.

### 8.6 Supported Models and Provider Specifics

The application has been tested and validated with the following model selections.

Agents working on provider or model support MUST consult this matrix before making assumptions about what works.

#### Gemini (Google)

Provider ID: `gemini`

Audio support: **YES**. Native multimodal support for audio input.

Requirements:
- Valid `GEMINI_API_KEY` environment variable.

Limitations:
- Audio input requires the model to support multimodal input (both flash and pro do).
- Prompt language is Portuguese (Brasil).

#### OpenAI / Groq / Together / OpenRouter

Provider ID: `openai` (mapped to OpenAI-compatible endpoint)

Audio support: **NO** (not standard in OpenAI API spec; audio input will be rejected)

Requirements:
- Valid API key for the selected provider.
- Correct `baseUrl` pointing to the provider's v1 endpoint.
- Model name matching the provider's model list.

Limitations:
- Text-only input via chat.completions.
- If audio is submitted, the app MUST reject it and fall back to requesting text.
- Prompt language is Portuguese (Brasil).

#### Ollama (Local)

Provider ID: `ollama`

Base URL default: `http://localhost:11434`

Supported models:
- Text-only: `llama3.2`, `qwen2.5`, `mistral`, `neural-chat`, or any instruct-tuned model.
- Multimodal (audio + text): `llava`, `llama3.2-vision`, `gemma3`, or vision-capable models.

Audio support: **YES** (with multimodal models only)

Requirements:
- Ollama running and reachable at the configured base URL.
- Model pre-pulled via `ollama pull <model>`.
- For audio: multimodal-capable model must be used.

Limitations:
- Audio input with text-only models will not fail gracefully; use multimodal models for audio.
- Prompt language is Portuguese (Brasil).
- Local processing means latency depends on hardware and model size.

#### Custom / LM Studio / vLLM / llama.cpp server

Provider ID: `custom` (OpenAI-compatible)

Base URL default: `http://localhost:1234/v1` (LM Studio)

Supported models:
- Any model available in the local server that supports chat.completions API.
- Examples: `local-model`, `gguf-model-name`, or whatever the server exposes.

Audio support: **NO** (not supported by standard OpenAI-compatible chat.completions)

Requirements:
- Local server running and exposing `/v1/chat/completions` endpoint.
- Model loaded and ready.
- If authentication is required, provide valid API key.

Limitations:
- Text-only.
- Prompt language is Portuguese (Brasil).
- Latency depends on local hardware and model size.

## 9. State Management and Persistence

### 9.1 State Ownership

Top-level state SHOULD remain in the app shell unless there is a strong reason to move it.

Rules:

- UI flow state MUST remain predictable.
- Result state MUST remain associated with the correct analysis request.
- History state SHOULD be refreshed after writes.

### 9.2 Persistence Rules

- History SHOULD persist across reloads when the implementation already supports it.
- Data migrations SHOULD be defensive and backward-compatible where possible.
- Persistent storage SHOULD fail softly rather than corrupting the application state.

### 9.3 Local Storage Contract (Browser-based)

The application persists user history locally using browser `localStorage` instead of a backend database.

**Storage location:**
- Key: `demosthenes_history`
- Format: JSON-serialized array of `HistoryItem` objects.
- Scope: Per browser, per domain, per user profile (no server sync).

**Data limits:**
- Maximum 50 items kept at any time (older items are dropped automatically on add).
- Each item contains the full `AnalysisResult`, so total size depends on analysis depth.
- Browser localStorage quota is typically 5-10 MB; exceeding it SHOULD fail silently per historyService implementation.

**HistoryItem structure:**
```
{
  id: string (timestamp-based or UUID)
  timestamp: number (milliseconds since epoch)
  result: AnalysisResult (full analysis object)
  preview: string (short text or "Audio Analysis")
}
```

**Contract for changes:**

- `getHistory()` MUST return an empty array if localStorage is unavailable or corrupted, not throw.
- `saveHistoryItem(item)` MUST add new item to the front, drop items beyond 50, and fail softly if quota is exceeded.
- `deleteHistoryItem(id)` MUST remove one item by ID without affecting others.
- `clearHistory()` MUST remove the entire key from localStorage without throwing.

**Rules for agents:**

- MUST NOT bypass the historyService API to directly manipulate localStorage.
- MUST NOT increase the 50-item limit without explicit requirement and performance assessment.
- MUST NOT store sensitive data (API keys, passwords) in history items. If a provider secret appears in analysis results accidentally, strip it before saving.
- MUST handle cases where localStorage is disabled (private browsing, strict security policies).
- MUST ensure any changes to `AnalysisResult` shape are backward-compatible or provide a safe migration path.
- MUST NOT assume history data survives browser clear-all or private browsing session end.

**Data migration strategy:**

If the `AnalysisResult` or `HistoryItem` shape changes:

1. Keep the historyService getHistory() handler as the only point of deserialization.
2. Inside getHistory(), parse stored items and normalize any missing/old fields to sensible defaults.
3. Log a warning if normalization happens, so developers know data shape changed.
4. On next save, the new shape will be persisted naturally.

Example:
```typescript
export const getHistory = (): HistoryItem[] => {
  try {
    const stored = localStorage.getItem(HISTORY_KEY);
    if (!stored) return [];
    let items = JSON.parse(stored);
    // Normalize: add missing 'preview' field if it doesn't exist
    items = items.map(item => ({
      ...item,
      preview: item.preview || (item.result?.feedback?.slice(0, 50) || 'Analysis')
    }));
    return items;
  } catch (error) {
    console.error('Failed to load history:', error);
    return [];
  }
};
```

**Privacy and user expectations:**

- The user SHOULD be able to clear history through the UI and have it truly deleted from the browser.
- The user SHOULD be able to export or access their analysis history if needed (implementation-defined).
- The user SHOULD know that history is local-only and does not sync across devices.
- Agents SHOULD NOT add cloud sync without explicit request, as it changes the trust model significantly.

## 10. Implementation Guidelines

### 10.1 Change Strategy

An AI agent working in this repository SHOULD:

1. start from the nearest owning file;
2. identify the smallest safe edit;
3. preserve public behavior unless the request asks otherwise;
4. keep types and runtime behavior aligned;
5. validate the result before expanding scope.

### 10.2 Code Quality Rules

- Prefer explicit types over implicit assumptions.
- Prefer service-layer changes for business logic.
- Prefer component changes for rendering-only adjustments.
- Avoid introducing duplicate source-of-truth state.
- Avoid unnecessary abstractions.
- Avoid broad renames unless they improve correctness or maintainability materially.

### 10.3 UI Quality Rules

- Preserve the current dark, focused visual style unless the task explicitly changes the design direction.
- Maintain readable contrast and clear hierarchy.
- Keep the interface practical and polished, not generic or over-animated.
- Mobile layout SHOULD continue to work when desktop behavior is changed.

### 10.4 Design Implementation Constraints

**When implementing UI changes, adhere to these principles:**

- **Color Palette Fidelity**: Use only colors defined in section 7.9.1. Do not introduce new colors without updating that section.
- **Typography Scale**: Use only the sizes and weights defined in section 7.9.2. Custom sizes should be exceptional and documented.
- **Spacing Consistency**: All padding, margins, and gaps MUST be multiples of 4px (base unit).
- **Component Reusability**: Common patterns (buttons, cards, alerts) MUST use the same component or styles across the app.
- **No Custom Styling Without Documentation**: If a component requires non-standard styling, add a comment explaining why and when.
- **Responsive First**: Test all UI changes at 320px (mobile), 640px (tablet), and 1024px+ (desktop) widths.
- **Keyboard Navigation**: Any interactive element MUST be reachable via Tab key; focus order MUST be logical.
- **Color Not Alone**: Never convey important information using color alone (e.g., error state needs both red color AND an icon or text).
- **Sufficient Contrast**: All text MUST meet WCAG AA (4.5:1) contrast ratio. Verify with a contrast checker.
- **Feedback for Every Action**: Every user action (click, keystroke, selection change) MUST produce visible or audible feedback (state change, loading spinner, toast, etc.).

### 10.5 Component Development Checklist

Before marking a component as "done", verify:

- [ ] Component renders correctly at mobile, tablet, and desktop sizes.
- [ ] All interactive elements have clear focus indicators (blue outline).
- [ ] Hover states work on desktop; touch states work on mobile.
- [ ] Color scheme uses palette from section 7.9.1; contrast ≥ 4.5:1.
- [ ] Spacing uses 4px base unit; no arbitrary margins/padding.
- [ ] Typography uses scale from section 7.9.2.
- [ ] Component includes aria-label or semantic HTML if needed (forms, buttons, modals).
- [ ] Component works in context: renders alongside other components without layout break.
- [ ] Loading, error, and success states are visually distinct.
- [ ] No animations fight the `prefers-reduced-motion` setting.
- [ ] Microcopy is in Portuguese (Brasil) and follows patterns in section 7.10.

## 11. Validation and Definition of Done

### 11.1 Required Validation

For meaningful changes, the agent MUST at minimum consider:

- `npm run lint`
- `npm run build`

### 11.2 Completion Criteria

A change is complete only when:

- the targeted behavior is implemented at the correct layer;
- related types still line up;
- the main app flow still works;
- obvious regressions are not introduced;
- the repository still builds successfully.

### 11.3 Extra Checks

If the edit touches provider behavior, audio, or history, the agent SHOULD also inspect the adjacent service and component paths to ensure the contract remains consistent.

### 11.4 Local Testing Checklist

After meaningful edits, verify locally:

- `npm run dev` starts without errors.
- Provider settings UI loads and allows switching between providers.
- Text input → analysis → results works for at least one provider.
- History list appears and persists after page reload.
- An error state is catchable and recoverable (e.g., bad API key).
- Lint and build pass: `npm run lint && npm run build`.

If the change involves audio:
- Audio recording works in the browser (check console for Web Audio API errors).
- Audio → analysis works for providers that support it (Gemini, Ollama with multimodal).
- Attempting audio with OpenAI properly rejects and prompts text fallback.

### 11.5 Input/Output Compatibility Matrix

When adding or changing provider behavior, refer to this matrix:

| Input Type | Gemini | OpenAI-compat | Ollama | Custom |
|---|---|---|---|---|
| Text | ✓ | ✓ | ✓ | ✓ |
| Audio | ✓ | ✗ (reject) | ✓ (multimodal only) | ✗ (reject) |
| Sections/markers | ✓ (in prompt) | ✓ (in prompt) | ✓ (in prompt) | ✓ (in prompt) |

Output is always `AnalysisResult` (same shape for all providers).

Error cases that MUST be handled:
- Invalid/missing API key → clear error message.
- Provider unreachable → suggestion to check config or network.
- Model mismatch → error naming the expected model.
- Audio with unsupported provider → reject with fallback suggestion.

## 12. Visual and UX Validation

### 12.1 Design Review Checklist

Before considering a UI-focused change complete, perform this review:

- [ ] **Layout**: Does the view maintain visual hierarchy at all breakpoints?
- [ ] **Spacing**: Are all margins and paddings multiples of 4px? Is there sufficient breathing room?
- [ ] **Colors**: Are the colors from the palette (section 7.9.1)? Do they meet contrast requirements?
- [ ] **Typography**: Does text use the scale from section 7.9.2? Is hierarchy clear (size + weight)?
- [ ] **Components**: Do shared patterns (buttons, cards, alerts) use the same styles throughout?
- [ ] **Animations**: Are animations purposeful (not distracting)? Do they respect prefers-reduced-motion?
- [ ] **States**: Are idle, loading, success, and error states visually distinct?
- [ ] **Focus**: Can all interactive elements be accessed via keyboard? Focus indicators visible?
- [ ] **Feedback**: Does every action produce immediate, visible feedback?
- [ ] **Microcopy**: Are labels, buttons, and messages clear, concise, and in Portuguese (Brasil)?
- [ ] **Mobile**: Does the view work on small screens (320px) without horizontal scroll?
- [ ] **Dark Mode**: Does the design work with the dark background (#0f0f0f / #1a1a1a)?

### 12.2 Visual Regression Testing

When making UI changes, verify:

1. **Before**: Screenshot or visual baseline of the affected component/view.
2. **After**: Screenshot of the same component/view after your changes.
3. **Comparison**: Differences are intentional and expected.
4. **Siblings**: Check that unchanged components nearby still render correctly (no layout shift or overflow).

### 12.3 Interaction Testing Scenarios

Test these user flows with the updated component:

1. **Happy Path**: Normal, expected use (e.g., enter text → analyze → see results).
2. **Error Path**: Network failure, missing config, or invalid input → see error message → recover.
3. **Edge Case**: Boundary values (empty input, very long input, slow network, multiple rapid clicks).
4. **Accessibility**: Keyboard-only navigation, screen reader announcement (if applicable).
5. **Responsive**: Test at 320px, 640px, and 1024px viewport widths.
6. **Touch**: Tap buttons, drag if applicable, check safe area on mobile.

### 12.4 Performance Considerations

- **First Paint**: Changes should not noticeably delay initial page load.
- **Metric Cards**: Rendering 4–6 cards should be instant (<100ms).
- **History List**: Rendering 50 items should be smooth (use virtualization if needed).
- **Animations**: 60 FPS (use transform and opacity; avoid layout-triggering properties like width/height).

### 12.5 Contrast and Accessibility Verification

**Tools:**
- Use WebAIM Contrast Checker (webaim.org/resources/contrastchecker/) for color pairs.
- Use browser DevTools Lighthouse or axe DevTools for accessibility audit.
- Manual keyboard test: Tab through all interactive elements; verify focus order is logical.
- Manual screen reader test (macOS VoiceOver, Windows Narrator, or NVDA): Verify all text is announced, buttons are labeled.

**Acceptable Results:**
- Text: 4.5:1 minimum (WCAG AA).
- Large text (18px+): 3:1 minimum.
- Graphics/components: 3:1 minimum.

## 13. Common Provider Issues and Troubleshooting

### Gemini

**Issue:** `Error: invalid API key`

Diagnosis: Check `GEMINI_API_KEY` env var or settings UI.

Fix: Obtain key from Google AI Studio, set env var or paste in settings.

---

**Issue:** `Error: RESOURCE_EXHAUSTED`

Diagnosis: Free tier rate limit hit or quota exceeded.

Fix: Wait a few minutes or upgrade to paid tier.

---

### OpenAI-compatible (Groq, Together, etc.)

**Issue:** `Error: 401 Unauthorized`

Diagnosis: API key missing, invalid, or wrong for the selected provider.

Fix: Verify key matches provider. Groq ≠ OpenAI ≠ Together.

---

**Issue:** `Error: Model not found`

Diagnosis: Model name doesn't exist in the provider or is spelled wrong.

Fix: Check provider docs for available model names. Use provider-specific naming.

---

**Issue:** Audio input accepted but response is garbage

Diagnosis: OpenAI-compatible does not support audio via standard chat.completions.

Fix: Reject audio input early. Switch to Gemini or Ollama (multimodal) for audio.

---

### Ollama

**Issue:** `Error: Ollama not running`

Diagnosis: App cannot reach `http://localhost:11434` (or custom URL).

Fix: Start Ollama: `ollama serve` in another terminal.

---

**Issue:** `Error: Model not found`

Diagnosis: Model is not pulled or name is wrong.

Fix: Pull the model: `ollama pull llama3.2` (or chosen model).

---

**Issue:** Audio analyzed but result makes no sense

Diagnosis: Text-only model used for audio; audio is not understood.

Fix: Switch to multimodal model: `ollama pull llava` or `llama3.2-vision`.

---

### Custom (LM Studio, vLLM)

**Issue:** `Error: Cannot connect to localhost:1234`

Diagnosis: Server not running or different port.

Fix: Start server and check base URL in settings matches it.

---

**Issue:** `Error: response_format not supported`

Diagnosis: Server doesn't support `response_format: json_object`.

Fix: Update server or adjust prompt to request JSON without strict mode.

---

## 14. Example: Adding Support for a New Provider

If an agent is asked to add a new provider (e.g., Anthropic Claude):

1. Create `services/providers/claudeProvider.ts` following the `LLMProvider` interface.
2. Add provider ID to `ProviderId` type in `services/providers/types.ts`.
3. Add constructor case in `services/providers/factory.ts`.
4. Add entry to `listProviders()` export.
5. Update `seed.md` section 8.6 with model matrix.
6. Test locally with `npm run dev`.
7. Verify settings UI shows new provider option.
8. Run `npm run lint && npm run build`.

## 15. What to Avoid

- Do not rewrite the app when a small fix is sufficient.
- Do not move logic into UI components when a service or hook is a better fit.
- Do not remove support for existing providers or practice flows without explicit direction.
- Do not break existing history data unless you also provide a safe migration path.
- Do not leave error states vague when they can be made actionable.

## 16. Recommended Working Order for AI Agents

1. Read `README.md` to confirm product intent and supported providers.
2. Inspect `App.tsx` to find the top-level flow.
3. Inspect the nearest owning component or service.
4. Make the smallest change that fixes the issue or adds the feature.
5. Validate with lint and build.
6. Update adjacent types or UI only if the change requires it.

## 17. Final Rule

If in doubt, prefer the behavior that:

- preserves existing user trust;
- keeps the app focused on speech analysis;
- changes the fewest files;
- keeps providers interchangeable;
- remains easy for the next agent to understand.
