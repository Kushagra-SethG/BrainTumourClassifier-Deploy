# Brain Tumor Classifier

NeuroScan AI is a brain tumor classification demo built around a PyTorch ResNet18 + CBAM model. The backend exposes an inference API for MRI uploads, and the React frontend provides an interface for uploading a scan and viewing the prediction result.

## Features

- MRI image classification with two outputs: `Normal` and `Tumor`
- FastAPI backend for inference and static frontend hosting
- React + Vite frontend with drag-and-drop upload
- Saved model weights included in the repository: `resnet18_cbam_best.pth`

## Project Structure

- `backend/` - FastAPI app that serves `/predict` and the built frontend in production
- `frontend/` - React + Vite UI that sends images to the backend
- `code_extract.py` - training / experimentation script
- `train.ipynb` - notebook for model development
- `results/` - saved training metrics and reports

## Requirements

- Python 3.10+ recommended
- Node.js 18+ recommended
- A working PyTorch installation appropriate for your CPU or GPU

## Setup

### 1) Backend

Install the backend dependencies:

```bash
cd backend
pip install -r requirements.txt
```

Start the API server:

```bash
python app.py
```

The API runs on `http://localhost:8000` by default.

### 2) Frontend

Install the frontend dependencies:

```bash
cd frontend
npm install
```

Start the Vite dev server:

```bash
npm run dev
```

The UI will usually be available at the local address printed by Vite, often `http://localhost:5173`. The dev server proxies `/predict` and `/health` to `http://localhost:8000`.

## Render Deployment

This repository is configured for a single Render web service. The backend serves the built Vite app from `frontend/dist`, so the deployed UI and API share the same origin.

The blueprint in `render.yaml` builds the frontend, installs the backend dependencies, and starts the FastAPI app with Render's assigned port.

Recommended Render setup:

1. Create a new Blueprint on Render and connect this GitHub repository.
2. Let Render read `render.yaml` from the repo root.
3. Deploy the generated web service.

The service will:

1. Run `npm install` and `npm run build` inside `frontend/`.
2. Install the Python packages from `backend/requirements.txt`.
3. Start the app with `uvicorn app:app --app-dir backend --host 0.0.0.0 --port $PORT`.

After deploy, Render should expose the app at the web service URL, with the frontend loading `/predict` from the same host.

## How It Works

1. Upload an MRI image from the frontend.
2. The frontend sends the file to `POST /predict`.
3. The backend preprocesses the image, runs inference with the saved model, and returns:
   - the predicted class
   - confidence
   - class probabilities

## API

### `POST /predict`

Request:

- Content type: `multipart/form-data`
- Field name: `file`

Response example:

```json
{
  "prediction": "Tumor",
  "confidence": 0.9981,
  "probabilities": {
    "Normal": 0.0019,
    "Tumor": 0.9981
  }
}
```

## Model Details

- Architecture: ResNet18 with CBAM attention blocks
- Input size: `224 x 224`
- Classes: `Normal`, `Tumor`
- Model weights are loaded from `resnet18_cbam_best.pth`

## Training / Experimentation

If you want to inspect or retrain the model, start with:

- `code_extract.py`
- `train.ipynb`

The training workflow expects a dataset organized into `train`, `val`, and `test` folders under a `data/` directory.

## Notes

- The frontend calls `/predict`, so it works locally through the Vite proxy and in production from the same Render origin.
- The backend reads `PORT` from the environment when run directly, which keeps local runs and Render deployments aligned.
- The project is intended for educational and demonstration purposes only and is not a clinical diagnostic tool.
