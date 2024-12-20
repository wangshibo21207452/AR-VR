# Facial Emotion Recognition

The aim of this section is to explore facial emotion recognition techniques from a live webcam video stream.

![image](video_app.png)

## Data

The data set used for training is the **Kaggle FER2013** emotion recognition data set

The data consists of 48x48 pixel grayscale images of faces. The faces have been automatically registered so that the face is more or less centered and occupies about the same amount of space in each image. The task is to categorize each face based on the emotion shown in the facial expression in to one of seven categories (0=Angry, 1=Disgust, 2=Fear, 3=Happy, 4=Sad, 5=Surprise, 6=Neutral).

![alt text](Images/Read_Images/bar_plot.png)

## Requirements

```
Python : 3.6.5
Tensorflow : 1.10.1
Keras : 2.2.2
Numpy : 1.15.4
OpenCV : 4.0.0
```

## Files

The different files that can be found in this repo :
- `Images` : All pictures used in the ReadMe, in the notebooks or save from the models
- `Resources` : Some resources that have been used to build the notebooks
- `Notebooks` : Notebooks that have been used to obtain the final model
- `Model`: Contains the pre-trained models for emotion recognition, face detection and facial landmarks
- `Python` : The code to launch the live facial emotion recognition



#### Model

The model we have chosen is an **XCeption** model, since it outperformed the other approaches we developed so far. We tuned the model with :
- Data augmentation
- Early stopping
- Decreasing learning rate on plateau
- L2-Regularization
- Class weight balancing
- And kept the best model

As you might have understood, the aim was to limit overfitting as much as possible in order to obtain a robust model.

- To know more on how we prevented overfitting, check this article : https://maelfabien.github.io/deeplearning/regu/
- To know more on the **XCeption** model, check this article : https://maelfabien.github.io/deeplearning/xception/

![image](Images/Read_Images/model_fit.png)

The XCeption architecture is based on DepthWise Separable convolutions that allow to train much fewer parameters, and therefore reduce training time on Colab's GPUs to less than 90 minutes.

![image](Images/Read_Images/video_pipeline2.png)

When it comes to applying CNNs in real life application, being able to explain the results is a great challenge. We can indeed  plot class activation maps, which display the pixels that have been activated by the last convolution layer. We notice how the pixels are being activated differently depending on the emotion being labeled. The happiness seems to depend on the pixels linked to the eyes and mouth, whereas the sadness or the anger seem for example to be more related to the eyebrows.

![image](Images/Read_Images/light.png)

## Performance

The set of emotions we are trying to predict are the following :
- Happiness
- Sadness
- Fear
- Disgust
- Surprise
- Neutral
- Anger

The models have been trained on Google Colab using free GPUs.

|       Features                          |   Accuracy    |
|-----------------------------------------|---------------|
| LGBM on flat image                      |     --.-%     |
| LGBM on auto-encoded image              |     --.-%     |
| SVM on HOG Features                     |     32.8%     |
| SVM on Facial Landmarks features        |     46.4%     |
| SVM on Facial Landmarks and HOG features|     47.5%     |
| SVM on Sliding window Landmarks & HOG   |     24.6%     |
| Simple Deep Learning Architecture       |     62.7%     |
| Inception Architecture                  |     59.5%     |
| Xception Architecture                   |     64.5%     |
| DeXpression Architecture                |     --.-%     |
| Hybrid (HOG, Landmarks, Image)          |     45.8%     |



# Live prediction

Since the input data is centered around the face, making a live prediction requires :
- identifying the faces
- then, for each face :
  - zoom on it
  - apply grayscale
  - reduce dimension to match input data

The face identification is done using a pre-trained Histogram of Oriented Gradients model. For further information, check the following article :
https://maelfabien.github.io/tutorials/face-detection/#b-the-integral-image

The treatment of the image is done through OpenCV

*1. Read the initial image*

![alt text](Images/Read_Images/face_1.png)

*2. Apply gray filter and find faces*

![alt text](Images/Read_Images/face_2.png)

*3. Zoom and rescale each image*

![alt text](Images/Read_Images/face_5.png)

Live prediction Illustration :

![alt text](Images/Read_Images/Mon-film-7_1.gif)


