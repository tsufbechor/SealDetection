# Motivation
A seal is a small, tamper-evident device placed on shipping containers
Its primary purpose is to prevent unauthorized access or indicate if any tampering has occurred.

# Old Approach
Traditional Approach: Combined Detection and Classification​

The earlier workflow relied heavily on a single-stage object detection model. This model took input images and performed both tasks—locating the seal in the image (localization) and identifying the type of seal (classification)—simultaneously. While convenient, this "all-in-one" approach had some limitations:​

## 1.Mixed Objectives:​

By trying to solve localization and classification at the same time, the model struggled to excel at both tasks. ​

This led to a higher miss rate- failure to detect certain seals\misclassification​

## 2. Insufficient Context for Classification:​

In the old approach, the bounding boxes generated by the detection model were often tightly cropped around the seal. While this minimized background noise, it provided very limited context for the classification step, making it difficult for the model to distinguish between very similar classes or conditions.​
# New Approach

## ROI Detection Model

Detects regions likely to contain a seal, cable, or socket.
Prioritizes recall over precision.
Produces cropped regions larger than typical bounding boxes to retain more context.

## Classification Model

Takes each cropped region as input.
![image](https://github.com/user-attachments/assets/f53fa156-b820-4c25-9d60-ce40031b92d6)

Outputs a label (e.g., Seal, CableSeal, OpenSocket, ClosedSocket, or No-Object).
Filters out the false positives from the ROI detection stage.


![image](https://github.com/user-attachments/assets/ae7294af-ce44-4f3c-8cd0-f83f46bdca00)

![image](https://github.com/user-attachments/assets/59e5da2a-3dac-44a4-b8c9-3e096a8167cf)

# Metrics:​

Classification Model- 98% accuracy, Inference Time 30ms​

ROI Detection Model- 95% of candidates correctly identified. Inference Time 30-40 ms​

ROI Detection model optimized to not miss any Candidate(High Recall)

Previous system had around less than 90% accuracy so this is big improvement.

# Integration
After I finished the training the models, converted them to OpenVino format and loaded them in C++. Implemented the whole pipeline in C++. 

New Seal system is running in multiple ports Worldwide
