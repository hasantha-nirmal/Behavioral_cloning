# Behavioral cloning (For Automated Driving)

The goals / steps of this project are the following:

1. Use the simulator to collect data of good driving behavior

2. Build, a convolution neural network in Keras that predicts steering angles from images

3. Train and validate the model with a training and validation set

4. Test that the model successfully drives around track one without leaving the road

5. Summarize the results with a written report


## Use the simulator to collect data of good driving behavior

First and foremost, in order to build a CNN, we must collect the data. This is for not just only to train the network but identify the preprocessing part that need to be done to the data(data augmentation techniques such as crop, horizontal flips etc.).So in this scenario we used the Udacity self-driving car simulator version 2.The data is recorded as 320*160*3 resolution and it’s got it’s corresponding .csv file as labeler. The data from the simulator consists of images that are taken from the three deferent cameras from the vehicle. Front left, front right and center cameras are those 3 cameras. So, for every single frame, there are 3 images representing it’s Centre, left and right position. Let’s see why it’s needed such three positions later. The data must be collected by us by driving the vehicle down the track. In this case I have only used the first track of the version 2 simulator. As instructed I have used 4 tracks of recording in both straight and reverse sides of the track.(Reverse means that we have driven the vehicle opposite side of the track).This is mainly due to the given track is biased to left side(sharp turns etc.),so driving in the reverse direction helps to reduce that biases effect. As mentioned before after the recording IMG folder and it’s corresponding .csv file could be obtained .csv file consists of the Centre, left and right image of a certain position, the steering, throttle, reverse and speed of the vehicle at that position. But in this scenario, we only focus on the steering angle of the vehicle which is in the interval [-1,1]

![](/README/Picture1.png)

## Build, a convolution neural network in Keras that predicts steering angles from images 
 
First let’s split the data in to 2 sets. Training and validation set. We need a validation set in order to reduce the overfit. We have got total of 10,450 images for the training from all the three cameras. In order to visualize our data, we built a histogram of 25 bins that represent the frequencies of each Centre, left and right of the steering wheel angles. As we can see, we have got very high number of frames which represent the images when the vehicle drives in the center of the road without any turns. This high number is import since our main target is to drive the vehicle in middle of the road. Then we did a random shuffle and random split of data to train and validation sets and we have got 5928 training images and 1482 validation images, which is in the range of recommendation of the NVIDIA model. After we have got a uniform distribution of frequencies vs steering angle in both validation and training data, it’s time for the preprocessing and image augmentation.  


![Balanced Dataset (Inspired by Gausssian Distribution)](/README/Picture2png.png)

![](/README/Picture3.png)


Before build the neural net, we indeed must do preprocessing of data because the captured images contained very unimportant information So, first we zoomed each image by x1.3 times. By doing so we were almost able to get rid of the portion of image that has captured the glimpse of front truck of the car. It also cleared out the sky and trees from the picture for some extent. For an example the skyline, trees from the distance etc. We don’t need those to train our neural network because that would result unwanted feature maps and waste of computation resources. The we used panning in order gives a blur to the data. By doing so we could focus more on the road and not the surrounding. We shouldn’t use a gaussian kernel because it’s just smoothed all the image but not panned the specific area. Then we altered the brightness for better performance special reduce to reduce the contrast, so some areas are sharper after that. Gamma correction is also possible in this scenario. Of course, we used another data augmentation technique horizontal flip which not only reduce the biased of the NN but also, it’s doubles the data we have already got. More good data are always welcomed. From the research paper of NVIDIA, it has mentioned that instead of using a tradition RGB channels, the YUV channels gives good result in training. Hence, we convert from RGB to YUV color channels, followed by a gaussian smoothing. After doing so, there were still some tree lines, part of the sky is in our image. So we removed the portion from our image by cropping the image from the top which results the resolution 66*300*3.This resolution is the recommended for the NVIDIA model that has been used here so even if there are no tree lines appear at the first place, we have to resize the image into this resolution as step in preprocessing before fed imaged into the NN. 

![](/README/Picture4.png)

![](/README/Picture5.png)

![](/README/Picture6png.png)

![](/README/Picture7.png)

![](/README/Picture8png.png)

## Train and validate the model with a training and validation set 
 
The architecture we used to build a CNN is from the NVIDIA research paper. They have claimed that it is very good for the self-driving cars. It’s a sequential model consists of 5 convolution layers with input image size of 66*300*3 as mentioned above. We reduced the kernel size appropriately with the help of NVIDIA paper to enhance the convolution process. We use Rectified Linear unit as the activation function and in order to reserve details, there are no pooling players. The dropout layers which use to drop some random activations which leads to overcome overfitting the model is not used after trial and errors that was composed from us. For this given data, we have seen that it’s better without any of dropouts. Of course, dropout layers are very much useful in order to avoid overfitting but for some reason it has given us a good validation loss without any of the default dropout layers. After the flattening layer we use 3 dense layers and the output layer (also dense layer). We have used ReLu as the activation function here, likewise in the convolution layers. We also used the Adam optimizer with learning rate of 1e-3. There were 11 models in total and we used both the Google Collab and my local machine with NVIDIA MX130 GPU for the training and this final model is trained by my local machine. 


![](/README/Picture9.png)


Then in fit generator we used NVIDA recommended values such as 300 steps for epochs etc. We perform 10 epochs and we have got no need of using keras functions such as early stopping or checkpoints due to good model performance. 

![](/README/Picture8.png)
