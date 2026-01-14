# Road-Semantic-Segmentation
ASO-LC71 - Computer Vision Project Group 1

The project demonstrates a working semantic road segmentation system that operates in real time using camera input. The results confirms that the trained U-Net based model is capable of identifying drivable roads under both daytime and nighttime conditions, which aligns with the original objective of achieving road perception in varying lighting environments.

Training Files:
- "Model_Training_RoadSegmen.ipynb" (V1)
- "Model_Training_RoadSegmen2.ipynb" (V2)

Testing Files:
- "ModelTesting_RoadSegmentation.ipynb" (Testing with Video File)
- "ModelTesting_Webcam_RoadSegmentation.ipynb" (Testing with Camera) 

Model Folders:
- "Trained_Model - (OriginalPARAM)" [V1]
- "Trained_Model - (NewPARAM)" [V2]


**Dataset**
- BDD100K: A road segmentation dataset sourced from Berkley DeepDrive. (http://bdd-data.berkeley.edu/download.html)
- Dashcam: A dataset consisting of our dashcam footage strolling around Alam Sutera.

**Methodology**

Training Model Pipeline:
1. Extract Dataset: Unzips and sets image_base / label_base for images and masks
2. Loading+Pre-processing: Reads image and masks, resizes to 256x256, scales images and masks to [0,1].
3. Augmentation: Apply horizontal flip and random brightness.
4. Dataset: Builds dataset from sorted file lists, map loaders, shuffles, batches, and prefetches.
5. Model: U-Net style encoder/decoder (3 encoder/decoder blocks, small channel counts 16 → 32 → 64) with final sigmoid output for binary segmentation.
6. Compile & Metric: Uses adam, accuracy, and IoU metric computer on predictions > 0.5
7. Train & Save: Fits training and validation for EPOCHS and saves model to .keras file
8. Visualization: Show sample inputs, ground truth masks, and thresholded predictions

Testing Model Pipeline:
1. Load Model & Setup: Define segmentation metrics (IoU) to load the trained model correctly
2. Capture Camera Frames: Continously captures video frames in real time for processing
3. ROI (Region of Interest): Focuses the model on the road area to reduce unnecessary computations.
4. Preprocessing: ROI resized to 256x256 (match model’s input), Pixel values are normalized to [0 or 1]
5. Visualization: Segmentation mask is resized back to original frame size. The detected road area is overlaid in green occupancy grid on the original frame, displayed in real-time.

**Analysis**

Training Model (V1) keeps data closer to the original imaages and masks.
- Image scaling: Normalizes pixel values
- Image distribuition: Origianl brightness preserved
- Brightness augmentation impact: Strong
- Mask values: Soft non-binary labels
- Label noise control: Low

Training Model (V2) Enforces stricter normalization and cleaner labels, but reduces lighting variation
- Image scaling: Divides pixel values by 255 + Standardization
- Image distribuition: Data centered at Zero with consistent spread
- Brightness augmentation impact: Weak
- Mask values: Strict binary labels
- Label noise control: High

Github Link Code, Documentation & Presentation: https://github.com/dhikawahi12/Road-Semantic-Segmentation

