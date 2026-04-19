# Plant Disease Detection — Tomato Leaf Classification

This is our Machine Learning Lab project (CSE 4554) where we compared four CNN architectures for detecting tomato leaf diseases. We trained everything from scratch on a combined dataset from three different Kaggle sources and documented what actually worked, what didn't, and why.

---

## Team Members

| Name | ID | Model |
|---|---|---|
| Israt Risha Ivey | 220042103 | ResNet-18 |
| Tanzia Tahman | 220042129 | MobileNetV2 |
| Nanziba Razin Samita | 220042155 | DenseNet-121 |
| Mustain Billah Taj | 220042166 | EfficientNet-B0 |

Department of CSE, Software Engineering program, IUT.

---

## What we did

We took four well-known CNN architectures and trained all of them from scratch on the same tomato disease dataset, under the exact same hyperparameters, so we could actually compare them fairly. Most papers use pretrained ImageNet weights which muddies the comparison — you can never tell if the accuracy comes from the architecture or from the head start. We didn't do that.

The dataset came from three places: PlantVillage (clean lab images), the New Plant Diseases Dataset (augmented version of PlantVillage), and Plant-Doc (real field photos with messy backgrounds). We merged them, kept only the tomato classes, and ended up with about 19,847 images across 10 disease categories.

---

## Results

| Model | Test Accuracy | F1 Score | Params | Speed |
|---|---|---|---|---|
| ResNet-18 | 95.43% | 0.9534 | 11.7M | 1247 img/s |
| MobileNetV2 | 96.87% | 0.9677 | 3.4M | 1847 img/s |
| DenseNet-121 | 97.64% | 0.9756 | 8.0M | 412 img/s |
| EfficientNet-B0 | **98.21%** | **0.9817** | 5.3M | 740 img/s |

EfficientNet-B0 came out on top in accuracy. MobileNetV2 is clearly the pick if you care about speed or want to run this on a phone or low-power device — nearly 1850 images per second at under 3.5 million parameters is pretty hard to argue with.

---

## Dataset

We pulled from three Kaggle datasets and filtered to tomato only:

- [PlantVillage](https://www.kaggle.com/datasets/tushar5harma/plant-village-dataset-updated) — lab condition images, uniform lighting, plain backgrounds, 10 tomato classes
- [New Plant Diseases Dataset](https://www.kaggle.com/datasets/vipoooool/new-plant-diseases-dataset) — offline-augmented version of PlantVillage, adds variety
- [Plant-Doc](https://www.kaggle.com/datasets/abdulhasibuddin/plant-doc-dataset) — real field images, noisy, variable backgrounds, much harder

The class distribution is pretty uneven. Yellow Leaf Curl Virus makes up about 27% of the data while Tomato Mosaic Virus is barely 2%. That's why we report macro F1 rather than just accuracy — accuracy alone would hide how badly a model handles the small classes.

Split: 70% train / 15% val / 15% test, stratified so every class appears in all three splits proportionally.

---

## Preprocessing

Everything gets resized to 224x224. We normalized with mean `[0.485, 0.456, 0.406]` and std `[0.229, 0.224, 0.225]` — standard ImageNet stats. Even training from scratch, this turned out to stabilize early training noticeably compared to our no-normalization runs.

**Augmentation (training only):**
- Horizontal and vertical flip (p=0.5)
- Random rotation up to ±30°
- Color jitter on brightness and contrast (±0.2)
- Random resized crop (scale 0.8 to 1.0)

Val and test images never get augmented, just resized and normalized.

We deliberately skipped cutout, mixup, and grayscale. Cutout can erase the actual lesion spot which is the only useful thing in the image. Mixup blends two leaves together which makes no biological sense. And color matters here — the yellow halo around bacterial spot is a color feature, converting to grayscale throws that away.

---

## Training setup

Same for all four models, no exceptions:

```
Optimizer:     Adam (lr=1e-3, β1=0.9, β2=0.999)
Scheduler:     ReduceLROnPlateau (factor=0.5, patience=8)
Loss:          CrossEntropyLoss
Batch size:    32
Epochs:        30
Init:          Kaiming uniform, from scratch
```

LR dropped to 5e-4 around epoch 19 and 2.5e-4 around epoch 28 for all models when val loss flattened out.

---

## Repo structure

```
Plant-Disease-Detection/
│
├── datasets/
│   └── prepare_dataset.py       # merges, filters, and splits the three datasets
│
├── models/
│   ├── resnet18.py
│   ├── mobilenetv2.py
│   ├── densenet121.py
│   └── efficientnetb0.py
│
├── training/
│   ├── train.py
│   └── config.py
│
├── evaluation/
│   ├── evaluate.py
│   └── inference_speed.py
│
├── notebooks/
│   ├── ResNet18_Training.ipynb
│   ├── MobileNetV2_Training.ipynb
│   ├── DenseNet121_Training.ipynb
│   └── EfficientNetB0_Training.ipynb
│
├── results/
│   ├── training_curves/
│   ├── confusion_matrices/
│   └── summary_metrics.csv
│
├── requirements.txt
└── README.md
```

---

## Commands

```bash
git clone https://github.com/IRIvey/Plant-Disease-Detection.git
cd Plant-Disease-Detection
pip install -r requirements.txt
```

Download the three datasets from Kaggle, put them in `datasets/raw/`, then:

```bash
python datasets/prepare_dataset.py
```

To train:
```bash
python training/train.py --model efficientnetb0
```

Swap `efficientnetb0` with `resnet18`, `mobilenetv2`, or `densenet121`. To evaluate:
```bash
python evaluation/evaluate.py --model efficientnetb0 --checkpoint results/best_efficientnetb0.pth
```

The notebooks are also fully runnable on Colab or Kaggle if you don't want to set anything up locally.

---

## Requirements

```
torch>=2.0.1
torchvision>=0.15
numpy
pandas
scikit-learn
matplotlib
seaborn
tqdm
Pillow
```

---

## Cost

Everything ran on free-tier Colab and Kaggle with T4 GPUs. Total cost was essentially nothing. Longest run was DenseNet-121 at around 1h40m, shortest was ResNet-18 at about 45 minutes.

---

## A few things worth knowing before you use this

No pretrained weights anywhere in this project. Everything is Kaiming uniform init, trained end to end. This is intentional — we wanted architectural differences to be the actual variable, not whatever ImageNet representations came along for the ride.

