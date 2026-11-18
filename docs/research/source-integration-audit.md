# Upstream Source Audit — ANA

Audit date: 2026-11-18. This is a source-selection record, not a claim that code has already been imported or tested in ANA.

## Integration rule

Do not combine whole applications or copy code indiscriminately. Reuse a specific module only after:
1. inspecting its current source and dependencies;
2. confirming the exact license for that code and any bundled assets/models;
3. isolating it behind an ANA interface;
4. adding tests and a rollback point;
5. recording attribution and notices.

A repository's top-level license does not automatically cover third-party models, SDKs, fonts, voice samples, or media.

## Reviewed sources

| Source | Verified strengths from its README/docs | ANA use | Decision |
|---|---|---|---|
| [Komorebi](https://github.com/kiskaserver/interactive_assistent) | Tauri 2/Rust/React desktop companion; Live2D; local-first speech and LLM with OpenRouter fallback; screen/region capture; proactive agent; affinity/relationship state; desktop actions | Candidate source for Live2D event handling, voice pipeline, proactive/affinity ideas, screen-awareness architecture | Inspect exact version and component licenses before copying. Not selected as ANA's base because ANA repository is currently empty and we need a clean, Persian-first architecture. |
| [Warashi](https://github.com/inni918/warashi) | Built on Open-LLM-VTuber; Live2D; voice chat; long-term per-character memory; proactive topics; sleep mode; Ollama/cloud model configuration; Windows/macOS | Character memory, per-character data separation, proactive chat patterns | Its README identifies MIT core plus bundled terms and attribution/NOTICE. Review NOTICE and bundled assets before reuse. |
| [Open-LLM-VTuber](https://github.com/Open-LLM-VTuber/Open-LLM-VTuber) | Live2D, hands-free voice, voice interruption, vision, emotion mapping, proactive speaking, multiple LLM/ASR/TTS backends; Windows/macOS/Linux | Voice turn-taking, interruption, expression event patterns, adapter concepts | MIT code; Live2D sample models have separate license. README says long-term memory was temporarily removed in the inspected version. |
| [AILIS](https://github.com/haowenGuo/AILIS) | Electron desktop companion; VRM avatar; voice, expressions, lip sync, long-term memory, screen/window/region and file context with permission; agent runtime and approval/audit flows | VRM renderer contract, media pipeline, permission/audit patterns, future vision context | MIT. Reuse only focused components; do not import its whole agent runtime into Phase 1. |
| [OpenMeido](https://github.com/lshhhhhhh/OpenMeido) | Live2D; cross-session/layered memory; relationship progression; personality lore; multiple TTS engines; proactive observer | Character progression and memory UX ideas | GPL-3.0. Keep as design reference unless ANA intentionally adopts GPL-compatible distribution. Its bundled Live2D assets and Cubism Core have separate terms. |
| [Elfie Assistant](https://github.com/izumiishikawa/elfie-assistant) | Persistent memory; voice; dynamic skills; desktop overlay; optional Live2D; web/mobile clients; tool integrations | Memory lifecycle, voice pipeline and plugin boundaries | Repository LICENSE says MIT, but vendored projects/assets need separate review. Optional real-time speech service is not assumed free. |
| [Monaw](https://github.com/kailiang0120/monaw) | Windows 10/11; ChatGPT account sign-in, OpenAI API and Gemini API; global/workspace memory; MCP; browser/computer use; scheduled tasks; explicit permission profiles | Provider/account connection research, memory UX, Windows permissions for later phases | Apache-2.0. README states voice input needs OpenAI or Google API key even if chat uses a ChatGPT account. Do not assume ChatGPT Free account sign-in grants API/voice access. |
| [Microsoft UFO](https://github.com/microsoft/UFO) | Windows desktop automation, GUI + API actions, task orchestration and agent coordination | Future Windows-control research only | MIT. Not part of the first conversational brain; computer-control permissions and evaluation are required first. |
| [Ari-VoiceCommand](https://github.com/DO0OG/Ari-VoiceCommand) | The supplied repository URL could not be retrieved during this audit | None until accessible | Unverified. Do not copy or claim capabilities. |
| [HeyGen Creative Studio](https://app.heygen.com/apps/creative-studio/video) | Hosted video creation product, not a source repository or documented local avatar runtime in the supplied link | Optional later asset/video production, not the live conversational brain | Not a code dependency. Account, plan and service terms apply. |

## What "use all" means safely

ANA can learn from all accessible sources, but cannot legally or technically merge 100% of every project:
- Some projects have incompatible frameworks and duplicated runtimes.
- OpenMeido is GPL-3.0; MIT/Apache projects have different redistribution obligations.
- Live2D runtime, sample models, VRM models, voices and fonts can have separate licenses.
- UFO and Monaw focus on agent/computer workflows, not the first character conversation loop.
- HeyGen is a hosted creative tool, not an open-source component.

Therefore, Phase 1 uses a clean provider-neutral conversation core and records source-specific modules for later, isolated integration.

## Provider/account caution

Monaw's README explicitly documents ChatGPT account sign-in for chat and separately says voice input requires an OpenAI or Google API key. This is evidence of Monaw's own integration, not a general entitlement or permission to reuse private ChatGPT web sessions. ANA will only implement documented, supported sign-in/API flows and will not scrape browser cookies, automate private web UI sessions, or reverse-engineer account endpoints.

## Next source inspections before code import

- Komorebi: exact current branch, license files, Live2D dependency, voice interfaces and proactive loop.
- Warashi: NOTICE, dependency license manifest, memory implementation and voice interruption.
- Open-LLM-VTuber: voice pipeline, ASR/TTS adapter contracts, Live2D model licensing.
- AILIS: VRM renderer, voice/media docs, permission and memory interfaces.
- Elfie: optional Live2D overlay and memory APIs.
- Monaw: ChatGPT auth implementation and API-key requirement boundaries; no credentials are to be copied into ANA.
