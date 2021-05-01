# **Behavioral Cloning** 

Author: Szabolcs Sergyan

## 1. Required Files

### 1.1. Are all required files submitted?

- ```model.py``` contains the code written by me.
- ```drive.py``` contains the code necessary to autonomously drive in Simulator. I did not make any change in this code which was provided by Udacity.
- ```model.h5``` contains the generated model.
- ```writeup.md``` you are reading it right now.
- ```video.mp4``` the output of the Simulator when I tested my model.
- ```README.md``` description of preprocessing and the model.

My training data was in folder ```training_data```, however the submittion of the project was unsuccessful so I copied this folder to ```~/opt/```.

## 2. Quality of Code

### 2.1. The model provided can be used to succesfully operate the simulation.

When I tested my model in the Simulator it was successfully operated. The car did not leave the road surface and lanes were not crossed by the car.

### 2.2. Is the code usable and readable?

The code generates a ```model.h5``` output. If I use this model file for autonomous driving in the Simulator the car is able to perform the track.

I used as many comments as I could, hopefully the code is now understandable.

## 3. Model Architecture and Training Strategy

### 3.1. Has an appropriate model architecture been employed for the task?

First I tried to use LeNet architecture which is well-known from the previous project. I found that it did not provide the expected result so I rejected it.

Then I used the architecture developed by [NVIDIA](https://developer.nvidia.com/blog/deep-learning-self-driving-cars/). The model contains five convolutional layers and five fully connected layers. Here you can see the architecture diagram from NVIDIA homepage. In my case the shape of the input image was different but I used this architecture. ![Architecture image from NVIDIA homepage](./examples/nvidia-architecture.png)

I made the following preprocessing steps:
1. The distribution of steering measurements was not close to uniform distribution. The frequency of steering angle 0 was much higher than other measurements. Therefore I cut out 95% of training data where the steering angle was 0.
2. The car drives on a round track so left turn is overestimated. In order to avoid this problem all images was flipped and stored in the dataset, and paralelly the respective steering measurements was multiplied by -1.
2. I croped the top 60 and bottom 20 rows of images. This part of images is relevant for lane detection.
3. The images were normalized to the range [0, 1]. Then they were shifted to the range [-0.5, +0.5], so the mean was 0.
4. Then the above-mentioned model architecture was used for the data set.

### 3.2. Has an attempt been made to reduce overfitting of the model?

20% of the data set was used for validation. In order to avoid overfitting I used dropout technique before the first four fully connected layer. The keeping probability was 75%, which means in Keras model dropout was used with 0.25 as the input argument.

### 3.3. Have the model parameters been tuned appropriately?

Adam optimizer was used, so learning parameter was irrelevant. I used 10 epohs during training of the model.

### 3.4. Is the training data chosen appropriately?

As I wrote in Section 3.1. the training data was preprocessed. It was successful to keep the car on the track during testing, so I guess the training data was chosen appropriately.

## 4. Architecture and Training Documentation

### 4.1. Is the solution design documented?

See ```README.md```.

### 4.2. Is the model architecture documented?

See ```README.md```.

### 4.3. Is the creation of the training dataset and training process documented?

See ```README.md```.

## 5. Simulation

### 5.1. Is the car able to navigate correctly on test data?

As you can find in ```video.mp4``` the tires did not leave the drivable portion of the track surface and the car did not pop up onto ledges or roll over any surfaces.