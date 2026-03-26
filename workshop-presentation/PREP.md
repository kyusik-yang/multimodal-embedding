# NYU Methods Workshop Preparation

**Date**: 2026-04-03 (approximately)
**Presenter**: Kyusik Yang
**Topic**: Multimodal Embedding for Political Communication Research
**Format**: Methods workshop (discussion-oriented, not formal paper presentation)

---

## Presentation Structure (Draft)

### 1. What is multimodal embedding? (5 min)

**Core idea**: Map text, audio, and video into a shared vector space so that semantically similar content is close together regardless of modality.

**Key distinction from prior work**:

| Generation | Method | What it captures | Limitation |
|-----------|--------|-----------------|------------|
| 1st | Bag-of-words, TF-IDF | Word frequency | No semantics |
| 2nd | Word2Vec, GloVe | Word-level semantics | No sentence meaning |
| 3rd | BERT, SBERT | Sentence-level text semantics | Text only |
| 4th | CLIP (OpenAI) | Image-text alignment | No audio/video |
| **5th** | **Gemini Embedding 2** | **Text + audio + video in one space** | New, less validated |

**Why this matters for political science**: Politicians communicate through multiple channels simultaneously. A legislator's YouTube Short contains spoken words, visual framing (setting, gestures, graphics), and audio tone. Text-only analysis captures only one dimension.

### 2. Prior literature landscape (10 min)

#### 2a. Text embedding in political science (established)
- Rodriguez & Spirling (2022, JOP): Word embeddings for applied research
- Rodriguez, Spirling & Stewart (2023, APSR): Embedding regression (conText) [DOI: 10.1017/S0003055422001228]
- Rheault & Cochrane (2020, Political Analysis): Word embeddings for ideological scaling [DOI: 10.1017/pan.2019.26]
- Grimmer, Roberts & Stewart (2022): *Text as Data* - comprehensive framework

#### 2b. Image/visual analysis in political science (emerging)
- Torres & Cantú (2022, Political Analysis): Learning to see - convolutional approach to social science data [DOI: 10.1017/pan.2021.9]
- Zhang & Pan (2019, Sociological Methodology): CASM - deep learning for collective action events [DOI: 10.1177/0081175019860244]
- Peng (2018, Journal of Communication): Same candidates, different faces - visual framing [DOI: 10.1093/joc/jqy041]
- Won, Steinert-Threlkeld & Joo (2017, ACM MM): Protest activity detection and perceived violence [DOI: 10.1145/3123266.3123282]

#### 2c. Multimodal approaches (very new, mostly outside polisci)
- Radford et al. (2021): CLIP - contrastive image-text pre-training [arXiv: 2103.00020]
- Girdhar et al. (2023): ImageBind - six-modality embedding space
- Liang et al. (2022, NeurIPS): Mind the gap - modality gap in contrastive representation learning (important validity concern)

#### 2d. The gap
- No published paper applying true multimodal embedding (text + video + audio in shared space) to political communication
- Existing multimodal work in polisci uses image-text only (CLIP); no audio/video
- ImageBind lacks Korean speech support; CLIP is image-text only
- Gemini Embedding 2 (March 2025) is the first model enabling this research

### 3. Our research: YouTube Shorts as a case (10 min)

#### Data
- 121,900 videos from 263 Korean National Assembly legislators
- 51,197 Shorts downloaded as MP4 (~342 GB on SSD)
- Full metadata: party, gender, district, committee, terms
- ~38,000 Whisper transcripts (MLX, Korean fine-tuned, ~74% usable)
- 2,969 GPT-4o-mini content classifications (10 categories)

#### The Visual-Verbal Gap (VVG) concept
- For each Short: embed video separately, embed text separately
- Compute cosine distance in shared Gemini Embedding 2 space
- VVG = degree to which visual performance diverges from verbal content
- High VVG: saying one thing, showing another (performance politics)
- Low VVG: text and visuals aligned (standard policy briefing)

#### Why Korean legislature?
- 93% of legislators have YouTube channels
- YouTube Shorts is dominant political short-form platform (87.4% share)
- Mixed electoral system (PR vs SMD) provides within-country variation
- Strong party system with clear predictions

### 4. Methodological challenges for discussion (15 min)

#### 4a. Embedding dimension selection

**The problem**: Gemini Embedding 2 outputs 3072-dimensional vectors. Before clustering, we reduce dimensions with UMAP. But how many dimensions?

| n_components | Use case | Tradeoff |
|-------------|----------|----------|
| 2 | Visualization | Loses most structure |
| 5-10 | Clustering input | Balance of speed and fidelity |
| 50-100 | Downstream ML | Preserves more structure |
| 3072 (raw) | Direct similarity | Curse of dimensionality for clustering |

**Open questions for workshop**:
- Is there a principled way to choose n_components for UMAP before HDBSCAN?
- Trustworthiness/continuity metrics (Venna & Kaski 2006)?
- Should we compare results across multiple dimension choices?

