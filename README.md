# 👗 Style Finder

Snap a photo of an outfit and get an instant style analysis: matched items, similar products, prices, and purchase links. Style Finder is a multimodal RAG app that combines a vision model, similarity search, and a vision-language LLM to turn a single image into a full fashion report.

## What it does

📸 Upload a photo of an outfit, and here's what happens behind the scenes:

1. **ResNet50** converts the image into a numerical feature vector (embedding)
2. **Cosine similarity search** finds the closest match in a fashion dataset
3. **Llama Vision**, served through IBM watsonx.ai, generates a professional, catalog-style analysis covering colors, materials, and styling — along with real item names, prices, and purchase links
4. You get back a clean, markdown-formatted report, ready to read

No manual tagging. No external API required beyond the LLM. 🪄

## Architecture

| Component | Role |
|---|---|
| `models/image_processor.py` |  Encodes images to base64 and ResNet50 feature vectors, finds the closest dataset match via cosine similarity |
| `models/llm_service.py` |  Interfaces with the Llama Vision Instruct model on IBM watsonx.ai to generate the fashion analysis |
| `utils/helpers.py` |  Formats responses, retrieves related items, handles model refusals gracefully |
| `app.py` |  Gradio interface tying the full pipeline together |

## Getting started

### Prerequisites

Python 3.11 or later, an IBM watsonx.ai project for the Llama Vision model, and a precomputed fashion dataset with image embeddings (not included in this repo — see the note below).

### Installation

```bash
git clone https://github.com/imenei/style-finder.git
cd style-finder
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### Configuration

Set your watsonx.ai project details in `config.py` or via environment variables:

```python
PROJECT_ID = "your-project-id"
REGION = "us-south"
LLAMA_MODEL_ID = "meta-llama/llama-4-..."
```

### Run

```bash
python app.py
```

Then open the local URL shown in the terminal (default `http://127.0.0.1:5000`). 

>  **Dataset note:** this repo doesn't include the fashion embeddings dataset (`swift-style-embeddings.pkl`) due to its size. You can generate your own using the `encode_image` method in `image_processor.py` on your product catalog, or plug in your own precomputed embeddings with an `Embedding`, `Image URL`, `Item Name`, `Price`, and `Link` column.

## Tech stack

ResNet50 (torchvision, pre-trained on ImageNet) for vision embeddings, scikit-learn for cosine similarity search, Llama Vision Instruct via IBM watsonx.ai for the language model, Gradio for the interface, and Pandas for data handling.

## License

This project is built on lab material licensed under Apache 2.0.

## About

Built as part of a hands-on multimodal RAG learning project combining computer vision embeddings with LLM-powered reasoning to bridge visual and textual fashion understanding. ✨
