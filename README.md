# Instagram catalog posts

Turn photos of one outfit into a single Instagram post: a catalog image of that outfit on a mannequin, and a caption for the same post.

You select up to 10 photos of the same garment, enter a suit code, and press one of two buttons. **Generate image** sends those photos in one call and saves `instaimage-<code>.png` at 1080×1350 (4:5). **Generate text** is a separate call and saves one caption, `instaimage-<code>.txt`, which can also be reused on Facebook. Both files land in `data/output/`.

```text
Photos of one outfit
        |
        +-- Generate image --> OpenAI-compatible image edit --> Instagram portrait
        |
        +-- Generate text  --> DeepSeek (or another chat model) --> caption
```

The image call keeps the garment from the photos and places it on a fashion mannequin, not a real person. The caption follows the template in `prompts/text_prompt.txt`. Shop name, address, and tagline there are the placeholders `SHOP_NAME`, `SHOP_ADDRESS`, and `SHOP_TAGLINE`. Replace them with your own shop. Notes you type in the app are added for that run only.

## What is not in this repository

API keys stay in `.env`. Outfit photos stay in `data/images/`. Generated posts stay in `data/output/`. None of those are committed. The Python package is `cloth_store`.

Tests use stand-in API clients, so `pytest` does not need a key or a photo.

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

Put your keys in `.env`. The same fields are in the sidebar, so you can switch servers without editing code.

```bash
API_KEY=sk-...
API_BASE_URL=https://api.openai.com/v1
IMAGE_MODEL=gpt-image-1-mini

TEXT_API_KEY=sk-...
TEXT_API_BASE_URL=https://api.deepseek.com
TEXT_MODEL=deepseek-flash
```

Other image models you can pick in the sidebar: `gpt-image-1`, `gpt-image-1.5`, `gpt-image-2`, `chatgpt-image-latest`. The stronger models follow the garment photos more closely and cost more.

## Run

```bash
streamlit run app.py
```

Put JPG, PNG, or WEBP photos in `data/images/`, or type another folder path in the sidebar.

## Tests

```bash
pip install -r requirements-dev.txt
pytest
```

## Layout

```text
app.py                         Streamlit entry point
prompts/                       image prompt and caption template
src/cloth_store/
  config.py                    paths and API settings
  services/                    client, prompts, image call, text call
  ui/                          sidebar, gallery, results
data/images/                   photos you add locally
data/output/                   generated portrait and caption
tests/                         pytest, no live API key
```
