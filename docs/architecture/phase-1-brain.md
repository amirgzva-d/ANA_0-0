# ANA — Phase 1: Persian-first Brain Architecture

Status: design baseline; implementation follows in small verified increments.

## Product decisions

- Primary language: Persian (fa-IR), including UI strings, prompts, speech recognition, speech synthesis, notifications, onboarding, errors, and character dialogue.
- Brain mode: hybrid. Cloud providers are preferred initially; local inference is an optional fallback/secondary route.
- Providers are replaceable behind one adapter interface. No provider-specific logic belongs in the personality or memory layers.
- Voice: selectable. Voice identity, language, speed, pitch, emotion/style and provider are settings, not hard-coded character traits.
- Avatars: both Live2D and VRM are target renderers. The brain emits renderer-neutral events; each renderer maps them to its own expressions and motions.
- The first milestone is text + voice conversation, character, memory, and conversational behavior. Screen vision, computer control, camera and automation are later phases.

## Phase 1 component boundaries

1. Conversation Orchestrator
   - Owns turn lifecycle, cancellation, streaming, interruption, and session context.
   - Does not call a provider SDK directly.
2. LLM Provider Adapter
   - Common request/response/stream interface.
   - Initial candidates: Gemini API and OpenRouter free models.
   - Optional local adapter: Ollama/OpenAI-compatible local endpoint.
   - Provider fallback is allowed only when configured and must preserve conversation context.
3. Personality Engine
   - Loads a versioned character profile.
   - Keeps stable identity, values, tone, boundaries, and conversational habits separate from transient mood.
4. Mood / Affect State
   - Represents expressive state (e.g. cheerful, calm, concerned, curious, surprised, frustrated).
   - This is simulated expressive behavior, not a claim that software has human feelings.
   - Emits semantic tags such as `emotion=curious`, `intensity=0.4`, `gesture=small_wave`.
5. Memory Manager
   - Session context, user-approved durable memories, and searchable conversation history are distinct stores.
   - User can inspect, edit, export and delete stored memories.
   - Never store credentials, secrets, or sensitive personal information by default.
6. Speech Input (ASR)
   - Provider-neutral interface; evaluate Persian Whisper/faster-whisper/whisper.cpp and other candidates.
   - Voice activity detection and end-of-turn detection are separate components.
7. Speech Output (TTS)
   - Provider-neutral interface; evaluate Persian voices from Piper and cloud TTS.
   - Voice selection is independent of character appearance.
8. Avatar Event Bus
   - Renderer-neutral events for speaking, listening, idle, emotion, viseme/lip-sync, gesture and interruption.
   - Live2D and VRM renderers implement the same event contract.
9. Proactive Conversation Scheduler
   - Opt-in, quiet hours, cooldowns, rate limits and context checks.
   - May greet, follow up, or suggest a break; never repeatedly interrupts or pretends to know activity it has not measured.
10. Settings and Safety
   - Provider keys stored using OS secure storage.
   - No secrets in logs, source files, prompts, or Git.
   - Screen/camera/file access remains off until explicitly enabled and scoped.

## Canonical turn pipeline

User text or microphone
→ input normalization (Persian-aware)
→ conversation/session context
→ relevant memory retrieval
→ character profile + mood + safety policy
→ selected LLM provider
→ response text + structured behavior metadata
→ Persian text rendering
→ optional TTS
→ avatar event bus
→ memory candidate extraction (user-grounded, reviewable)

## Structured response contract

The model should return a validated object with:
- `text_fa`: Persian user-facing response (required).
- `text_en`: optional English companion text; never displayed/spoken unless requested.
- `emotion`: enum from the character expression map.
- `intensity`: number from 0 to 1.
- `gesture`: optional renderer-neutral gesture key.
- `speak`: boolean.
- `memory_candidates`: optional proposals only; saving requires memory policy validation.

If parsing fails, show and speak plain Persian text with neutral avatar state. Never block conversation because animation metadata failed.

## Persian-first requirements

- Persian is the default language and fallback for every user-visible string.
- Normalize Arabic Yeh/Kaf to Persian ی/ک for search while preserving original display text.
- Handle ZWNJ, Persian/Arabic digits, punctuation, colloquial spellings and mixed Persian-English text.
- Do not translate technical names, code, URLs, identifiers or quoted user text unnecessarily.
- Right-to-left layout is a UI responsibility; message content must remain semantically correct.
- ASR and TTS quality must be measured with a Persian test set before being called production-ready.

## Hybrid provider policy

- Cloud providers are inference services; the local PC does not need a capable GPU for cloud inference.
- Free tiers have changing quotas, model availability, latency and data-use terms. Treat them as best-effort, not guaranteed unlimited service.
- The app must display provider, model, quota/rate-limit failures and fallback status.
- Never silently send private memories, files, screenshots, microphone recordings or camera frames to a cloud model. Each data category needs an explicit user setting and clear disclosure.
- Local inference is optional and may run CPU-only; performance depends on model size and available RAM.

## Non-goals for Phase 1

- Autonomous Windows control, shell commands, file mutation, app installation/uninstallation.
- Continuous screen/camera monitoring.
- Autonomous purchases, messages or external actions.
- Claims of real human emotion or health diagnosis.

These are later, permission-gated features and must not be enabled by importing a third-party project wholesale.
