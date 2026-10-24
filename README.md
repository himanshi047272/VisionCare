# 🩻 VisionCare Analytics

**An AI-powered chest X-ray classification research prototype built with PyTorch, ResNet18, and FastAPI.**

VisionCare Analytics is a local web application that uses deep learning to classify chest X-ray images into five dataset-defined categories. It combines an image-classification pipeline with an interactive dashboard for image uploads, prediction history, dataset inspection, and model evaluation insights.

> **Disclaimer:** This project is for educational and research purposes only. It is not a medical device and must not be used to diagnose, treat, or make clinical decisions about any health condition.

## ✨ Features

- **Five-class image classification:** Bacterial Pneumonia, Corona Virus Disease, Normal, Tuberculosis, and Viral Pneumonia.
- **Transfer learning:** ResNet18 initialized with pretrained ImageNet weights and fine-tuned on chest X-ray images.
- **Interactive dashboard:** View scan statistics, recent predictions, and model status.
- **Image upload:** Submit PNG or JPEG chest X-rays for model inference.
- **Prediction history:** Review saved predictions and export available records.
- **Dataset inspection:** Inspect image collections, labels, and class distributions.
- **Model insights:** View evaluation results and analyze classification performance.
- **Explainability:** Includes an attention-map/Grad-CAM workflow for exploratory model visualization.
- **Local inference:** Run the application on your own machine using a Python backend.

## 🧠 Model and Training

VisionCare uses ResNet18, a convolutional neural network initialized with ImageNet pretrained weights.

The model was fine-tuned using a dataset organized into five classes:

1. Bacterial Pneumonia
2. Corona Virus Disease
3. Normal
4. Tuberculosis
5. Viral Pneumonia

### Training configuration

| Parameter | Value |
|---|---|
| Architecture | ResNet18 |
| Initialization | ImageNet pretrained weights |
| Optimizer | Adam |
| Learning rate | 0.0001 |
| Epoch limit | 8 |
| Input image size | 160 × 160 |
| Loss | Weighted cross-entropy |
| Execution device | CPU |

Early stopping selected the best checkpoint based on validation loss.

### Initial experimental result

The first training run reported **88.25% test accuracy** on 2,025 test images.

This is an initial experimental result, not evidence of clinical performance. The dataset combines images from multiple sources, and potential duplicate-image leakage, label inconsistencies, source bias, and patient-level separation have not been fully ruled out. Further data auditing and independent evaluation are needed.

## 🛠️ Tech Stack

- **Language:** Python
- **Deep learning:** PyTorch, Torchvision
- **Computer vision:** Pillow, NumPy
- **Model evaluation:** scikit-learn
- **Backend:** FastAPI, Uvicorn
- **Frontend:** HTML, CSS, JavaScript
- **Development:** Visual Studio Code, Git, GitHub

## 📁 Project Structure

```text
VisionCare-Analytics/
├── backend/
│   ├── app/
│   │   └── main.py
│   ├── tests/
│   └── requirements.txt
├── frontend/
│   └── index.html
├── scripts/
│   ├── prepare_data.py
│   └── train.py
├── data/
├── models/
│   └── checkpoint.pt
├── artifacts/
│   └── eval.json
├── README.md
├── Dockerfile
└── docker-compose.yml
```

The `data/`, `models/`, and `artifacts/` directories may be empty or omitted from the repository depending on what you choose to publish. Do not commit large datasets or trained model files without checking repository size limits, dataset licensing, and redistribution permissions.

## 🚀 Getting Started

### Prerequisites

- Python 3.11
- Git
- Internet access for the initial download of pretrained model weights, unless they are already cached
- Sufficient disk space and memory for PyTorch and image processing

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/VisionCare-Analytics.git
cd VisionCare-Analytics
```

Replace `YOUR_USERNAME` with your GitHub username.

### 2. Create a virtual environment

**Windows PowerShell:**

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```powershell
python -m pip install --upgrade pip
python -m pip install --extra-index-url https://download.pytorch.org/whl/cpu -r backend/requirements.txt
```

### 4. Prepare your dataset

Download a properly licensed dataset and extract it locally. The source directory should contain `train`, `val`, and `test` folders, each containing one subfolder per class.

import kagglehub

# Download latest version
path = kagglehub.dataset_download("omkarmanohardalvi/lungs-disease-dataset-4-types")

print("Path to dataset files:", path)

Example:

```text
Lung Disease Dataset/
├── train/
│   ├── Bacterial Pneumonia/
│   ├── Corona Virus Disease/
│   ├── Normal/
│   ├── Tuberculosis/
│   └── Viral Pneumonia/
├── val/
│   └── ...
└── test/
    └── ...
```

### 5. Train the model

To train using the provided training script and your prepared dataset:

```powershell
python scripts/train.py --data "PATH_TO_DATASET" --epochs 8 --pretrained --lr 1e-4 --size 160
```

Replace `PATH_TO_DATASET` with the actual path to your dataset.

The training script saves the checkpoint to `models/checkpoint.pt` and evaluation metrics to `artifacts/eval.json`. The model checkpoint must be available at the expected path for the backend to load it.

### 6. Start the application

From the project root, run:

```powershell
python -m uvicorn app.main:app --app-dir backend --reload
```

Open the dashboard:

- Application: http://127.0.0.1:8000
- API documentation: http://127.0.0.1:8000/docs

Keep the terminal running while using the application. Stop the server with `Ctrl + C`.

## 📊 Evaluation

The training pipeline records:

- Training and validation loss by epoch
- Test accuracy
- Per-class precision, recall, and F1-score
- Classification report and confusion matrix

Evaluation artifacts are saved in `artifacts/eval.json`.

For meaningful evaluation, check for near-duplicate images across splits, inconsistent labels, class imbalance, and source-specific artifacts. A robust assessment should use patient-level separation where patient identifiers are available and independent data from a different source.

## 🔒 Data, Privacy, and Responsible Use

- Use datasets only in accordance with their licences and usage restrictions.
- Do not upload identifiable patient information or confidential medical images to a public repository.
- Avoid committing datasets, secrets, credentials, local environments, and large generated files.
- Treat Grad-CAM maps as exploratory visualizations rather than proof of model reasoning.
- Never use model output as a substitute for professional medical evaluation.

## 🔮 Future Improvements

- Add automated bulk evaluation and downloadable prediction reports.
- Improve duplicate detection and dataset quality auditing.
- Introduce patient-level dataset splitting where metadata permits.
- Evaluate on an independent external dataset.
- Add calibration and uncertainty reporting.
- Improve test coverage, deployment documentation, and reproducibility.
- Explore additional architectures and compare them under a consistent evaluation protocol.

## 👩‍💻 Author

**YOUR NAME**

GitHub: https://github.com/YOUR_USERNAME

## 📄 License

Add a project licence, such as the MIT License, only after confirming that you have the rights to publish your own code under that licence. The dataset and pretrained weights may have separate terms.

---

*Built as an educational AI/ML and computer-vision project.*
