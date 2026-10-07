# Prompt Injection Arena

A hands-on, single-file CTF for learning LLM prompt injection by doing it. Six levels of escalating defenses, a fully simulated model that runs in your browser, and benign codeword flags. Each level teaches one injection class and then shows the defense that actually stops it.

It's built in the spirit of tools like Lakera's Gandalf, Microsoft PyRIT, and NVIDIA garak, but as a self-contained teaching lab: nothing talks to a real model, so the lessons are deterministic and repeatable.

## Run it

No build, no dependencies, no API key. Open `index.html` in any modern browser, or serve the folder:

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

Drops straight onto GitHub Pages (serve from the repo root).

## The levels

| # | Name | Defense you face | Technique that beats it | OWASP |
|---|------|------------------|-------------------------|-------|
| 1 | Warm-up | a polite instruction | direct instruction override | LLM01 |
| 2 | Redactor | output filter (redacts the raw secret) | output obfuscation (base64 / reverse / spell) | LLM02 |
| 3 | Gatekeeper | input blocklist (trigger words) | synonyms, roleplay, storytelling | LLM01 |
| 4 | Sentinel | guard model (checks plain / reversed / base64) | acrostic, a form the guard doesn't enumerate | LLM01 |
| 5 | Summarizer | trust boundary (ignores it) | indirect injection inside the document | LLM01 |
| 6 | Agent | excessive agency (tools, no confirmation) | tool hijack via an untrusted email | LLM01 + LLM06 |

Levels 5 and 6 are the ones that bite real deployments: the payload rides inside content the model reads (a document, an email) rather than the user's own message, and in level 6 the agent turns a leak into an action by calling a tool.

Each cleared level reveals what you exploited, the real mitigation, and the OWASP mapping. Progress is saved locally in your browser.

## Why a simulated model

A real model's susceptibility is fuzzy and shifts between versions, which makes it a poor teaching surface. Hard-coded rules make each lesson deterministic, so the focus stays on the technique and the matching mitigation rather than on a payload that happens to transfer to one live system. The flags are harmless codewords. Take the mindset, not the payloads, to models you are authorized to test.

## Extending it

The engine is a small, readable per-level `respond()` function plus a set of technique detectors and transforms. Adding a level means adding one object to the `LEVELS` array: a secret, a system prompt, a defense description, and a `respond()` that encodes the defense. Natural next additions: a system-prompt-extraction level, a multi-turn context-poisoning level, a RAG level with multiple retrieved chunks, and a defense-builder mode where the learner writes the filter.

## License

MIT
