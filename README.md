# Performance Enhancement Technique for Traffic Sign Recognition using Combining Label Smoothing and Focal Loss


## Table of Contents
1. [Research Background](#research-background)  
2. [Research Methods](#research-methods)  
3. [Experiments](#experiments)  
4. [Conclusion](#conclusion)  
5. [Dataset](#dataset)  

---

## Research Background
With the rapid development and commercialization of autonomous driving technology, the importance of accurate road environment perception is steadily increasing.

**Traffic Sign Recognition (TSR)** is a key perception module in both autonomous driving systems and ADAS, providing real-time traffic sign information to drivers or generating vehicle control signals.

In real-world environments, recognition accuracy often decreases under adverse conditions such as **nighttime, rain, and fog**.

Recognizing the necessity of developing a robust object detection model that performs reliably under diverse environmental conditions, this study aims to **enhance the performance of a YOLOv8-based traffic sign recognition model**.

---

## Research Methods
*You can fill this section with details on the model architecture, loss functions, image augmentation, training/validation/test split, etc.*

---

## Experiments
*This section can describe experimental setup, performance metrics (Precision, Recall, mAP@50, mAP@50-95), hyperparameter tuning, and comparisons of different loss functions.*

---

## Conclusion
*Summarize the key findings here. For example:*
- The proposed combined loss function improves performance under challenging conditions.
- The YOLOv8-based model shows high accuracy and robustness across various weather and lighting conditions.
- Future work includes implementing real-time detection and lightweight models for practical deployment.

---

## Dataset
- **Source**: Publicly available traffic sign dataset from Kaggle  
- **Total images**: 877  
- **Number of classes**: 4  
  - Speed Limit: 783  
  - Traffic Light: 170  
  - Crosswalk: 200  
  - Stop: 91  

*Note: The dataset exhibits class imbalance, which was considered during model training and evaluation.*
