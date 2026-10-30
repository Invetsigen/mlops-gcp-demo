# MLOps Pipelines on GCP - demo repo

A small end-to-end ML pipeline that runs inside the Google Cloud free tier.
The model is a logistic regression on 5 features of the breast cancer dataset.
Follow the step-by-step student guide (PDF) to run it using only the browser.

| What | File | Where it runs |
|---|---|---|
| Data prep, plus a "next week" file with drift | src/prepare_data.py | GitHub Actions |
| Training, MLflow logging, quality gate (f1 >= 0.85) | src/train.py | GitHub Actions and Cloud Build |
| Unit tests for the API | tests/test_api.py | GitHub Actions |
| Batch scoring | src/batch_predict.py | GitHub Actions |
| Drift check (KS test) | src/drift.py | GitHub Actions |
| Real-time API | src/app.py + Dockerfile | Cloud Run |
| MLflow server | vm_startup_script.sh | e2-micro VM |
| Sample data | data/*.csv | upload to your Cloud Storage bucket |

## The one setting you must add

In your fork, open Settings, then Secrets and variables, then Actions.
Add a repository secret named MLFLOW_TRACKING_URI with the value http://YOUR_VM_EXTERNAL_IP:5000

## Run on your own computer (optional)

    pip install -r requirements-dev.txt
    python -m src.prepare_data
    python -m src.train
    pytest -q
    python -m src.batch_predict
    python -m src.drift

The MLflow server in this demo has no password. Delete the VM and the firewall rule after class.
