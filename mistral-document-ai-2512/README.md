# Mistral Document AI 2512 on Microsoft Foundry

This demo showcases how to use **`mistral-document-ai-2512`** deployed on **Microsoft Foundry** (Azure AI Foundry) for enterprise-grade document understanding — OCR, table extraction, structured JSON extraction, and image annotation.

<img src="mistral.png">

---

## What is Mistral Document AI 2512?

`mistral-document-ai-2512` combines two powerful models:

- **mistral-ocr-2512** (OCR 3) — state-of-the-art OCR with 99%+ accuracy across 25+ languages
- **mistral-small-2506** — intelligent document understanding for structured extraction

### Key Capabilities

| Feature | Details |
|---|---|
| Text extraction | Preserves document structure: headers, tables, lists, multi-column layouts |
| Table output | HTML or Markdown format |
| Header/Footer extraction | Per-page header and footer text |
| Image bounding boxes | Extracted with annotations and base64 data |
| Structured JSON extraction | Custom schemas via Pydantic / JSON Schema |
| Supported file types | PDF, PNG, JPEG, AVIF, PPTX, DOCX, and more |
| Limits (Azure Foundry) | Up to 30 MB / 30 pages per request |

### Azure Foundry Deployment Benefits

- **Regional data residency** — documents processed inside your Azure region
- **Enterprise-grade security** — network isolation and access controls
- **Unified billing** — through your Azure subscription
- **Content safety** — filters applied on annotation outputs

> Reference blog post: https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/unlocking-document-understanding-with-mistral-document-ai-in-microsoft-foundry/4495664  
> Model page on Azure AI: https://ai.azure.com/explore/models/mistral-document-ai-2512/version/1/registry/azureml-mistral

---

## Notebook: `mistral_document_ai_2512_azure_foundry.ipynb`

The notebook walks through 13 progressively advanced use cases:

| Section | Description |
|---|---|
| 1. Setup & Configuration | Load environment variables, configure the endpoint URL, acquire Entra ID token |
| 2. Helper functions | Utilities for encoding documents, calling the API, rendering OCR results |
| 3. Basic OCR — PDF from URL | Extract text from a publicly accessible PDF (arXiv paper) |
| 4. OCR with Table Formatting (HTML) | Extract tables as HTML and load them into Pandas DataFrames |
| 5. OCR with Header & Footer Extraction | Retrieve per-page headers and footers from a multi-page PDF |
| 6. OCR on a Local PDF | Encode a local PDF as base64 and process it; display extracted images and tables |
| 7. OCR on an Image | Process PNG/JPEG images (URL and local file) |
| 8. Page Selection | Process only specific pages (0-indexed) from a multi-page document |
| 9. BBox Annotations — Structured Image Analysis | Extract image bounding boxes with custom schema annotations |
| 10. Document Annotations — Structured Data Extraction | Extract invoice data (line items, totals, vendor info) as structured JSON |
| 11. Research Paper Extraction | Extract paper metadata (title, authors, abstract, keywords) as structured JSON |
| 12. Combined BBox + Document Annotations | Run both annotation types in a single API call |
| 13. Saved results | List all exported JSON/Markdown files |

---

## Folder Contents

```
mistral-document-ai-2512/
├── mistral_document_ai_2512_azure_foundry.ipynb   # Main demo notebook
├── azure.env                                       # Environment variables (fill in your endpoint)
├── requirements.txt                                # Python package list
├── mistral.png                                     # Mistral logo image
├── documents/
│   ├── article.pdf                                 # Sample multi-page PDF for local OCR demo
│   ├── invoice.pdf                                 # Sample invoice for structured extraction demo
│   └── scan.jpg                                    # Sample scanned image for image OCR demo
└── results/                                        # Output folder (created at runtime)
```

---

## Prerequisites

### Azure

- An **Azure subscription**
- Access to **Microsoft Foundry** (Azure AI Foundry)
- The **`mistral-document-ai-2512`** model deployed (as a Foundry resource or serverless endpoint)
- Your user/service principal must have the **`Cognitive Services User`** role on the resource

### Local environment

- Python **3.10+** recommended
- Jupyter Notebook or JupyterLab (or VS Code with the Jupyter extension)

---

## Configuration

Edit `azure.env` and set your endpoint URL:

```env
AZURE_MISTRAL_DOC_AI_ENDPOINT = "https://<your-resource>.services.ai.azure.com"
```

The notebook supports two endpoint types:

| Endpoint type | URL pattern | Auth |
|---|---|---|
| Foundry resource (recommended) | `*.services.ai.azure.com` or `*.cognitiveservices.azure.com` | Entra ID Bearer token |
| Serverless endpoint | `*.inference.ai.azure.com` | Entra ID Bearer token |

Authentication uses **`DefaultAzureCredential`** — no API key required. Make sure you are logged in with `az login` (or have a managed identity / service principal configured).

---

## Getting Started

1. **Clone the repository**

   ```bash
   git clone https://github.com/retkowsky/Azure-AIGEN-demos.git
   cd Azure-AIGEN-demos/mistral-document-ai-2512
   ```

2. **Fill in `azure.env`** with your Foundry endpoint URL (see Configuration above).

3. **Open the notebook**

   ```bash
   jupyter notebook mistral_document_ai_2512_azure_foundry.ipynb
   ```

   Or open it in VS Code.

4. **Run all cells** — the notebook will:
   - Authenticate with Entra ID using `DefaultAzureCredential`
   - Process PDFs and images from URLs and local files
   - Save extracted JSON and Markdown results to the `results/` folder

---

## Output Files

All results are saved to the `results/` directory (created automatically):

| File | Content |
|---|---|
| `ocr1.json` / `content1.md` | Basic OCR — arXiv PDF |
| `ocr3.json` / `content3.md` | OCR with header/footer extraction |
| `ocr4.json` / `content4.md` | Local PDF OCR with images and tables |
| `ocr5.json` / `content5.md` | Image OCR (URL) |
| `ocr6.json` / `content6.md` | Image OCR (local scan) |
| `ocr7.json` / `content7.md` | Page-selection OCR |
| `invoice.json` | Structured invoice extraction (annotation) |
| `annotation10.json` | Research paper metadata extraction |

---

## Author

| Field | Details |
| --- | --- |
| Name | Serge Retkowsky |
| Email | serge.retkowsky@microsoft.com |
| LinkedIn | https://www.linkedin.com/in/serger/ |
| Medium publications | https://medium.com/@sergems18/ |
