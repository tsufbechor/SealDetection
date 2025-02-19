# Motivation
A seal is a small, tamper-evident device placed on shipping containers
Its primary purpose is to prevent unauthorized access or indicate if any tampering has occurred.

Additionaly, accurate detection of open vs. closed sockets is crucial for preventing mechanical or electrical hazards, ensuring regulatory compliance, and maintaining operational efficiency.

# Old Approach
Traditional Approach: Combined Detection and Classification​

The earlier workflow relied heavily on a single-stage object detection model. This model took input images and performed both tasks—locating the seal/socket in the image (localization) and identifying the type of seal/socket (classification)—simultaneously. While convenient, this "all-in-one" approach had some limitations:​

## 1.Mixed Objectives:​

By trying to solve localization and classification at the same time, the model struggled to excel at both tasks. ​

This led to a higher miss rate- failure to detect certain seals\misclassification​

## 2. Insufficient Context for Classification:​

In the old approach, the bounding boxes generated by the detection model were often tightly cropped around the seal/socket. While this minimized background noise, it provided very limited context for the classification step, making it difficult for the model to distinguish between very similar classes or conditions.​

## 3. Downsizing of images
In the old approach, 1920X1080 images were fed into a SSD based object detector and thus downsized in quality, this led to errors in distinguishing between the seal/socket objects which are small and suffer from the downsizing

# New Approach

## ROI Detection Model

First, a specified localization model is applied to the input image to identify potential regions of interest (ROIs) where seals or cable seals might be present. This model focuses purely on accurate bounding box proposals, without being tasked with the final classification decision.

## Classification Model

Once the localization model has identified candidate regions, each region is cropped out from the original image.
These cropped candidates are then passed through a separate classification model that is dedicated solely to identifying the type of seal. ​

Cropping the ROI from the original image ensures we dont lose any pixels when trying to classify between them.

Example of input to Classification model:![image](https://github.com/user-attachments/assets/f53fa156-b820-4c25-9d60-ce40031b92d6)

Outputs a label (e.g., Seal, CableSeal, OpenSocket, ClosedSocket, or No-Object).
Filters out the false positives from the ROI detection stage.


![image](https://github.com/user-attachments/assets/ae7294af-ce44-4f3c-8cd0-f83f46bdca00)

![image](https://github.com/user-attachments/assets/59e5da2a-3dac-44a4-b8c9-3e096a8167cf)

# Metrics:​

Classification Model- 98% accuracy, Inference Time 30ms​

ROI Detection Model- 95% of candidates correctly identified. Inference Time 30-40 ms​

ROI Detection model optimized to not miss any Candidate(High Recall)

Previous system less than 90% accuracy so this is big improvement.

# Integration
After I finished the training the models, converted them to OpenVino format and loaded them in C++. Implemented the whole pipeline in C++. 

Accuracy has risen and the inference time of the whole pipeline in C++ is around 100-150ms. 

New Seal system is running in production in multiple ports Worldwide
