# Multimodal Embedding for Political Communication

Analyzing Korean politicians' YouTube Shorts with Gemini Embedding 2.

**Live site**: https://kyusik-yang.github.io/multimodal-embedding/

## Authors

- Kyusik Yang (New York University)
- Yongjai Yu (UC Riverside)

## What is this?

A Quarto book site that documents our research pipeline for embedding YouTube Shorts (text + audio + video) into a shared vector space using Google's Gemini Embedding 2 model. It serves as both a working research notebook and a tutorial for multimodal embedding in social science.

## Site structure

| Chapter | Contents |
|---------|----------|
| [1. Setup](https://kyusik-yang.github.io/multimodal-embedding/01-setup.html) | Google AI Studio API config, SDK setup, connection test |
| [2. Embedding](https://kyusik-yang.github.io/multimodal-embedding/02-embedding.html) | Audio, video, and multimodal embedding walkthrough |
| [3. Analysis](https://kyusik-yang.github.io/multimodal-embedding/03-analysis.html) | Cosine similarity, UMAP visualization, HDBSCAN clustering |
| [4. Panel Design](https://kyusik-yang.github.io/multimodal-embedding/04-panel.html) | Politician x month aggregation, regression model, scale-up plan |
| [References](https://kyusik-yang.github.io/multimodal-embedding/references.html) | Technical references with links |

## How to contribute

Each page is a `.qmd` file (Quarto markdown). Edit, commit, and push; GitHub Actions will automatically rebuild and deploy the site.

### Local preview

```bash
git clone https://github.com/kyusik-yang/multimodal-embedding.git
cd multimodal-embedding
quarto preview
```

Requires [Quarto](https://quarto.org/docs/get-started/) (v1.4+).

### Without local preview

You can also edit `.qmd` files directly on GitHub. The site will rebuild automatically after each push.
