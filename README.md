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
<img width="450" height="43" alt="image" src="https://github.com/user-attachments/assets/3e4ba742-afe3-4166-8c03-29ab063b4d4b" />

- 𝑦∈{0,1}   : Ground truth (actual label)
- 𝑦 ̂∈{0,1} : Sigmoid output (predicted probability)

- Representative loss function used in binary classification
- Default loss function in YOLOv8
- Calculates the difference between predicted probability and true label
- **Tends to make the model overconfident in the correct class**
  &rarr; May cause performance degradation when dealing with class imbalance or hard-to-classify samples.

---

## Focal Loss
<img width="307" height="39" alt="image" src="https://github.com/user-attachments/assets/34fa5e81-8037-4913-bcc8-52195fef4cf2" />

- 𝑝_𝑡 : Predicted probability for true class
- 𝛼 : Class weight
- 𝛾 : Focusing parameter → reduces loss for easy samples, focuses on hard samples

- Adjusts the weighting so that the model can focus more on hard samples rather than easy ones
- α assigns larger weight to minority classes 
  $rarr; **Helps alleviate class imbalance**
- γ gives higher loss to misclassified samples

---

## Label Smoothing Cross Entropy Loss
<img width="219" height="57" alt="image" src="https://github.com/user-attachments/assets/b294a327-9c10-4528-8971-3b5b47f8701f" />

- 𝛼∈{0,1} : smoothing factor
- 𝐾 : Number of classes
- 𝑦 ̃_𝑘 : Smoothed label distribution
 <img width="186" height="266" alt="image" src="https://github.com/user-attachments/assets/15bf2b45-a321-4d71-94e1-1770f9207e92" />

- 𝑦 ̃_𝑖  : Smoothed label distribution
- 𝑝_𝑖 : Softmax output (predicted probability)

- **Adds small uncertainty** to true class, preventing overconfidence in the correct label
  **&rarr; Improves generalization and prevent overfitting**
ex ) [0, 0, 1, 0] &rarr; [0.05, 0.05, 0.85, 0.05] 

---

## Performance Metrics
- Precision : The proportion of detected objects that are actually correct
- Recall : The proportion of true objects that are correctly detected by the model
- mAP@50 : Average precision per class when predicted boxes overlap with ground-truth boxes by ≥ 50%
- mAP@50-95 : Average mAP over IoU thresholds from 50% to 95% in 5% increments

---

## Comparison of Loss Function
<img width="701" height="190" alt="image" src="https://github.com/user-attachments/assets/3261f99c-705a-4fd9-bb57-3355a16dda12" />

- BCE showed overall balanced performance, serving as a stable baseline model.
- Focal Loss achieved the highest Recall (0.870), indicating that the model **missed fewer objects** overall.
**&rarr; Suggests relative robustness to hard or rare samples**
- Label Smoothing showed the highest Precision (0.916) but the lowest Recall (0.785), indicating **a tendency for the model to avoid predictions in uncertain cases.**
**&rarr; more conservative, reduces overconfidence, improves generalization**

- No single loss function performs best across all metrics 
**&rarr; Combines the strengths of Label Smoothing and Focal Loss**

---

## Combined Loss : Label Smoothing + Focal Loss
<img width="260" height="266" alt="image" src="https://github.com/user-attachments/assets/7fdb9a25-f93f-43f9-82b0-e62c0e6bfb54" />

- 𝑝_𝑖 : Softmax output (predicted probability)
- 𝑦 ̃_𝑖  : Smoothed label distribution
- 𝛾 : Focusing parameter → reduces loss for easy samples, focuses on hard sample

Combines Label Smoothing **(adds uncertainty to true class)**
Focal Loss **(assigns higher weights to hard samples)**
**&rarr; Prevents overfitting and focuses on hard samples**

### Experiments to find the optimal hyperparameters 
<img width="711" height="270" alt="image" src="https://github.com/user-attachments/assets/666faecf-04cc-4c00-85f7-4cb0b0442b23" />

- When **𝑦 ̃_𝑖=0.05,  𝛾=2.0**, the model showed a slight advantage in Precision and mAP@50-95, and achieved overall high performance. 

### Fine-grained hyperparameter tuning focused on 𝑦 ̃_𝑖 
<img width="709" height="155" alt="image" src="https://github.com/user-attachments/assets/2e25e62b-b6c3-4ae1-bdde-6a1f405e3170" />

- When **𝑦 ̃_𝑖=0.05,  𝛾=2.0**, the model achieved the best performance in Precision, mAP@50, and mAP@50-95, while maintaining a high level of Recall. 
- Confirmed as the most optimal setting for overall performance

### Additional tuning of Confidence & IoU thresholds to improve post-processing performance
<img width="877" height="127" alt="image" src="https://github.com/user-attachments/assets/05d1aba5-af44-47bb-bf61-e9a4f75da34b" />

- When the confidence threshold of 0.25, the model achieved the best performance, recording the highest Precision (0.969), Recall (0.885), mAP@50 (0.939), and mAP@50-95 (0.822). 
- Final hyperparameter settings : **𝑦 ̃_𝑖=0.05,  𝛾=2.0,  conf=0.25**

---

## Conclusion
<img width="394" height="231" alt="image" src="https://github.com/user-attachments/assets/63df1071-a7fe-4528-9717-4b2086bded78" />

- Accurately detects and classifies traffic signs using the test dataset.
- YOLOv8 with combined loss function achieved best performance: **Precision: 0.969, Recall: 0.885, mAP@50: 0.939, mAP@50–95: 0.822**
- Demonstrates **accurate and stable object detection under nighttime and adverse weather.**
- For future work, we plan to extend the model to real-world applications by implementing real-time detection and lightweight architectures. 
