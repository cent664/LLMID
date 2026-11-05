# Project page update spec: "Invariant Features in Language Models" (NeurIPS 2026)

This document is a build brief for revising the project page at https://cent664.github.io/LLMID/ (hosted with GitHub Pages from the paper's repository). It explains what to change, why, and gives every number the page needs. Do not use numbers from anywhere else.

---

## 0. Instructions to the builder

- **Starting point:** the current `index.html` in the repository. Keep its existing visual style, fonts and colors unless a section below says otherwise. Keep existing image assets (figure PNGs) and reuse them where noted.
- **Hosting:** GitHub Pages serves static files. Do not add a build step, server code or a framework that needs compiling. One `index.html` plus a small `assets/` folder (CSS, JS, images) is the target.
- **Libraries:** if a chart library is needed, use one pinned version loaded from a CDN (for example `https://cdn.jsdelivr.net/npm/chart.js@4.4.1`). Plain SVG or canvas is also fine. Use no other external dependencies.
- **Data:** every number on the page must come from Section 4 of this document ("Data"), which is copied from the camera ready paper. Do not invent, interpolate, round differently from what is stated, or generate synthetic "results". The only synthetic data allowed is in the toy method demo (Section 3.3), and it must be labelled as an illustration.
- **Copy style:** short, plain sentences. No em dashes. Avoid strong words such as "proves", "always", "perfectly", "universal". The paper's claims are deliberately scoped (local, approximate, evidence consistent with), and the page must match that.
- **Accessibility and layout:** must work at phone width (no horizontal scrolling, 16px side padding). Charts need axis labels and units. Do not rely on color alone; label series directly or with a legend. Support light and dark mode if the current page does.
- **Placeholders:** where a link or value is marked `TODO_AUTHOR`, render the element but leave a visible HTML comment so the author can fill it in. Do not invent URLs.

---

## 1. Why the page is changing (justification)

The current page was written for the arXiv preprint, before the camera ready revision. Since then the paper changed in ways the page does not reflect:

1. **Venue and status.** The paper is now a NeurIPS 2026 poster. Visitors arrive from the conference site, the poster or the proceedings, and the page should present it as a NeurIPS paper and link to the materials they expect (paper, code, data, poster, slides, BibTeX).
2. **Larger dataset.** The evaluation grew from 10 semantic groups to 1,400 groups (57,400 sentences across 13 domains). The page does not mention the dataset at all, yet it is the main change that addresses reviewer concerns about scale.
3. **New evidence.** The camera ready adds a cPCA baseline, 95% bootstrap confidence intervals, a pooling comparison, a multihop reasoning check and a semantic ambiguity stress test. These answer specific reviewer questions, and none of them are on the page.
4. **Scoped claims.** The paper now describes the framework as a local, approximate decomposition and has an explicit limitations section. A project page that sounds stronger than the paper invites the same criticism the reviewers raised. The page should state the limitations briefly.
5. **Two headline numbers on the page do not match the paper exactly.** "~90–96%" attribution accuracy does not cover Phi-4-mini (as low as 85.8% in Table 5). "~0.01–0.02" nuisance energy is the paper's own wording, but Table 3 reaches 0.0231. Both should use exact ranges.
6. **Explaining the gap and the method.** Project pages are read in a minute or two by people deciding whether to read the paper. The fastest way to show the idea is a concrete example of paraphrase vs meaning change, plus a small interactive picture of what the generalized eigenproblem does. Both can be built from material already in the paper, without new experiments.
7. **What is left out, and why.** Some appendix results are left off the page on purpose. The per group nuisance intervention table (Table 8) has values the authors are still checking. The dense per layer grids (Figures 6 to 8) are hard to read at web size and are better in the paper. Interactive elements that would imply live per input results (for example "type a sentence and see its projection") are excluded, because they would need a hosted model and new runs.

---

## 2. Page structure (top to bottom)

Order of sections:

1. Header (title, venue, authors, links, TL;DR, Figure 1)
2. The problem (gap, plus a dataset example explorer)
3. Method (three steps, plus an interactive toy demo)
4. Results (four subsections, each backed by a paper table)
5. Additional checks (four small cards)
6. Limitations
7. Citation
8. Footer

