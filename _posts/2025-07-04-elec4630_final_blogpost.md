# Efficient Deep Learning for Image Classification: Insights from ELEC4630 Final Project

Author: Jiyeon Beack (s4866889)

## 🔍 Overview

This blog post summarizes the experiments and findings from my final project for the ELEC4630 course (Computer Vision and Deep Learning). The task involved building robust image classifiers for paddy leaf disease detection and AI-generated image detection using various deep learning architectures including ResNet, ConvNeXt, EfficientNet, and EfficientViT.

---

## 🧠 ResNet vs ConvNeXt: Classical vs Modern CNNs

**ResNet (He et al., 2015)** introduced residual connections to solve the degradation problem in deep networks. Skip connections allowed gradients to flow more easily during backpropagation.

**ConvNeXt (Liu et al., 2022)** takes inspiration from Transformers, replacing ReLU with GELU, BatchNorm with LayerNorm, and using depthwise convolutions and larger kernels. The result: better performance and modern design.

![ResNet and ConvNeXt comparison](https://raw.githubusercontent.com/yourusername/assets/resnet_vs_convnext.png)

---

## 🌾 Task 1: Paddy Leaf Disease Classification

### 🔧 Setup

- **Training set:** 10,407 images across 10 classes
- **Validation set:** 3,469 images
- **Framework:** FastAI + `resnet26d`, `convnext_small_in22k`
- **Transformations:** `aug_transforms`, `Resize`, `PadMode.Zeros`

### 🧪 Model Comparisons

| Model            | Error Rate | Epoch Time | Final Score |
|------------------|------------|------------|-------------|
| ResNet26d        | 0.05       | ~10 min    | —           |
| ConvNeXt Small   | 0.011      | ~20 min    | 0.018       |
| EfficientViT     | 0.32       | ~11 min    | —           |
| EfficientNetV2 S | 0.40+      | ~11 min    | —           |

### ✅ Best Practices Applied

- **Learning Rate Finding:** Used `learn.lr_find()` to tune the optimal LR.
- **Aggressive Augmentation:** `do_flip`, `flip_vert`, `max_rotate=20`, `max_zoom=1.2`, etc.
- **Test-Time Augmentation (TTA):** Increased robustness by averaging predictions from different augmented versions of input images. [Source](https://nbviewer.org/github/fastai/fastbook/blob/master/07_sizing_and_tta.ipynb#Test-Time-Augmentation)

---

## 🧪 Task 2: Real vs AI-Generated Image Classification

This task addressed the increasing concern of deepfake images. Using the Kaggle dataset ["AI vs Real Image"](https://www.kaggle.com/competitions/hack-rush-deep-fake-detection), I experimented with:

- **ResNet26d** as baseline
- **ConvNeXt** as best performer (0.0123 error rate after TTA)
- **EfficientNetV2** which failed to converge properly (error rate ≈ 0.40)

---

## ⚙️ EfficientViT: Why It Didn't Work Well

Although **EfficientViT (Liu et al., 2023)** is designed for resource-limited devices using *Cascaded Group Attention* and *Sandwich Layout*, the FastAI implementation fell short. Possible reasons:

- Lack of official support
- Inefficient PyTorch wrapper
- Overhead from FastAI API incompatibility

![EfficientViT structure](https://raw.githubusercontent.com/yourusername/assets/efficientvit_layout.png)

---

## 📌 Key Takeaways

- **ConvNeXt** offers top accuracy but requires more time/resources.
- **ResNet** is reliable, fast, and still competitive.
- **EfficientViT** is promising but not production-ready in standard pipelines.
- **Data augmentation and learning rate tuning** have significant impact.
- **Avoid ensembling** unless marginal gains are mission-critical.

---

## 📚 References

- He et al., 2015: [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385)
- Liu et al., 2022: [A ConvNet for the 2020s](https://arxiv.org/abs/2201.03545)
- Liu et al., 2023: [EfficientViT](https://arxiv.org/abs/2305.07027)
- FastBook Chapter 7: [TTA Reference](https://nbviewer.org/github/fastai/fastbook/blob/master/07_sizing_and_tta.ipynb#Test-Time-Augmentation)

---

## 🙏 Acknowledgement

Some expression revisions were supported by [ChatGPT](https://chat.openai.com/), and I ensured that all insights and implementation reflected my personal understanding and experiments.

