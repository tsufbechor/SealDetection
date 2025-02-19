# Motivation
A seal is a small, tamper-evident device placed on shipping containers
Its primary purpose is to prevent unauthorized access or indicate if any tampering has occurred.

Traditionally, a single object detection model was used to find seals in images. However, I found that expanding context around the region of interest leads to better downstream classification.

Consequently, the pipeline was split into two stages:

## ROI Detection Model

Detects regions likely to contain a seal, cable, or socket.
Prioritizes recall over precision 
Produces cropped regions larger than typical bounding boxes to retain more context.

## Classification Model

Takes each cropped region as input.
Outputs a label (e.g., Seal, CableSeal, OpenSocket, ClosedSocket, or No-Object).
Filters out the false positives from the ROI detection stage.

![image](https://github.com/user-attachments/assets/59e5da2a-3dac-44a4-b8c9-3e096a8167cf)
