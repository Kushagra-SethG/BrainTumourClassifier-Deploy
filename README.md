# Brain Tumor Classifier

NeuroScan AI is a brain tumor classification demo built around a PyTorch ResNet18 + CBAM model. For Hugging Face Spaces, the project is deployed as a single Gradio app that loads the model and serves the upload UI directly from Python.

## Features

- MRI image classification with two outputs: `Normal` and `Tumor`
- Gradio web UI for Hugging Face Spaces
- Saved model weights included in the repository: `resnet18_cbam_best.pth`

## Project Structure

- `app.py` - Gradio Space entrypoint for inference and UI
- `backend/` - original FastAPI backend kept for local/API-based use
- `frontend/` - original React + Vite UI kept for local development
- `code_extract.py` - training / experimentation script
- `train.ipynb` - notebook for model development
- `results/` - saved training metrics and reports

## Requirements

- Python 3.10+ recommended
- A working PyTorch installation appropriate for your CPU or GPU
- `gradio`

## Setup

### Local Gradio Run

Install the Python dependencies:

```bash
pip install -r requirements.txt
```

Start the Gradio app:

```bash
python app.py
```

The UI will open on `http://localhost:7860`.

### Hugging Face Spaces

1. Create a new Space and choose the `Gradio` SDK.
2. Push the repository with `app.py` at the root.
3. Make sure `requirements.txt` includes `gradio`, `torch`, `torchvision`, and `pillow`.
4. Keep `resnet18_cbam_best.pth` in the repository root so the app can load it.

## How It Works

1. Upload an MRI image in the Gradio interface.
2. The app preprocesses the image and runs inference with the saved model.
3. The UI shows the predicted class and class probabilities.

## API

The Gradio app does not expose a separate public REST endpoint by default. If you want the old `/predict` API as well, keep the `backend/` app for local use.

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

- The repository now deploys cleanly as a single Gradio Space.
- The project is intended for educational and demonstration purposes only and is not a clinical diagnostic tool.