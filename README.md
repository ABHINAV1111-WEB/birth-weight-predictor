# Birth Weight Predictor

A Flask web application that predicts newborn birth weight using a trained machine learning model (scikit-learn). 
The app uses a form-based UI to collect maternal features (gestation, parity, age, height, weight, smoke) and returns a predicted birth weight.

---

## Project structure

- `app.py` - Flask application that serves the form and prediction endpoint.
- `templates/` - HTML templates (`index.html` and `index_basic.html`).
- `dataset/babies.csv` - Dataset used for training (original dataset).
- `model/model.pkl` - Trained scikit-learn model (pickled) used for inference.
- `model_training.ipynb` - Jupyter notebook with training code and experiments.
- `requirements.txt` - Python dependencies for the project.
- `Procfile` - Heroku process file for deploying with Gunicorn.
- `runtime.txt` - Python runtime for Heroku.

---

## Features

- Web UI to input maternal data and get a birth weight prediction.
- REST-style POST endpoint `/predict` that accepts form data and returns prediction (rendered in template).
- Ready for local development and simple deployment (e.g., Heroku).

---

## Installation (local)

> These instructions assume you are using the provided virtual environment `myenv`. If you prefer to create a new one, use Python 3.10+.

1. Activate the virtual environment (PowerShell):

```powershell
.\myenv\Scripts\Activate.ps1
```

2. Install dependencies:

```powershell
pip install -r requirements.txt
```

3. Start the Flask development server:

```powershell
python app.py
```

4. Open your browser and visit: `http://127.0.0.1:5000/`

---

## Usage (API)

- Web form: visit `/` and fill the form.
- Programmatic request example (curl):

```bash
curl -X POST \
  -F "gestation=280" \
  -F "parity=0" \
  -F "age=28" \
  -F "height=64" \
  -F "weight=130" \
  -F "smoke=0" \
  http://127.0.0.1:5000/predict
```

The current app returns the value rendered in the template. You can modify `app.py` to return JSON if you need a pure API (e.g., `return jsonify({'prediction': prediction})`).

---

## Training a new model

The training workflow is included in `model_training.ipynb` and uses `dataset/babies.csv`.

High-level steps:

1. Open and run `model_training.ipynb` (Jupyter).
2. Preprocess data (handle NA, feature engineering).
3. Train a regression model (e.g., `LinearRegression`, `RandomForestRegressor` or `GradientBoostingRegressor`).
4. Save the trained model to `model/model.pkl` using `pickle`.

A minimal example to save a model in Python:

```python
import pickle
# model is your trained scikit-learn estimator
with open('model/model.pkl', 'wb') as f:
    pickle.dump(model, f)
```

Be sure to match the input columns expected by `app.py` (`gestation, parity, age, height, weight, smoke`).

---

## Deployment

- Heroku (example):
  - `Procfile` is configured to run `gunicorn app:app`.
  - `requirements.txt` and `runtime.txt` are present for compatibility.
  - To deploy: push this repo to a Heroku app and scale the web process.

For production, consider:
- Using Gunicorn (already listed in `requirements.txt`).
- Ensuring `model.pkl` is available in the deployed environment.
- Adding logging, monitoring, and error handling.
- Securing the app (e.g., CORS, input validation).

---

## Notes & Suggestions

- The app currently expects form values as numbers; consider adding server-side validation and helpful error messages.
- Add a JSON endpoint for API clients if you want programmatic access.
- Include unit tests (pytest) for data preprocessing and API behavior.
- Consider containerizing with Docker for more robust deployment.

---

## License

This project is provided as-is. Add a license file (e.g., `LICENSE`) to clarify terms (MIT recommended for open source).

---

## Contact

If you want help improving the app (tests, CI, Docker, or refactoring), open an issue or reach out to the repository owner.
