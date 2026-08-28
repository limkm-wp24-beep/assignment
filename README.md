# assignment
# Seoul Bike Demand — Streamlit App

Predicts hourly bike rental demand in Seoul using a tuned Gradient Boosting
Regressor trained on the SeoulBikeData dataset.

## Files

- `train_model.py` — reproduces the notebook's feature engineering + trains the
  best model (Gradient Boosting), saves it to `model.pkl`
- `app.py` — the Streamlit app that loads `model.pkl` and serves predictions
- `requirements.txt` — pinned dependencies for Streamlit Cloud
- `model.pkl` — the trained pipeline (you generate this locally, see below)

## 1. Train the model locally

You need `SeoulBikeData.csv` (the raw dataset) in this folder.

```bash
pip install -r requirements.txt
python train_model.py
```

This prints test MAE/RMSE/R² and creates `model.pkl` in this folder.
Grid search can take a couple of minutes — that's expected.

## 2. Test the app locally (optional but recommended)

```bash
streamlit run app.py
```

Open the local URL it prints (usually http://localhost:8501) and confirm
predictions work before deploying.

## 3. Push to GitHub

Create a new GitHub repo (e.g. `seoul-bike-demand-app`), then from this
folder:

```bash
git init
git add app.py train_model.py requirements.txt model.pkl README.md
git commit -m "Seoul bike demand streamlit app"
git branch -M main
git remote add origin https://github.com/<your-username>/seoul-bike-demand-app.git
git push -u origin main
```

Important: commit `model.pkl` too (not just the code) — Streamlit Cloud
only has access to what's in the repo, it can't run `train_model.py` for
you automatically. If `model.pkl` ends up over ~100MB, use
[Git LFS](https://git-lfs.github.com/) instead of a normal commit.

Do **not** commit `SeoulBikeData.csv` unless you're sure you're allowed to
redistribute it — the app only needs `model.pkl`, not the raw data.

## 4. Deploy on Streamlit Community Cloud

1. Go to https://share.streamlit.io and sign in with GitHub.
2. Click "New app".
3. Pick your repo, branch (`main`), and set the main file path to `app.py`.
4. Click "Deploy". Streamlit Cloud will install `requirements.txt` and run
   `app.py` automatically.
5. You'll get a public URL like:
   `https://<your-app-name>-<random-id>.streamlit.app`
   — same pattern as the churn example you linked.

## Updating the app later

Any `git push` to `main` auto-redeploys the app on Streamlit Cloud. If you
retrain the model, just re-run `train_model.py`, commit the new `model.pkl`,
and push.
