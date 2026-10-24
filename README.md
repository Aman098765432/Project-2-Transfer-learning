## Project 2, Week 5: Scene Classification with Transfer Learning

- **Dataset:** Intel Image Classification, 6 classes, 14,034 original training images / 3,000 testing images. The training data was further split into 11,929 training and 2,105 validation images.
- **Model:** ResNet-18 pretrained on ImageNet.
- **Feature extraction (3,078 trainable parameters):** Best validation accuracy = **90.7%**.
- **Fine-tuning layer4 (8,396,806 trainable parameters):** Best validation accuracy = **93.9%**.
- **Test accuracy (used once):** **90.4%**; weakest class by F1-score: **Mountain (0.830)**. The confusion matrix is needed to identify which class Mountain was most frequently confused with.
- **Most instructive error:** A Buildings image containing a road and surrounding hills was incorrectly classified as Mountain with a confidence score of 0.63, showing how background features can confuse the model.
- **Next — My Own Domain in Week 6:** **Plant Leaf Disease Classification** — I plan to apply transfer learning using ResNet-18 to classify healthy and diseased plant leaves, exploring how pretrained CNN models can be adapted for smart agriculture.

## Dataset

**Dataset Name:** PlantVillage Dataset

Author:Aman Riaz

**Dataset Link:** [PlantVillage Dataset](https://www.kaggle.com/datasets/abdallahalidev/plantvillage-dataset)

This dataset contains healthy and diseased plant leaf images used to train a ResNet-18 model for plant disease classification using transfer learning.


### Key Learning
Fine-tuning improved validation accuracy by **3.2 percentage points** compared with feature extraction. This experiment demonstrated how pretrained CNN models can be adapted to new image classification tasks using limited training time and computational resources.

## Week 6: Plant Leaf Disease Detection (healthy vs diseased) with ResNet-18

**Who benefits:** farmers and agricultural extension workers screening pepper, potato and tomato
leaves. **Why it matters:** a missed infection (e.g. late blight) can spread through a whole field;
early detection allows targeted treatment instead of blanket spraying.

**Dataset:** PlantVillage, Kaggle `emmarex/plantdisease`
(https://www.kaggle.com/datasets/emmarex/plantdisease), license as stated on the Kaggle page;
20,638 images (20,624 after removing duplicates), 15 folders merged into 2 classes; labels from the
PlantVillage project, assigned per folder. Lab photos, RGB 256×256, plain cloth background.

**Data audit:** imbalance ratio 5.4 (17,417 diseased / 3,221 healthy; only 152 healthy potato);
exact duplicates 28 files (14 pairs, extra copies removed); test images with a near-duplicate in
train: 0 (closest distance 0.20); split by leaf id (regex on the file code, removing shot number and
day), 1.02 images per group, 0 shared groups between train/val and train/test.

**Imbalance handling:** weighted cross-entropy, weights diseased 0.593, healthy 3.192.

**Results (test, used once):** macro-F1 0.993; recall diseased 0.997, healthy 0.995;
binary: sensitivity 0.992 and specificity 0.995 at threshold 0.893 (set on validation for
recall ≥ 0.95); AUC 1.000. PPV 0.999 at test prevalence (84.7%), 0.957 at a realistic 10% prevalence.

**Grad-CAM:** on correct predictions the model looks at lesions and veins in all three crops; in
wrong predictions the heat often lies on the background (clearest case: a tomato late-blight leaf
predicted healthy with attention on the empty background). Shortcut risk: background, framing and
lighting from lab capture sessions — strongly suspected, not yet confirmed (proposed test:
background-only images).

**Limitations:** lab images only (single detached leaves, plain background, controlled light);
3 crops; no plant id for most images, so the split is mostly per image; very few healthy potato
images; ImageNet weights loaded from a local copy because internet was unavailable; near-perfect
scores likely overestimate field performance.
**Not for:** real spraying or treatment decisions without validation on field photos.

### Responsibility questions

**1. Who could be harmed and how?**
Farmers: a false "healthy" lets disease spread and destroys yield and income; a false "diseased"
leads to unnecessary pesticide cost and environmental harm. Farmers growing crops or varieties not in
the dataset may get confident but wrong answers.

**2. Where was the model not validated?**
Real field photos (soil, other leaves, sunlight, shadows), phone cameras, crops other than
pepper/potato/tomato, diseases not in the 15 folders, and leaves with several diseases, nutrient
deficiency or insect damage.

**3. What is the human role?**
The model is only a screening aid. An agronomist or trained extension worker must confirm the
diagnosis before any treatment decision.

**4. What evidence would change my conclusions?**
A large drop in performance on field images, or the model scoring well above chance on
background-only images (confirming the shortcut), would show that 0.993 does not reflect real-world
ability. Strong performance on an independent field dataset would increase confidence.
