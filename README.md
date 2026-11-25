# DT Translator

A professional **English ➜ Persian (Farsi)** translator chatbot built with [Streamlit](https://streamlit.io) and a local LLM served by [Ollama](https://ollama.com).

مترجم حرفه‌ای انگلیسی به فارسی با Streamlit و مدل زبانی محلی (Ollama)، با رعایت آیین نگارش فارسی و نیم‌فاصله‌ها.

## Features

- Multi-line input (submit with the button or `Ctrl + Enter`)
- Output follows Persian orthography rules of the Academy of Persian Language and Literature (فرهنگستان): ZWNJ (نیم‌فاصله), Persian characters and digits, Persian punctuation («»، ؛ ؟)
- Tone selection: *Same as source*, *Formal (رسمی)*, *Neutral (معیار)*, *Casual (محاوره‌ای)*
- Preserves formatting: line breaks, lists, Markdown, code, URLs, and placeholders are kept intact
- Prompt-injection safe: the input is always translated, never answered or obeyed
- Rejects non-English input
- A post-processing step fixes common issues (Arabic «ي/ك», Arabic digits, extra spaces, broken ZWNJs)
- Works with any model installed in your Ollama (model picker in the sidebar)
- Right-to-left output with the Vazirmatn font, translation history, and token/time stats

## How it works

1. You type or paste English text and pick a tone in the sidebar.
2. The app builds a system prompt with translation rules, the selected tone, and Persian writing standards.
3. The text is sent to the selected Ollama model (each request is independent, so previous translations never affect a new one).
4. The answer is cleaned up (Persian characters, digits, spacing, and ZWNJ) and shown right-to-left.

## Requirements

- Python 3.13+
- [Ollama](https://ollama.com/download) installed and running
- **At least one model installed in Ollama** (see below)

## 1. Install a model (important)

This app does **not** ship with a model. It uses the models installed in **your own** Ollama, and you choose one from the **Model** list in the sidebar.

Install a model with `ollama pull`, for example:

```shell
ollama pull qwen3:8b
```

| Model | Download size | Notes |
|---|---|---|
| `qwen3:8b` | ~5 GB | **Recommended.** Good Persian quality, runs on most PCs |
| `qwen3:4b` | ~2.5 GB | Faster, for weaker hardware (lower quality) |
| `gemma4:26b` | ~18 GB | Higher quality, needs a strong GPU / lots of RAM |

Check your installed models with `ollama list`. Larger models translate better but are slower; the first translation after starting is always slower because the model is being loaded into memory.

> The author's default model (`qwen36-35b:latest`) is a custom local model and is **not** available on Ollama. You don't need it: the app automatically selects the first installed model from its preferred list (`PREFERRED_MODELS` in [app.py](app.py)), or simply the first model you have.

## 2. Setup and run

```powershell
python -m venv .venv
.\.venv\Scripts\activate          # Linux/macOS: source .venv/bin/activate
python -m pip install -r requirements.txt

streamlit run ./app.py
```

Then open <http://localhost:8501>.

### Ollama on another address

By default the app connects to `http://127.0.0.1:11434`. If Ollama runs elsewhere, set `OLLAMA_HOST` before running:

```powershell
$env:OLLAMA_HOST = "http://192.168.1.10:11434"     # Linux/macOS: export OLLAMA_HOST=...
streamlit run ./app.py
```

## Troubleshooting

| Message in the app | Solution |
|---|---|
| **Cannot connect to Ollama** | Install Ollama and make sure it is running (open the app or run `ollama serve`); check `OLLAMA_HOST` |
| **No model is installed in Ollama** | Run `ollama pull qwen3:8b`, then click **🔄 Refresh model list** or refresh the page |
| **Translation failed ...** | The model may be too large for your RAM/GPU: choose a smaller model from the sidebar |
| Translation is very slow | Use a smaller model (e.g. `qwen3:4b`); the first request is always slower |

## Project structure

| File | Description |
|---|---|
| `app.py` | The Streamlit translator app (system prompt, Persian normalizer, UI) |
| `dt_ollama.py` | Helper for calling the Ollama chat API |
| `dt_lm.py` | Shared language-model constants |
| `dt_utility.py` | General utilities (time formatting, etc.) |

## Credits

The `dt_*.py` helper modules are from Dariush Tasdighi's Streamlit learning course.
