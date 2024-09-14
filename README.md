# ASL Alphabet Detection and Identification using YOLOv8

## Overview

This project aims to detect and identify the 26 letters of the ASL (American Sign Language) alphabet in real-time using a video or webcam feed. While some existing models have attempted to classify ASL letters, many exhibit bias or poor performance. To address this, we created a new dataset to validate and test existing models and trained the YOLOv8 model for real-time ASL alphabet detection.

## Existing Works

We explored an existing publication that used the YOLOv5 model for ASL letter detection. However, the dataset used in this publication appeared biased, as the training and testing data were taken in the same setting and angle, using the same hand. This resulted in a model that performed poorly when tested on different data.

We replicated this model and tested it using the YOLOv8 model. However, the results were underwhelming, with poor recognition of most signs and low confidence scores (~65%). This was due to the dataset containing signs from the signer’s perspective, using the same signer in the same environment. As a result, the model likely developed a bias.

## Methods

### Dataset Creation
To overcome the limitations of existing datasets, we created our own dataset by extracting frames from YouTube videos at 2fps. These frames were then annotated to ensure they contained correct signs from the viewer’s perspective. The dataset includes:
- 7 different signers with varying lighting and backgrounds.
- A variety of camera angles.
- A mix of images where the signers are both close to and far from the camera.

We used augmentations such as horizontal flipping (to account for left-handed signers), rotation, and shearing to ensure the model is invariant to small changes in hand position.

### Training the YOLOv8 Model
We used this dataset to train the YOLOv8 model. The model performed well at detecting signs from varying distances and across all categories. Some letters, like "M" and "N," showed weaker performance, but overall the results were superior to the previous model trained on the older dataset.

## Results

Once the model is trained, you can monitor its performance by testing on different videos or webcam input. Check for areas where the model may not perform well, such as signs made closer to the camera or letters like "M" and "N" that historically have lower detection accuracy. The model performs well in identifying signs, especially when the signer is further from the camera. However, it struggles with signs made by signers closer to the camera. This discrepancy likely results from the dataset containing more images of signers farther away, making the model biased toward that setup. Additionally, some extracted frames included too much of the signer’s face within the ROI box or signs that were unclear, further affecting performance.

## Limitations

- **Dataset Bias**: The dataset was biased toward signers at a distance from the camera, making the model less effective for signers closer to the camera.
- **Quality of Frames**: Some frames extracted from videos were unclear or included too much of the signer’s face, affecting model accuracy.
- **Sign Detection Performance**: Some signs, such as "M" and "N," were not accurately identified, possibly due to insufficient variation in the dataset.

## Future Works

### Word-Level Detection with Motion
Currently, the model only detects static signs. Most ASL signs include motion, so extending the model to word-level detection (WL-ASL) would require a more complex architecture, such as an LSTM. However, similar dataset creation techniques could be applied for this purpose.

### Dataset Diversity
The dataset can be further improved by including more signers with different skin tones, ages, and backgrounds. The current dataset, containing 7 signers, is not representative of the diversity in the ASL community. Including more varied backgrounds, such as different types of furniture and wall colors, would make the model more robust to arbitrary settings.

## Files

- `DetectionModel.ipynb`: Jupyter notebook for training the YOLOv8 model on the ASL dataset.
- `train.sh`: Shell script for setting up and training the model.
- `WebcamVideoDataset.ipynb`: Jupyter notebook for creating datasets from webcam videos.
- `WebcamTest.ipynb`: Jupyter notebook for testing the model with webcam input.
- `YoutubeVideoDataset.ipynb`: Jupyter notebook for extracting frames from YouTube videos to build the dataset.

## Contributing
We welcome contributions to further improve the model or dataset. Some areas for improvement include:

- **Expanding the Dataset**: Contribute additional videos or images with diverse backgrounds, lighting conditions, and signers to reduce bias in the model.
- **Improving Sign Detection**: Explore other neural network architectures, such as LSTM or Transformers, to improve detection for dynamic signs.
- **Testing in Various Environments**: Test the model in different real-world environments to assess its robustness and provide feedback.

If you're interested in contributing, please fork the repository and submit a pull request.

## License
This project is licensed under the MIT License. See the `LICENSE` file for more details.

