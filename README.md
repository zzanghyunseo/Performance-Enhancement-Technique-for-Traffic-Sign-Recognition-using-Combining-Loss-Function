# Performance Enhancement Technique for Traffic Sign Recognition using Combining Label Smoothing and Focal Loss

---

## Research Background
With the rapid development and commercialization of autonomous driving technology, the importance of accurate road environment perception is steadily increasing.

**Traffic Sign Recognition (TSR)** is a key perception module in both autonomous driving systems and ADAS, providing real-time traffic sign information to drivers or generating vehicle control signals.

In real-world environments, recognition accuracy often decreases under adverse conditions such as **nighttime, rain, and fog**.

Recognizing the necessity of developing a robust object detection model that performs reliably under diverse environmental conditions, this study aims to **enhance the performance of a YOLOv8-based traffic sign recognition model**.

---

## Dataset
- Dataset: Publicly available traffic sign dataset from Kaggle  
- Total images: 877  
- Number of classes: 4  
  - Speed Limit: 783  
  - Traffic Light: 170  
  - Crosswalk: 200  
  - Stop: 91
  <img width="346" height="234" alt="image" src="https://github.com/user-attachments/assets/fc5caf95-4e25-4831-92ba-29b99318d944" />
&rarr; Class imbalance

- Image augmentation using Albumentations library
	- Brightness, contrast, blur, fog, rain, etc.
- Dataset split : Randomly shuffled &rarr; Train 80% / Validation 10% / Test 10%
- **Train : Original + Night/Adverse weather augmented images**
  &rarr; to account for diverse scenarios
- **Validation / Test : Original images**
  &rarr; to fairly evaluate the generalization performance of the model
  
  <img width="818" height="160" alt="image" src="https://github.com/user-attachments/assets/491f7d8b-9676-45d5-9f12-127148788b91" />

---

## YOLOv8 (You Only Look Once)
- Backbone
 Multi-scale feature extraction using advanced CNN

- Neck
 Fusion of features at different scales
 Improves detection performance for small to large objects

- Head
 **Anchor-Free structure** : Direct prediction without Anchor Boxes &rarr; **simplifying the architecture and enhancing flexibility**

---

## Binary Cross Entropy (BCE)
ℒ_𝐵𝐶𝐸=−[𝑦∙log⁡(𝑦 ̂ )+(1−𝑦)∙𝑙𝑜𝑔(1−𝑦 ̂ )]
- 𝑦∈{0,1}   : Ground truth (actual label)
- 𝑦 ̂∈{0,1} : Sigmoid output (predicted probability)

- Representative loss function used in binary classification
- Default loss function in YOLOv8
- Calculates the difference between predicted probability and true label
- **Tends to make the model overconfident in the correct class**
  &rarr; May cause performance degradation when dealing with class imbalance or hard-to-classify samples.

---



*Note: The dataset exhibits class imbalance, which was considered during model training and evaluation.*