Navigation bar anchors: `Problem`, `Method`, `Results`, `Limitations`, `Cite`, plus external buttons `Paper`, `arXiv`, `Code`.

Remove the current "Additional views" section (nuisance swap grid and k sensitivity grids).

---

## 3. Section by section specification

### 3.1 Header

- **Venue badge:** `NeurIPS 2026 · Poster`. Replace the current "arXiv:2605.06458 · cs.LG" preprint label. The arXiv ID can stay as a secondary line.
- **Title:** "Invariant Features in Language Models: Geometric Characterization and Model Attribution" (full camera ready title; the current page uses only the first half).
- **Authors:** Agnibh Dasgupta\*, Abdullah All Tanvir\*, Xin Zhong†. Note the middle name "All", which the current page omits.
  - Under the names: "\* Equal contribution. † Corresponding author."
  - Affiliation: Department of Computer Science, University of Nebraska Omaha.
- **Link buttons**, in this order:
  - Paper (`TODO_AUTHOR`: NeurIPS proceedings or OpenReview URL)
  - arXiv (https://arxiv.org/abs/2605.06458)
  - Code (`TODO_AUTHOR`: repository URL)
  - Data (`TODO_AUTHOR`, or hide if not released)
  - Poster (`TODO_AUTHOR`)
  - Slides (`TODO_AUTHOR`)
  - BibTeX (anchor to #cite)
  - Hide any button whose URL is still `TODO_AUTHOR` at deploy time, but keep it in the HTML as a comment.
- **TL;DR** (one or two sentences, shown above the figure):
  > Paraphrases move a language model's hidden states along "nuisance" directions, while meaning lives in a separate, more stable subspace. We find that subspace by contrasting meaning changing and meaning preserving edits, test it with interventions, and use it to tell models apart without training.
- **Figure 1:** keep the existing overview image. Caption from the paper: "(A) Paraphrases propagate through layers and tend to converge to compact zones. (B) Local geometric interpretation: nuisance vs semantic tangent directions. (C) Geometric decomposition detects invariant zones for model attribution."

### 3.2 The problem

**Short text (about 80 words):**
> Language models give consistent answers to prompts that are worded differently. This suggests they represent meaning in a way that is stable under paraphrasing. But a raw hidden state mixes meaning with surface form, so two paraphrases can sit far apart while two different questions sit close together. Prior work shows semantic structure exists in these representations, but does not separate meaning preserving variation from meaning changing variation. We ask where a stable semantic subspace exists, how to find it, and whether it differs between models.

**Dataset example explorer (light interactive):**
- A row of tabs or a dropdown listing the 10 group names from Data D1 (Cats, France, Python, ...).
- For the selected group, show four labelled boxes:
  - **Anchor**
  - **Semantic preserving (SP):** both SP variants
  - **Semantic changing (SC), soft:** both SC variants
  - **Hard SC:** only for the "France" example, use the hard SC sentence from D1. For the other groups, show the line "Hard SC variants change the topic or domain (for example, from cats to monarch butterfly migration)."
- Below the explorer, a one line dataset summary from D2: "1,400 semantic groups · 13 domains · 57,400 sentences · each group: 1 anchor, 20 SP, 10 soft SC, 10 hard SC · disjoint discovery and validation splits."
- **Why:** this makes SP and SC concrete in a few seconds, and it shows the larger dataset, which is one of the main camera ready changes.

### 3.3 Method

**Three steps.** Keep the current three numbered steps, with these wording edits:
1. **Local geometric definition.** Invariant features are stable under meaning preserving edits and still distinguish different meanings. Locally, the tangent space splits approximately into semantic and nuisance directions.
2. **Contrastive sensitivity decomposition.** From the hidden state changes caused by SP edits and by SC edits, compute two covariances, S_SP and S_SC. Solve the generalized eigenproblem `S_SC v = λ (S_SP + εI) v`. Directions with large λ vary a lot under meaning changes and little under paraphrases. The top k form the invariant subspace. Its orthogonal complement is the nuisance subspace, giving `z = z_inv + z_nuis`.
3. **Zero shot attribution.** Concatenate the invariant projections from the layers with the strongest separation (the "invariant zone") into a model signature, and match a test model to the closest reference signature.

Render the equation with KaTeX (pinned CDN version) or as clean HTML. Keep Figures 2 and 3 from the current page beside or below the steps.

**Interactive toy demo (the main new interactive element).**

- **Purpose:** show why the generalized eigenvector finds the "meaning" direction when plain variance (PCA) does not.
- **Label, shown on the widget:** "Illustration with synthetic 2D data. Not model activations."

- **Canvas setup:**
  - A 2D canvas, about 360 to 480 px square and responsive.
  - Generate 150 SP displacement points from a zero mean Gaussian with covariance R(θ_sp) · diag(σ_sp_long², σ_sp_short²) · R(θ_sp)ᵀ. This is an elongated cloud, since paraphrases move a lot along one direction.
  - Generate 150 SC displacement points from a zero mean Gaussian with covariance S_SP_true + σ_sem² · u uᵀ, where u is a fixed unit "meaning" direction, for example at 75 degrees. This matches the paper's assumption that SC edits traverse the nuisance directions plus extra semantic directions.
  - Use a seeded random generator so the picture is stable between reloads.

- **Computation (in JS, closed form for 2x2):**
  - Empirical covariances S_SP and S_SC from the sampled points.
  - B = S_SP + εI, with ε = 1e-3 relative to the trace.
  - Solve S_SC v = λ B v. For example, compute B^(-1/2) from the 2x2 eigen decomposition of B, form M = B^(-1/2) S_SC B^(-1/2), take the eigenvectors u of M, and set v = B^(-1/2) u.
  - Sort by λ.
  - PCA direction: the top eigenvector of the covariance of all points (SP and SC pooled).

- **Draw:**
  - SP points in one color and SC points in another, labelled "SP (paraphrase)" and "SC (meaning change)".
  - 2 sigma ellipses of S_SP and S_SC.
  - A thick line through the origin along the top generalized eigenvector, labelled "invariant direction (λ = …)".
  - A dashed line along the PCA direction, labelled "PCA direction". It can be toggled with a checkbox, on by default.
  - A readout: "λ₁ = …, λ₂ = …".

- **Controls (sliders):**
  - "Paraphrase direction" θ_sp from 0 to 180 degrees.
  - "Paraphrase spread" σ_sp_long, from small to large.
  - "Meaning change strength" σ_sem.

- **Behavior to check:** when the paraphrase spread is large, the PCA line should follow the SP cloud while the invariant line stays near the meaning direction u. Add one caption sentence that says this:
  > When paraphrases cause large but meaning irrelevant motion, the direction of largest variance follows them. Dividing by paraphrase variance (the generalized eigenproblem) recovers the meaning direction instead.

- **Why this demo is justified:** it needs no experiment, it illustrates Eq. 2 of the paper directly, and the PCA comparison matches the paper's PCA and cPCA baseline results (Results 4.2 and 4.4 below). It is the clearest way to explain "high λ means stable under paraphrase, sensitive to meaning" to a non specialist.

**Optional small static diagram.** Below the demo, add a small SVG showing a hidden state h split into h_inv and h_nuis, with arrows for "swap h_inv from a donor" and "swap h_nuis from a donor". This sets up the intervention results. Keep it schematic, with no numbers.

### 3.4 Results

Use a short heading plus a one sentence takeaway for each subsection, then the chart or figure, then a caption that names the source table. Keep takeaways scoped.

**4.1 Where invariance appears.**
- Takeaway: "Separation between meaning changing and meaning preserving variation grows in mid to late layers."
- Content: the existing eigenvalue profile figure (paper Figure 4).
- Caption: "Per layer generalized eigenvalue profiles (top 32 directions, log scale) for one representative semantic group in Llama-3-8B-Instruct. Layers are ranked by eigenvalues normalized within each layer, which reduces bias toward deeper layers." Do not claim this pattern is identical across models.

**4.2 Meaning lies outside the nuisance subspace.**
- Takeaway: "Displacement between semantic groups puts little of its energy in the nuisance subspace, much less than with PCA or cPCA subspaces."
- Chart A: a line or grouped bar chart from Data D4 (Table 12, Llama). The x axis is layer (1, 4, 8, 12, 16), the y axis is nuisance projection energy E_nuis on a **log scale**, with three series: Ours, cPCA, PCA. Lower is better; say so on the axis label.
- Stat line under the chart, from Data D3: "Across nine models, E_nuis with our subspace ranges from 0.0105 to 0.0231 (Table 3)."
- Definition, in small text: "E_nuis = fraction of the displacement between two group centroids that lies in the nuisance subspace."

**4.3 Interventions: the invariant subspace controls meaning.**
- Takeaway: "Swapping in the invariant component from a paraphrase barely changes the next token distribution. Swapping it from a different meaning changes it a lot."
- Chart B: a grouped bar chart from Data D5 (Table 4). One group per model (nine models) with three bars each: Within group SP donor, Within group SC donor, Between group donor. The y axis is KL divergence. Give a one line legend explanation of each donor type.
  - Add a model dropdown that highlights one model, or show all nine. Either is fine.
  - Caption: "KL divergence between original and intervened next token distributions after replacing the invariant component of the final token hidden state. One representative semantic group; averaged over the top 4 invariant layers (paper Table 4)."
- Chart C (uncertainty): from Data D6 (Table 16, 1,400 groups, 95% bootstrap CIs). Use a small multiple or a layer selector (3, 5, 8). For each model show two bars, Nuisance KL and Invariant KL, on a **log y axis**, with CI whiskers. The CIs are very narrow, so the whiskers may be barely visible. That is expected and should not be exaggerated.
  - Caption: "Mean over 1,400 semantic groups with 95% bootstrap confidence intervals (1,000 resamples). Invariant swaps produce larger changes than nuisance swaps for every model at every reported layer."
- Do not show nuisance MSE values from Tables 7 or 8 anywhere on the page.

**4.4 Zero shot model attribution.**
- Takeaway: "Invariant zone signatures identify the source model, including after fine tuning and distillation, and do better than PCA and cPCA signatures."
- Chart D: from Data D7 (Table 5). A grouped bar chart or a small heatmap with models as rows and B, FT, D as columns. Add a toggle for Group 1, 2 or 3. The y axis is accuracy in %; start it at 80, and state that the axis does not start at 0.
- Stat line: "Base models: 89.2% to 96.1%. Fine tuned and distilled variants: about 2 to 4 points lower (Table 5)."
- Chart E: from Data D8 (Table 6). Three clusters (Base, Fine tuned, Distilled) with three bars each (PCA, cPCA, Ours).
  - Caption: "Averaged over Llama, Mistral, Qwen2.5 and Gemma with matched subspace dimension."
  - Mention the gains over cPCA: +6.13, +6.68 and +7.60 points.
- Keep the existing t-SNE figure (MS MARCO, full space vs invariant subspace), with caption: "200 MS MARCO queries per model. Invariant subspace learned on the controlled dataset and applied without recomputation."
- One line on how variants were made, from D10: "Fine tuned: LoRA on Alpaca. Distilled: smaller students trained to match the teacher's outputs. Signatures come from the base model only."

### 3.5 Additional checks (four small cards in a row, stacking on mobile)

Each card has a title, one sentence and one headline comparison from the data, plus "Paper, Appendix X".

1. **Semantic ambiguity (Appendix F).**
   - Sentence: "On scope, punctuation and attachment ambiguities, our subspace keeps lower nuisance energy than PCA and cPCA."
   - Headline: "Ambiguity, layer 4: 0.0084 vs 0.0718 (cPCA) vs 0.0921 (PCA)."
   - Example: "I saw the man who was holding a telescope." → "I saw the man with a telescope."
2. **Multihop reasoning (Appendix I).**
   - Sentence: "On 200 two step reasoning examples, semantic component swaps change outputs more than nuisance swaps."
   - Headline: "Layer 4 KL: 0.360 vs 0.069."
   - Example: "Maya lives in Brookfield. Brookfield is in Canada." (SC: "...is in France").
3. **Pooling (Appendix H).**
   - Sentence: "Mean pooling and last token pooling both keep SC effects above SP effects. Last token pooling disturbs paraphrases less."
   - Headline: "Llama, layer 3, SP KL: 0.000037 (last token) vs 0.001190 (mean)."
4. **Subspace size (Appendix C).**
   - Sentence: "Results are similar for k = 1, 10 and 32 invariant directions, suggesting the main structure is low dimensional."
   - No number.

### 3.6 Limitations

Three bullets, close to the paper's Section 5:
- "The decomposition is local and approximate: each semantic group has its own invariant subspace. It is not a universal decomposition of language representations."
- "The dataset is synthetically constructed in English with short inputs. Naturally occurring, multilingual and long context inputs remain to be evaluated."
- "The nuisance subspace is not fully disentangled. Nuisance swaps still cause some output change, even with paraphrase donors, so it may hold residual semantic information."

Optionally, show paper Figure 5 (nuisance swap KL by layer for one model) here, captioned "SP and SC nuisance swaps rise together with depth, without consistent separation."

**Why:** stating limitations on the page matches the paper's scoped framing and the camera ready changes requested by reviewers.

### 3.7 Citation

Two BibTeX blocks, each with a Copy button:
- The NeurIPS entry: `@inproceedings{dasgupta2026invariant, ...}` with `booktitle = {Advances in Neural Information Processing Systems}` and `year = {2026}`. Leave volume and pages as `TODO_AUTHOR`.
- The existing arXiv entry, labelled "Preprint".

The full author list in both: "Dasgupta, Agnibh and Tanvir, Abdullah All and Zhong, Xin".

### 3.8 Footer

"Department of Computer Science, University of Nebraska Omaha", plus links to the paper, arXiv and code.

---

## 4. Data (copied from the camera ready paper; use exactly)

### D1. Example groups (Table 13; hard SC from Section 4.1)

Each entry gives an anchor, SP variants and soft SC variants.

**Cats**
- Anchor: Describe the hunting behavior of domestic cats.
- SP: Describe the hunting habits of domestic cats. | Detail how house cats pursue and capture prey.
- SC: Describe the sleeping behavior of domestic cats. | Describe the nocturnal behavior of domestic cats.

**France**
- Anchor: What is the official language of France?
- SP: Which language is officially recognized in France? | Identify the language that serves as the official one in France.
- SC: What is the capital city of France? | What is the official flower of France?
- Hard SC (Section 4.1): What is the longest river in South America?

**Python**
- Anchor: Why did Python become the dominant language for data science?
- SP: What factors led to Python becoming the dominant language in data science? | Explain the reasons behind Python's leading position in data science.
- SC: Why did Python become popular in academia? | Why did Python become popular for scientific computing?

**Photosynthesis**
- Anchor: What are the pros and cons of engineering crops for enhanced photosynthesis?
- SP: What are the advantages and disadvantages of engineering crops for improved photosynthesis? | Discuss the benefits and costs of engineering crops to improve photosynthetic performance.
- SC: What are the pros and cons of engineering crops for drought resistance? | What are the pros and cons of engineering crops for improved carbon sequestration?

**Democracy**
- Anchor: Describe the role of free speech in a democratic society.
- SP: What role does free speech play in a democratic society? | Explain the function of free expression in democratic systems.
- SC: Describe the role of free press in a democratic society. | Describe the role of voting rights in a democratic society.

**Einstein**
- Anchor: What is Einstein's most famous theory?
- SP: Which theory is Albert Einstein best known for? | Identify the theory for which Albert Einstein is most widely recognized.
- SC: What is Einstein's nationality? | What is Einstein's most famous thought experiment?

**Climate Change**
- Anchor: Describe the effect of climate change on global sea levels.
- SP: What impact does climate change have on global sea levels? | Explain how climate change affects sea levels on a global scale.
- SC: Describe the effect of climate change on Arctic ecosystems. | Describe the effect of climate change on global monsoon patterns.

**Calculus**
- Anchor: How do you find the derivative of a composite function?
- SP: How is the derivative of a composite function calculated? | Explain how to differentiate a function composed of other functions.
- SC: How do you evaluate the limit of a function? | How do you compute the second derivative of a function?

**Meditation**
- Anchor: Describe the cognitive benefits of regular meditation practice.
- SP: What cognitive benefits are associated with regular meditation practice? | Explain the mental effects of consistent meditation practice.
- SC: Describe the physical benefits of regular meditation practice. | Describe the psychological benefits of regular meditation practice.

**Bread**
- Anchor: What is the primary leavening agent in sourdough bread?
- SP: What is the main leavening agent in sourdough bread? | Identify the ingredient responsible for leavening in sourdough bread.
- SC: What is the main ingredient in sourdough bread? | What is the typical crust texture of sourdough bread?

### D2. Dataset (Table 1)

| Item | Value |
|---|---|
| Semantic groups | 1,400 |
| Domains | 13 |
| Total sentences | 57,400 |
| Per group | 1 anchor, 20 SP, 10 soft SC, 10 hard SC |
| Split | Discovery and validation are disjoint, each with 10 SP, 5 soft SC and 5 hard SC |

### D2b. Models (Table 2)

| Model | Size |
|---|---|
| Mistral-7B-Instruct-v0.3 | 7B |
| Llama-3-8B-Instruct | 8B |
| Gemma-7B-IT | 7B |
| Qwen2.5-7B-Instruct | 7B |
| GLM-4-9B-Chat | 9B |
| DeepSeek-MoE-16B-Chat | 16B |
| InternLM2-Chat-7B | 7B |
| Phi-4-Mini-Instruct | 4B |
| Falcon3-7B-Instruct | 7B |

### D3. Nuisance projection energy, representative groups G1, G4, G6 (Table 3)

```json
{"Mistral":[0.0189,0.0213,0.0198],"Llama3":[0.0127,0.0141,0.0135],"Qwen2.5":[0.0158,0.0172,0.0164],
 "Gemma":[0.0169,0.0184,0.0176],"Phi4-mini":[0.0215,0.0231,0.0223],"GLM4":[0.0112,0.0126,0.0119],
 "DeepSeek":[0.0105,0.0118,0.0112],"InternLM2":[0.0121,0.0134,0.0128],"Falcon3":[0.0116,0.0129,0.0123]}
```

### D4. Layerwise nuisance energy, Llama, Ours vs cPCA vs PCA (Table 12)

```json
{"layer":[1,4,8,12,16],
 "ours":[0.0025,0.0050,0.0085,0.0053,0.0047],
 "cpca":[0.0323,0.0676,0.0691,0.0590,0.0152],
 "pca":[0.7320,0.5584,0.8851,0.9572,0.9689]}
```

### D5. Invariant component intervention KL (Table 4)

Format is [between group, within SP, within SC].

```json
{"Mistral-7B":[0.974,0.036,0.341],"Llama-3-8B":[0.986,0.040,0.360],"Gemma-7B":[0.979,0.042,0.382],
 "Qwen2.5-7B":[0.984,0.047,0.401],"GLM-4-9B":[0.983,0.034,0.337],"DeepSeek-MoE":[0.985,0.052,0.427],
 "InternLM2-7B":[0.984,0.039,0.354],"Phi-4-mini":[0.981,0.045,0.391],"Falcon3-7B":[0.986,0.049,0.418]}
```

### D6. Bootstrap means with 95% CI half widths, 1,400 groups (Table 16)

Format per layer is [nuisKL, nuisKL_ci, invKL, invKL_ci, nuisMSE, nuisMSE_ci, invMSE, invMSE_ci].

```json
{
"Mistral-7B":{"3":[0.000294,0.000022,0.183353,0.000924,0.001498,0.000840,0.554108,0.000398],
              "5":[0.001740,0.000084,0.048431,0.000880,0.001239,0.000699,0.124954,0.000296],
              "8":[0.000535,0.000022,0.115219,0.000627,0.002914,0.000477,0.538303,0.000858]},
"Llama-3-8B":{"3":[0.001294,0.000098,0.010279,0.000647,0.003071,0.000194,0.024246,0.001549],
              "5":[0.006545,0.000497,0.039726,0.001782,0.016768,0.001773,0.091234,0.003889],
              "8":[0.009954,0.000670,0.109119,0.003465,0.027611,0.002654,0.211765,0.007343]},
"Qwen2.5-7B":{"3":[0.001692,0.000049,0.141399,0.000169,0.002138,0.000237,0.280972,0.000876],
              "5":[0.000498,0.000021,0.103953,0.000996,0.001561,0.000520,0.449803,0.000882],
              "8":[0.002991,0.000074,0.134659,0.001000,0.001339,0.000355,0.024437,0.000509]},
"Gemma-7B":  {"3":[0.000982,0.000041,0.083551,0.000415,0.001444,0.000264,0.528555,0.000336],
              "5":[0.000252,0.000035,0.174981,0.000732,0.002773,0.000624,0.362715,0.000635],
              "8":[0.001787,0.000030,0.175533,0.000438,0.002566,0.000295,0.143605,0.000834]},
"Phi-4-mini":{"3":[0.001135,0.000073,0.150466,0.000103,0.002899,0.000279,0.216349,0.000892],
              "5":[0.001609,0.000055,0.157141,0.000147,0.002398,0.000918,0.200817,0.000420],
              "8":[0.000629,0.000069,0.137118,0.000304,0.002453,0.000747,0.483504,0.000699]},
"GLM-4-9B":  {"3":[0.001060,0.000083,0.025708,0.000593,0.002141,0.000409,0.017117,0.000323],
              "5":[0.002712,0.000082,0.077275,0.000622,0.002567,0.000122,0.074551,0.000403],
              "8":[0.001142,0.000044,0.181325,0.000979,0.001591,0.000384,0.142025,0.000125]},
"DeepSeek-MoE-16B":{"3":[0.002609,0.000038,0.147375,0.000650,0.002082,0.000829,0.265895,0.000227],
              "5":[0.002764,0.000089,0.190571,0.000611,0.002600,0.000257,0.029917,0.000208],
              "8":[0.001402,0.000064,0.027540,0.000402,0.001993,0.000376,0.117297,0.000493]},
"InternLM2-7B":{"3":[0.002665,0.000025,0.163942,0.000931,0.001603,0.000531,0.231792,0.000880],
              "5":[0.000171,0.000056,0.106152,0.000652,0.002800,0.000651,0.209048,0.000898],
              "8":[0.002509,0.000086,0.037368,0.000844,0.001217,0.000796,0.292352,0.000495]},
"Falcon3-7B":{"3":[0.000382,0.000039,0.087830,0.000734,0.002887,0.000966,0.117867,0.000723],
              "5":[0.001994,0.000037,0.102695,0.000221,0.002094,0.000239,0.392787,0.000200],
              "8":[0.001578,0.000037,0.027555,0.000999,0.002944,0.000565,0.310904,0.000624]}
}
```

Chart C uses the KL fields. MSE fields are included for completeness; plot them only if a toggle is added.

### D7. Attribution accuracy %, B / FT / D, three groups (Table 5)

Format is [B, FT, D] per group.

```json
{
"Mistral":  {"G1":[93.2,91.4,89.8],"G2":[92.7,91.0,89.4],"G3":[93.5,91.8,90.2]},
"Llama3":   {"G1":[94.1,92.5,90.9],"G2":[93.8,92.1,90.5],"G3":[94.4,92.9,91.3]},
"Gemma":    {"G1":[92.8,90.9,89.2],"G2":[92.3,90.5,88.8],"G3":[93.0,91.2,89.6]},
"Qwen2.5":  {"G1":[93.6,91.8,90.1],"G2":[93.1,91.3,89.7],"G3":[93.9,92.0,90.5]},
"Phi4-mini":{"G1":[89.6,87.8,86.1],"G2":[89.2,87.4,85.8],"G3":[90.1,88.2,86.5]},
"GLM4":     {"G1":[95.2,93.6,91.9],"G2":[94.8,93.1,91.5],"G3":[95.6,94.0,92.3]},
"DeepSeek": {"G1":[95.8,94.2,92.6],"G2":[95.3,93.7,92.0],"G3":[96.1,94.5,92.9]},
"InternLM2":{"G1":[94.9,93.3,91.7],"G2":[94.5,92.9,91.3],"G3":[95.2,93.6,91.9]},
"Falcon3":  {"G1":[95.5,93.9,92.2],"G2":[95.0,93.4,91.8],"G3":[95.8,94.2,92.5]}
}
```

### D8. Attribution vs baselines, averaged over 4 models (Table 6)

| Variant | PCA | cPCA | Ours |
|---|---|---|---|
| Base | 86.08 | 89.33 | 95.45 |
| Fine tuned | 83.98 | 87.53 | 94.20 |
| Distilled | 81.25 | 84.65 | 92.25 |

Gains over cPCA: +6.13, +6.68 and +7.60 points.

### D9. Additional checks

**Ambiguity stress test (Table 15)**, nuisance energy averaged across models; lower is better.

| Setting | Layer | Ours | cPCA | PCA |
|---|---|---|---|---|
| Ambiguity | 3 | 0.0230 | 0.0718 | 0.1170 |
| Ambiguity | 4 | 0.0084 | 0.0718 | 0.0921 |
| Clear SP | 3 | 0.0122 | 0.0389 | 0.2673 |
| Clear SC | 4 | 0.0123 | 0.1284 | 0.2843 |

Examples (Table 14):
- Clear SP: "Alice is taller than Bob." → "Bob is shorter than Alice."
- Ambiguity: "I saw the man who was holding a telescope." → "I saw the man with a telescope."
- Clear SC: "Not every student submitted the assignment." → "No student submitted the assignment."

**Multihop (Table 19)**, 200 examples, averaged across models.

| Layer | Nuisance MSE | Semantic MSE | Nuisance KL | Semantic KL |
|---|---|---|---|---|
| 3 | 0.143 | 0.970 | 0.064 | 0.353 |
| 4 | 0.130 | 0.908 | 0.069 | 0.360 |
| 5 | 0.107 | 0.832 | 0.080 | 0.391 |

Example (Table 18): "In which country does Maya live?"
- Anchor: "Maya lives in Brookfield. Brookfield is in Canada." Answer: Canada.
- SP: "Maya resides in Brookfield. Brookfield is located in Canada." Answer: Canada.
- SC: "Maya lives in Brookfield. Brookfield is in France." Answer: France.

**Pooling (Table 17)**, layer 3.

| Model | Pooling | SP KL | SC KL | SP MSE | SC MSE | Nuisance energy |
|---|---|---|---|---|---|---|
| Llama-3-8B | Mean | 0.001190 | 0.065358 | 0.003095 | 0.123798 | 0.061882 |
| Llama-3-8B | Last token | 0.000037 | 0.000876 | 0.000087 | 0.001567 | 0.013685 |
| Mistral-7B | Mean | 0.002501 | 0.027186 | 0.000094 | 0.004563 | 0.061063 |
| Mistral-7B | Last token | 0.000027 | 0.001793 | 0.000089 | 0.000729 | 0.009884 |
| Qwen2.5-7B | Mean | 0.000692 | 0.038182 | 0.000187 | 0.007310 | 0.012205 |
| Qwen2.5-7B | Last token | 0.000087 | 0.000445 | 0.000094 | 0.000774 | 0.006332 |
| Gemma-7B | Mean | 0.001159 | 0.008129 | 0.003581 | 0.007889 | 0.066067 |
| Gemma-7B | Last token | 0.000108 | 0.000355 | 0.000199 | 0.000385 | 0.044512 |

### D10. Adapted variants (Appendix J.6)

- **Fine tuned:** LoRA supervised fine tuning on Alpaca (about 52k instruction and response pairs), one epoch.
- **Distilled:** students trained to match the teacher's output distribution on Alpaca.
  - Mistral and Gemma → Gemma-2B
  - Llama → TinyLlama-1.1B-Chat
  - Qwen → Qwen-1.5B
  - The other five models → Phi-3-Mini
- 27 model variants in total.
- Signatures and layer selection come from the base model only.

---

## 5. Do not

- Do not show Table 7 nuisance columns, Table 8, or any nuisance MSE comparison between within group and between group settings.
- Do not show the per layer grids (paper Figures 6, 7, 8) or the k sensitivity grids from the current page.
- Do not build any widget that accepts user text or claims to compute projections for new inputs.
- Do not describe the decomposition as exact, universal or proven. Use "approximate", "local", and "evidence consistent with a causal role".
- Do not use "~90–96%". Use the ranges in Section 3.4.
- Do not use em dashes in page copy.

---

## 6. Acceptance checklist

- [ ] Venue badge says NeurIPS 2026 Poster, and the full title and the full author names are correct.
- [ ] Every number on the page appears in Section 4 of this document.
- [ ] The toy demo is visibly labelled as synthetic, the sliders work, and the PCA line visibly departs from the invariant line when paraphrase spread is high.
- [ ] Log scale is used for Charts A and C, and the axis labels say "lower is better" where relevant.
- [ ] The Table 4 chart caption says "one representative semantic group" and "top 4 invariant layers".
- [ ] All TODO_AUTHOR links are either filled or hidden.
- [ ] The page works at 375px width with no horizontal scroll.
- [ ] It loads from GitHub Pages with no console errors and no build step.
