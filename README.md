# DeepCAPTCHA: Attention-Based CAPTCHA Recognition System

## Overview

DeepCAPTCHA is a custom deep learning framework designed for automated CAPTCHA recognition. The model was developed entirely from scratch without using any pretrained weights or transfer learning, complying with competition constraints.

The system combines a CNN-based feature extractor, CBAM attention modules, BiLSTM sequence modeling, and Multi-Head Self Attention to accurately recognize fixed-length CAPTCHA strings from noisy and distorted images.

The project was developed for a CAPTCHA OCR challenge where the primary evaluation metric was Character Error Rate (CER).

---

## Key Features

- Custom architecture trained entirely from scratch
- No pretrained models or transfer learning
- Attention-enhanced feature extraction
- Sequence-aware recognition using BiLSTM
- Multi-Head Self Attention for contextual character modeling
- Optimized for low Character Error Rate (CER)
- Fixed-length CAPTCHA recognition (6 characters)

---

## Dataset Information

### Training Set
- Total Images: 20,000
- Labels Provided: Yes (`train-labels.csv`)
- Image Size: 200 × 100 pixels
- CAPTCHA Length: 6 characters

### Test Set
- Total Images: 5,000
- Labels Provided: No

### Character Set

The dataset contains the following characters:

```text
23456789
ABCDEFGHJKMNPQRSTUVWXYZ
```

Excluded characters:

```text
0, 1, I, L, O
```

These ambiguous characters were intentionally omitted from the dataset.

---

## Data Preprocessing

### Label Cleaning

Two corrupted samples were detected and removed from the training data:

```text
train-2184.png
train-6819.png
```

After cleaning:

```text
Total Valid Training Samples: 19,998
```

### Image Processing

- Grayscale conversion
- Pixel normalization
- Tensor conversion

### Data Augmentation

The following augmentations were applied during training:

- Gaussian Noise
- Motion Blur
- Random Brightness & Contrast
- Grid Distortion
- Perspective Transform
- Affine Transformations

---

## Model Architecture

### Pipeline

```text
Input CAPTCHA Image
        │
        ▼
CNN Backbone
        │
        ▼
CBAM Attention Modules
        │
        ▼
Sequence Encoder
        │
        ▼
BiLSTM (Bidirectional)
        │
        ▼
Multi-Head Self Attention
        │
        ▼
Character Decoder
        │
        ▼
6 Character Predictions
```

---

## Architecture Details

### CNN Backbone

- Convolutional Layers
- Residual Connections
- Batch Normalization
- Max Pooling

### CBAM Attention

The Convolutional Block Attention Module (CBAM) improves feature extraction using:

- Channel Attention
- Spatial Attention

### Sequence Encoder

Transforms CNN feature maps into sequential representations for OCR-style processing.

### BiLSTM

Captures contextual information in both directions:

- Forward sequence modeling
- Backward sequence modeling

### Multi-Head Self Attention

Allows the model to focus on important feature regions and inter-character relationships.

### Character Decoder

Generates predictions for all six character positions simultaneously.

---

## Training Configuration

### Optimizer

```text
AdamW
```

### Learning Rate

```text
3e-4
```

### Weight Decay

```text
1e-4
```

### Scheduler

```text
Cosine Annealing Learning Rate Scheduler
```

### Batch Size

```text
64
```

### Epochs

```text
40
```

### Gradient Clipping

```text
Max Norm = 5.0
```

---

## Evaluation Metric

### Character Error Rate (CER)

The challenge evaluation metric was Character Error Rate.

Formula:

```text
CER = (Substitutions + Insertions + Deletions) / Total Characters
```

Lower values indicate better performance.

---

## Results

### Baseline Model

Architecture:

```text
CNN + Global Pooling + Multi-Head Classification
```

Performance:

```text
Validation CER ≈ 0.2880
```

---

### Final Model

Architecture:

```text
CNN + CBAM + BiLSTM + Multi-Head Self Attention
```

Performance:

```text
Best Validation CER = 0.0007
```

Character Accuracy:

```text
99.93%
```

---

## Project Structure

```text
.
├── train_images/
├── test_images/
├── train-labels.csv
├── notebook.ipynb
├── submission.csv
├── best_crnn_attention.pth
└── README.md
```

---

## Installation

Clone the repository:

```bash
git clone https://github.com/your-username/DeepCAPTCHA.git
cd DeepCAPTCHA
```

Install dependencies:

```bash
pip install torch torchvision
pip install numpy pandas
pip install opencv-python
pip install albumentations
pip install scikit-learn
pip install jiwer
pip install tqdm
```

---

## Running the Project

### Train the Model

Open:

```text
notebook.ipynb
```

Run all cells sequentially.

### Generate Predictions

Load the saved checkpoint:

```python
model_v2.load_state_dict(
    torch.load("best_crnn_attention.pth")
)
```

Run the inference section to generate:

```text
submission.csv
```

---

## Competition Compliance

This project fully complies with competition rules:

- No pretrained weights used
- No transfer learning used
- No external datasets used
- Entirely trained on provided dataset
- Custom architecture built from scratch

---

## Technologies Used

- Python
- PyTorch
- OpenCV
- Albumentations
- NumPy
- Pandas
- Scikit-Learn

---

## Future Improvements

- Ensemble Learning
- Test Time Augmentation (TTA)
- Transformer-Based OCR Decoder
- Lightweight deployment model for real-time inference

---

## Author

**Akul Katta**

DeepCAPTCHA: Attention-Based CAPTCHA Recognition System

Developed as part of a CAPTCHA OCR Challenge focused on minimizing Character Error Rate (CER) using custom deep learning architectures.
