# Linguistic Framing of the George Floyd Killing
### A Computational CDA Analysis: Fox News vs. The New York Times
**02.137DH – Digital Humanities**

---

## Overview

This project implements a Natural Language Processing pipeline to analyse how two major US news outlets — **Fox News** and **The New York Times** — linguistically framed the killing of George Floyd (May 25, 2020). Drawing on Halliday's (1985) transitivity framework and Critical Discourse Analysis (Fairclough, 1995; van Dijk, 1988), four framing devices are operationalized computationally across a corpus of 8 news articles.

---

## Research Question

> *How do Fox News and The New York Times differ in their linguistic framing of the killing of George Floyd, with respect to agency attribution, grammatical voice, epistemic hedging, and lexical choice?*

---

## Key Findings

| Framing Device | Fox News | NYT |
|---|---|---|
| Passive voice (police-agent clauses) | 33% | 16% |
| Hedging (police-agent clauses) | 27% | 9% |
| Hedging (civilian-agent clauses) | 50% | 0% |
| Victim named as patient of violent verbs | 0% | 22% |
| Institutional euphemisms used | 0 | 14 |

χ²(4) = 8.07, p = 0.089 (n = 191 verb-instances, 8 articles)

---

## Methodology

### Corpus
- **8 articles** total: 3 Fox-original, 5 NYT-original
- **Window:** May 25 – June 8, 2020
- **Inclusion criteria:** Fox/NYT-bylined news articles (not opinion), ≥400 words, centrally focused on the Floyd killing

### Pipeline
Articles are processed using **spaCy's `en_core_web_sm`** dependency parser. For each sentence containing a target verb, the pipeline extracts:
- Grammatical subject and object (`nsubj`, `nsubjpass`, `dobj`, `agent → pobj`)
- Voice (active/passive via `auxpass`/`nsubjpass` detection)
- Actor category (Police, Victim, Civilian, Suspect, Other) via rule-based head-noun matching
- Epistemic hedging (set-intersection against a hedging-marker lexicon)

### Four CDA Framing Devices
1. **Transitivity** (Halliday, 1985) — agent/patient role distribution
2. **Grammatical voice** (Fairclough, 1995) — passive rate when police are agent
3. **Epistemic hedging** (Hyland, 2005) — modal/adverbial uncertainty markers
4. **Lexical framing** (Bolinger, 1980) — euphemism vs. direct phrasing frequency

---

## Repository Structure

```
floyd-framing-nlp/
├── pipeline.py              # Main extraction pipeline
├── analyzer.py              # Statistical analysis + visualisation
├── Floyd_Framing_Pipeline.ipynb  # Jupyter notebook (annotated)
├── corpus/
│   ├── fox/                 # Fox News articles (.txt)
│   └── nyt/                 # NYT articles (.txt)
├── extractions.csv          # Pipeline output (generated)
├── euphemisms.csv           # Euphemism analysis output (generated)
└── framing_analysis_poster.png  # 4-panel chart (generated)
```

---

## Requirements

```bash
pip install spacy pandas matplotlib seaborn scipy
python -m spacy download en_core_web_sm
```

---

## How to Run

**Option 1: Jupyter / Google Colab**

Open `Floyd_Framing_Pipeline.ipynb` and run cells sequentially.

**Option 2: Command line**

```bash
# Step 1: Run extraction pipeline
python pipeline.py

# Step 2: Run analysis and generate charts
python analyzer.py
```

Output files generated:
- `extractions.csv` — 191 verb-instance rows with agent, patient, voice, hedged columns
- `euphemisms.csv` — euphemism/direct phrase counts by outlet
- `framing_analysis_poster.png` — 4-panel comparative chart at 300 DPI

---

## Corpus Note

Article text files are not included in this repository due to copyright. Articles were collected from Fox News (`foxnews.com`) and The New York Times (`nytimes.com`) within the May 25 – June 8, 2020 window. To reproduce the analysis, collect articles meeting the inclusion criteria and place `.txt` files in `corpus/fox/` and `corpus/nyt/` with the following header format:

```
HEADLINE: [headline]
OUTLET: [Fox News / NYT]
DATE: YYYY-MM-DD
URL: [url]
BYLINE: [author]

[article body]
```

---

## References

- Bolinger, D. (1980). *Language: The Loaded Weapon*. Longman.
- Dixon, T. L., & Linz, D. (2000). Overrepresentation and underrepresentation of African Americans and Latinos as lawbreakers on television news. *Journal of Communication*, 50(2), 131–154.
- Entman, R. M. (1990). Modern racism and the images of Blacks in local television news. *Critical Studies in Mass Communication*, 7(4), 332–345.
- Entman, R. M. (1993). Framing: Toward clarification of a fractured paradigm. *Journal of Communication*, 43(4), 51–58.
- Fairclough, N. (1995). *Media Discourse*. Edward Arnold.
- Halliday, M. A. K. (1985). *An Introduction to Functional Grammar*. Edward Arnold.
- Hirschfield, P. J., & Simon, D. (2010). Legitimating police violence. *Theoretical Criminology*, 14(2), 155–182.
- Honnibal, M., et al. (2020). *spaCy: Industrial-strength NLP in Python*. Zenodo.
- Hyland, K. (2005). *Metadiscourse*. Continuum.
- van Dijk, T. A. (1988). *News as Discourse*. Lawrence Erlbaum.

---