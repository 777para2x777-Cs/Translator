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
- Right-to-left output with the Vazirmatn font, translation history, and token/time stats

## Requirements

- Python 3.13+
- [Ollama](https://ollama.com) running locally (`http://127.0.0.1:11434`)
- A model pulled in Ollama. The default is `qwen36-35b:latest`; change `MODEL_NAME` in [app.py](app.py) to use another one (e.g. `qwen3:8b`, `gemma4:26b`).

## Setup and run

```powershell
python -m venv .venv
.\.venv\Scripts\activate          # Linux/macOS: source .venv/bin/activate
python -m pip install -r requirements.txt

streamlit run ./app.py
```

Then open <http://localhost:8501>.

## Project structure

| File | Description |
|---|---|
| `app.py` | The Streamlit translator app (system prompt, Persian normalizer, UI) |
| `dt_ollama.py` | Helper for calling the Ollama chat API |
| `dt_lm.py` | Shared language-model constants |
| `dt_utility.py` | General utilities (time formatting, etc.) |

## Credits

The `dt_*.py` helper modules are from Dariush Tasdighi's Streamlit learning course.
