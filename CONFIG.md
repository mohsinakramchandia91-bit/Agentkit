# ⚙️ AgentKit CONFIG Reference

Every workflow has a **⚙️ CONFIG** node (Set). Edit ONLY this node — no hunting
through the workflow. Field reference below.

## AI fields (all workflows)

| Field | Ollama (free/local) | OpenAI |
|-------|---------------------|--------|
| `ai_base_url` | `http://localhost:11434/v1` | `https://api.openai.com/v1` |
| `ai_api_key` | `ollama` (anything works) | your `sk-...` key |
| `ai_model` | `llama3.1` (or any pulled model) | `gpt-4o-mini` |

Ollama setup: install from ollama.com, then `ollama pull llama3.1`.
n8n must reach it: if n8n runs in Docker, use `http://host.docker.internal:11434/v1`.

## 01 — Telegram AI Assistant

| Field | Meaning | Default |
|-------|---------|---------|
| `system_prompt` | Bot personality & rules | helpful assistant prompt |
| `sheet_id` | Google Sheet ID | `YOUR_GOOGLE_SHEET_ID` |
| `memory_tab` | Tab holding chat memory | `memory` (columns: `chat_id \| role \| content \| ts`) |
| `log_tab` | Tab for run logs | `log` |
| `max_history` | Messages remembered per chat | `10` |

## 02 — Content Repurposer

| Field | Meaning | Default |
|-------|---------|---------|
| `sheet_id` / `log_tab` | Log of generated packs | `log` |
| `default_tone` | Writing tone for all 4 outputs | `bold, friendly, no fluff` |

Trigger: send `/repurpose <topic>` to your bot.

## 03 — Lead Qualifier

| Field | Meaning | Default |
|-------|---------|---------|
| `telegram_chat_id` | Where hot-lead alerts go | `YOUR_TELEGRAM_CHAT_ID` |
| `sheet_id` / `log_tab` | Leads table | `leads` |
| `score_threshold` | Minimum score = "hot" | `7` |

Webhook: `POST /webhook/agentkit-lead` with JSON
`{name, email, business, budget, message}`.
Get your Telegram chat ID: message @userinfobot on Telegram.

## 04 — Daily Researcher

| Field | Meaning | Default |
|-------|---------|---------|
| `rss_url` | News feed to digest | `https://hnrss.org/frontpage` |
| `max_items` | Stories per digest | `5` |
| `digest_language` | `English` or `Roman Urdu` | `English` |
| `telegram_chat_id` / `sheet_id` / `log_tab` | Delivery + log | `digests` |

Schedule node 🕗 controls the time (default 08:00 daily).

## 05 — Support Bot

| Field | Meaning | Default |
|-------|---------|---------|
| `sheet_id` | Sheet holding FAQ | `YOUR_GOOGLE_SHEET_ID` |
| `kb_tab` | FAQ tab | `faq` (columns: `question \| answer`) |
| `log_tab` | Q&A log tab | `support_log` |
| `fallback_text` | Reply when answer unknown | includes `YOUR_NUMBER` — replace! |

The AI is instructed to answer ONLY from the FAQ. Anything else → fallback text.
