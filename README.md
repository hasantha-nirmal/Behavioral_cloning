# Behavioral cloning (For Automated Driving)

The goals / steps of this project are the following:

1. Use the simulator to collect data of good driving behavior

2. Build, a convolution neural network in Keras that predicts steering angles from images

3. Train and validate the model with a training and validation set

4. Test that the model successfully drives around track one without leaving the road

5. Summarize the results with a written report


## Use the simulator to collect data of good driving behavior

First and foremost, in order to build a CNN, we must collect the data. This is for not just only to train the network but identify the preprocessing part that need to be done to the data(data augmentation techniques such as crop, horizontal flips etc.).So in this scenario we used the Udacity self-driving car simulator version 2.The data is recorded as 320*160*3 resolution and it’s got it’s corresponding .csv file as labeler. The data from the simulator consists of images that are taken from the three deferent cameras from the vehicle. Front left, front right and center cameras are those 3 cameras. So, for every single frame, there are 3 images representing it’s Centre, left and right position. Let’s see why it’s needed such three positions later. The data must be collected by us by driving the vehicle down the track. In this case I have only used the first track of the version 2 simulator. As instructed I have used 4 tracks of recording in both straight and reverse sides of the track.(Reverse means that we have driven the vehicle opposite side of the track).This is mainly due to the given track is biased to left side(sharp turns etc.),so driving in the reverse direction helps to reduce that biases effect. As mentioned before after the recording IMG folder and it’s corresponding .csv file could be obtained .csv file consists of the Centre, left and right image of a certain position, the steering, throttle, reverse and speed of the vehicle at that position. But in this scenario, we only focus on the steering angle of the vehicle which is in the interval [-1,1]

