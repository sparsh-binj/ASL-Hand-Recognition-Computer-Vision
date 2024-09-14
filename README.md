# ASL Alphabet Detection and Identification using YOLOv8
![The word 'HELLO' in ASL](https://github.com/sparsh-binj/ASL-recognition-dataset/blob/main/img/hello.jpg?raw=true)

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

#### Model 1

[![Video](https://img.youtube.com/vi/mSuA84eaCWk/maxresdefault.jpg)](https://www.youtube.com/watch?v=)

We decided to train YOLOv8 on an existing dataset using a 70-20-10 split. The results were quite underwhelming. It didn’t recognise most signs correctly and for the ones it was correct, the confidence was around 65%. This is not surprising since the dataset includes signs from the signer’s perspective and not from the viewer’s perspective. Further, all of the pictures are of David Lee and his hand in the same environment making the model highly biased.

The limitations of the previous dataset was that it did not include different signers, some of the training data was either incorrectly signed or the pictures were from the signer’s perspective, and the same background was used for all pictures.

#### Model 2

We decided then to look for youtube videos that we could then break it down to a series of frames (using 2fps) and then annotate those to create our dataset. The idea behind this was to create a dataset that not only had correct signs from the viewer’s perspective but also a variety of signers with varying backgrounds which would make our model more accurate and less biased.

When we trained this model, we also included augmentations for horizontal flip (to include left handed signers), rotation and shearing by +- 5 degrees to make the prediction invariant to minute changes in the hand position.

[![Video](https://img.youtube.com/vi/kXcpHrirTx8/maxresdefault.jpg)](https://www.youtube.com/watch?v=kXcpHrirTx8) 

This new model gave superior results to the previous model but it has its limitations as well (as can be seen in the video). The model does very well in identifying signs if they’re placed a bit further away from the camera than if they’re placed closer. This result made sense when we looked at our dataset again and most of the dataset were pictures that resembled the setup that Duncan had with the webcam, i.e. located a bit further away from the webcam. Hence, the signs made by Duncan were more accurately identified than the signs made by Sparsh who was located closer to the webcam. Another limitation of this model was that we could not find very clear and sharp images for many of the signs since a lot of the frames we extracted from the videos either included much of the signer’s face within the ROI box or their signs were not properly angled towards the camera.

#### Model 3 (Best)

We created our own dataset was to generate pictures that had the following features:

- Correct signs and from the perspective of the viewer.
- 7 different signers in varying lighting and background settings.
- Varying camera angles with respect to the signer.
- A mix of pictures that are close to the camera as well as further away.

[![Video](https://img.youtube.com/vi/cV5y8b68DJ8/maxresdefault.jpg)](https://www.youtube.com/watch?v=cV5y8b68DJ8) 

The idea behind this was to create a dataset that more closely resembles the environment in which an ASL recognition model might be deployed. We used our dataset to train the YOLOv8 model and, as you can see from the video, it does well for signs that are at a varying distance from the camera and is pretty accurate across all categories. There are a few that don’t quite work well like the letters “J” and “N” but that can be improved.


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

