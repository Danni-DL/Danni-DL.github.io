# Synthetic Multimodal Biomedical Dataset Generator

Streamlit app that generates synthetic data matching the CSP assessment scenario
and lets you preview and download it.

## What it produces

| Artifact | Grain | Purpose |
|---|---|---|
| `clinical.csv` | one row per existing (subject, session) | tabular clinical + behavioral data, missingness flags |
| `embeddings.csv` / `.npz` | one row per session with an embedding | high-dimensional model output |
| `sensors.csv` | long: subject/session/channel/t/value | wearable-style multimodal sensor signal |
| `ground_truth.csv` | one row per subject | oracle latent factor + true class (validation only) |
| `config.json` | — | exact parameters used, for reproducibility |

## Scenario features baked in

- **Multimodal**: embeddings + sensor time series + tabular data per session.
- **Longitudinal**: multiple sessions, with progression for affected classes.
- **Cross-site heterogeneity**: per-site batch/scale shift on embeddings, different
  sensor sampling rates (variable series length), different label schema
  (diagnosis vs. behavioral score), quality, and dropout.
- **Meaningful missingness**: structural (a site never collects sensors),
  MNAR-ish dropout (higher-severity subjects skip later sessions), and MAR
  clinical gaps.
- **Evolving model**: `model_version` reseeds the embedding projection so the same
  subject yields different embeddings across revisions.
- **Recoverable ground truth**: every modality is driven by a shared latent
  factor, so a prototype can be validated on synthetic data even with no agreed
  metric on the real data.

## Deploy on Streamlit Community Cloud

1. Push `app.py`, `synthetic_data.py`, `requirements.txt` to a public GitHub repo.
2. Go to share.streamlit.io, **New app**, select the repo and `app.py`.

## Run locally

```bash
pip install -r requirements.txt
streamlit run app.py
```
