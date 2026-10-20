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
