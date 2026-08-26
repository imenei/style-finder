# 👗 Style Finder — AI Fashion Analyzer

> Snap a photo of an outfit and get an instant style analysis: matched items, similar products, prices, and purchase links. Style Finder is a multimodal RAG app that combines a vision model, similarity search, and a vision-language LLM to turn a single image into a full fashion report.
> 
![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![Gradio](https://img.shields.io/badge/UI-Gradio-orange?logo=gradio&logoColor=white)
![PyTorch](https://img.shields.io/badge/Vision-PyTorch%20%2F%20ResNet50-EE4C2C?logo=pytorch&logoColor=white)
![watsonx.ai](https://img.shields.io/badge/LLM-IBM%20watsonx.ai%20%7C%20Llama%20Vision-054ADA)
![License](https://img.shields.io/badge/license-Apache%202.0-green)

---

##  What it does

**Style Finder** is a multimodal Retrieval-Augmented Generation (RAG) app that turns any fashion photo into a full style report:

1.  **Upload a photo** of an outfit
2.  **ResNet50** converts it into a numerical feature vector (embedding)
3.  **Cosine similarity search** finds the closest match in a fashion dataset
4.  **Llama Vision (via IBM watsonx.ai)** generates a professional, catalog-style analysis — colors, materials, styling  plus real item names, prices & purchase links

No manual tagging. No external API required beyond the LLM. Just image in, style report out.

---

##  How it works

```
┌─────────────┐     ┌──────────────────┐     ┌───────────────────┐     ┌─────────────────┐
│   Upload    │ ──▶ │  Image Encoding   │ ──▶ │  Similarity Match  │ ──▶ │  LLM Analysis    │
│   Image     │     │  (ResNet50)       │     │  (Cosine Sim.)     │     │  (Llama Vision)  │
└─────────────┘     └──────────────────┘     └───────────────────┘     └─────────────────┘
                                                                                  │
                                                                                  ▼
                                                                        ┌─────────────────┐
                                                                        │  Formatted       │
                                                                        │  Markdown Report │
                                                                        └─────────────────┘
```

| Component | Role |
|---|---|
| `models/image_processor.py` | Encodes images to base64 + ResNet50 feature vectors; finds closest dataset match via cosine similarity |
| `models/llm_service.py` | Interfaces with the Llama Vision Instruct model on IBM watsonx.ai to generate the fashion analysis |
| `utils/helpers.py` | Formats responses, retrieves related items, handles model refusals gracefully |
| `app.py` | Gradio interface tying the full pipeline together |

---

##  Getting Started

### Prerequisites
- Python 3.11+
- An IBM watsonx.ai project (for the Llama Vision model)
- A precomputed fashion dataset with image embeddings (`.pkl`, not included — see below)

### Installation

```bash
git clone https://github.com/imenei/style-finder.git
cd style-finder
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### Configuration
Set your watsonx.ai project details in `config.py` (or via environment variables):

```python
PROJECT_ID = "your-project-id"
REGION = "us-south"
LLAMA_MODEL_ID = "meta-llama/llama-4-..."
```

### Run

```bash
python app.py
```

Then open the local URL shown in the terminal (default: `http://127.0.0.1:5000`).

>  **Dataset note:** This repo does not include the fashion embeddings dataset (`swift-style-embeddings.pkl`) due to size. Generate your own using `image_processor.py`'s `encode_image` method on your product catalog, or plug in your own precomputed embeddings with an `Embedding`, `Image URL`, `Item Name`, `Price`, and `Link` column.

---

##  Preview

| Upload | Analysis Output |
|---|---|
| Any outfit photo | Style breakdown + matched/similar items with prices & links |

![Fashion Analysis](assets/demo-analysis.png)
*Automatic outfit analysis with similar item suggestions*

---

##  Tech Stack

- **Vision Model:** ResNet50 (torchvision, pre-trained on ImageNet)
- **Similarity Search:** scikit-learn cosine similarity
- **LLM:** Llama Vision Instruct via IBM watsonx.ai
- **UI:** Gradio Blocks
- **Data:** Pandas

---

##  License

This project is built on lab material licensed under **Apache 2.0**.

---

##  About

Built as part of a hands-on multimodal RAG learning project  combining computer vision embeddings with LLM-powered reasoning to bridge visual and textual fashion understanding.
