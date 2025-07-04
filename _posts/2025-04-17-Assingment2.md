---
title: "Multiclass Image & Fingerprint Recognition with fastai"
date: 2025-04-17
author: Jiyeon Beack
tags: [fastai, image classification, t-SNE, confusion matrix, fingerprint recognition]
permalink: /blog/assignment2-fingerprint/
---

> In this post, I document my approach to solving an assignment on fingerprint recognition and multiclass classification using the `fastai` library, `DuckDuckGo` image search, and `t-SNE` visualization.

---

## Problem 1: Fingerprint Recognition

### Task

I was provided with a Jupyter notebook from [https://github.com/lovellbrian/fingerprint](https://github.com/lovellbrian/fingerprint) and asked to:

- Build a GUI to **enroll fingerprints** and associate them with a name
- Save fingerprint **templates to file**
- **Compare new fingerprints** to those stored in a gallery
- **Evaluate matching performance** and visualize ROC curves

### GUI Preview (Jupyter widgets)

```python
VBox([
  widgets.Label('Enroll Fingerprint and Compare to DB1_B'),
  name_input, btn_upload, btn_run, img_out, lbl_result
])
```

![gui screenshot](https://jiyeonbeackuq.github.io/assets/img/fingerprint.png)

### Matching Logic!

```python
def match_against_db1b(des):
    db_dir = Path("/workspaces/fingerprint/DB1_B")
    template_files = sorted(db_dir.glob("*.tif"))
    results = []

    for file in template_files:
        db_img = Image.open(file).convert("L")
        db_pre = preprocess_fingerprint(np.array(db_img))
        _, db_des = extractMinutiaeFeatures(db_pre)
        score = match_score(des, db_des)
        results.append((file.name, score, file))

    return sorted(results, key=lambda x: x[1], reverse=True)
```

### ROC Curve

I measured genuine vs impostor pairs and computed the ROC curve:

![roc curve](https://jiyeonbeackuq.github.io/assets/img/fingerprint_roc.png)

```python
fpr, tpr, thresholds = roc_curve(y_true, y_scores)
plt.plot(fpr, tpr, label=f"AUC = {auc(fpr, tpr):.2f}")
```

I also estimated:

> **FPR at FNR = 1%** → `FPR = 0.0036`

---

## Problem 2: Multiclass Image Classification

### Target Classes

I collected image data for five classes:

```python
classes = ['airplane', 'automobile', 'bird', 'cat', 'dog']
```

Each class was populated using DuckDuckGo web scraping and `download_images()` from `fastai.vision.utils`.

### Model Pipeline

- `resnet18` base model
- `CrossEntropyLossFlat` for multiclass classification
- Data augmentation with `Resize(224)`

```python
dls = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    get_y=parent_label,
    splitter=RandomSplitter(0.2),
    item_tfms=Resize(224)
).dataloaders(path, bs=32)
```

### ✅ Loss Function Rationale

I used **CrossEntropyLossFlat**, which is well-suited for multiclass tasks where each image has exactly one correct label. It combines softmax activation with negative log-likelihood, accounting for mutual exclusivity between classes.

> Reference: [PyTorch docs](https://pytorch.org/docs/stable/generated/torch.nn.CrossEntropyLoss.html), [fastai loss functions](https://docs.fast.ai/losses.html)

---

## Evaluation Results

### Confusion Matrix

![confusion matrix](https://jiyeonbeackuq.github.io/assets/img/confusion-matrix.png)

### Classification Report

| Class       | Precision | Recall | F1-Score | Support |
|-------------|-----------|--------|----------|---------|
| Airplane    | 0.97      | 1.00   | 0.98     | 31      |
| Automobile  | 1.00      | 0.95   | 0.97     | 37      |
| Bird        | 0.97      | 0.97   | 0.97     | 38      |
| Cat         | 0.97      | 1.00   | 0.99     | 39      |
| Dog         | 1.00      | 1.00   | 1.00     | 35      |

**Accuracy: 0.98**

### t-SNE Visualization

```python
from sklearn.manifold import TSNE
tsne = TSNE(n_components=2).fit_transform(acts.cpu().numpy())
```

![t-sne](https://jiyeonbeackuq.github.io/assets/img/t-SNE.png)

The t-SNE plot shows clearly separated clusters, confirming that the model learned well-discriminated features for each class.

---

## Real-world Test with My Dog

I used an iPhone photo of my dog, converted from `.HEIC` to `.JPG`, and predicted it using:

```python
img = PILImage.create("mydog.jpg")
pred, idx, probs = learn.predict(img)
print(f"Prediction: {pred}, Probability: {probs[idx]:.2f}")
```