#### 4b. Cluster number / validation

**HDBSCAN advantage**: Does not require pre-specifying k (unlike k-means).

**But**: Still requires `min_cluster_size` and `min_samples` parameters, which implicitly affect the number of clusters found.

**Validation approaches**:

| Method | What it measures | Reference |
|--------|-----------------|-----------|
| DBCV (Density-Based Cluster Validation) | Density-based quality | Moulavi et al. (2014) |
| Silhouette score | Cohesion vs separation | Rousseeuw (1987) |
| Cluster stability (bootstrap) | Robustness to perturbation | Hennig (2007) |
| Qualitative audit | Face validity | Manual inspection of cluster members |
| Agreement with known labels | External validation | Compare with GPT classifications |

**Our strategy**: Use existing 2,969 GPT-4o-mini classifications as external validation. Do HDBSCAN clusters align with human-interpretable categories?

**Open questions**:
- When HDBSCAN clusters don't match GPT categories, which is "right"?
- Is it legitimate to use supervised labels to validate unsupervised clusters?
- How to handle noise points (cluster = -1)?

#### 4c. Modality comparison: does multimodal add value?

**The key empirical question**: Is multimodal embedding worth the cost?

Strategy for answering:
1. Embed same Shorts with text-only, audio-only, and full multimodal
2. Compare clustering results across strategies
3. Measure: does multimodal reveal structure that text-only misses?

If text-only and multimodal produce identical clusters, the visual channel adds no information. If they diverge, that divergence IS the finding.

**Cost reality**:

| Strategy | Per Short | For 50K Shorts | Time |
|----------|----------|----------------|------|
| Text only | ~$0.00001 | ~$0.50 | Minutes |
| Audio + Text | ~$0.007 | ~$350 | Hours |
| Full multimodal | ~$0.043 | ~$2,150 | Days |

#### 4d. From embedding to inference

**The hard problem**: Embeddings are great for description (clustering, visualization). But how do we move from "these Shorts are similar" to causal or inferential claims?

Possible approaches:
- Embedding regression (Rodriguez, Spirling & Stewart 2023): Use embedding as DV, regress on covariates
- VVG as independent variable: Predict engagement from visual-verbal divergence
- Panel design: Legislator x month, track embedding trajectories over time
- Event study: Do embeddings shift around elections, crises?

**Question for workshop**: What is the appropriate inferential framework when your key variable is a position in latent space?

### 5. Research design and next steps (5 min)

#### Pilot results (completed, 15 videos x 4 strategies)
1. **Audio-transcript redundancy**: For speech-heavy Shorts, audio and transcript embeddings nearly identical (cos_dist 0.11-0.20); diverge sharply for meme content (0.40-0.43)
2. **Video adds most information**: text vs. multimodal distance = 0.478, confirming video channel carries unique semantic content
3. **Title-based party signal is artifactual**: title embedding shows +0.124 same-party gap; transcript shows -0.018 (no signal). Party hashtags in titles, not content, drive this.
4. **VVG is measurable**: pilot demonstrates that video-only vs. transcript distance varies meaningfully across content types

#### Full study (in progress)
1. Phase 2: Embed all 51K transcripts via Gemini text embedding (~$0.50)
2. Phase 3: Stratified 2,500-video subsample for multimodal + video_only + audio (~$234)
3. Compute VVG for subsample, test H_VVG1-H_VVG4
4. Model VVG ~ opposition + election proximity + seniority + gender + PR/SMD (legislator FE)
5. Model engagement ~ VVG + content_type + channel FE
6. Temporal analysis: VVG trends around April 2024 election

### 6. Questions I want workshop feedback on

1. **Modality choice**: Should we invest in full multimodal ($2K+) or is audio+text sufficient?
2. **VVG validation**: How do we validate that VVG actually measures "visual performance" and not just noise?
3. **Dimension reduction**: Any principled alternatives to UMAP for this use case?
4. **Clustering**: How to report HDBSCAN results in a way that satisfies quantitative reviewers?
5. **Inference**: Is embedding regression the right framework, or should we use embeddings purely for description and run standard regressions on extracted features?

---

## Files to prepare

- [ ] Slide deck (Beamer, using personal template)
- [x] Pilot results (15 videos, 4 strategies) - 7 showcase figures in outputs/
- [x] UMAP visualization examples - fig2_umap_comparison.pdf, fig5_multimodal_space.pdf
- [x] Cost/benefit comparison table - tiered strategy documented
- [ ] 1-page handout summarizing VVG concept
- [ ] Full-corpus text embedding results (Phase 2, pending)
- [ ] Subsample multimodal results (Phase 3, pending)

## Key literature for workshop discussion

- Liang et al. (2022, NeurIPS): Modality gap - the main validity concern for VVG
- Rodriguez, Spirling & Stewart (2023, APSR): Embedding regression framework
- Chari & Pachter (2023): UMAP limitations for inference (use for visualization only)
