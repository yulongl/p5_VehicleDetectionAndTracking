# Vehicle Detection and Tracking
### Yulong Li  

---  

## Demo  
![demo](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/images/demo.gif)  

## Some Words at Beginning  

I know the main goal of this project is using the HOG features approach to train a classifier. However, I had been working on this approach for the whole week and couldn't get a good result. I tried different combanitions of parameters including 'orient', 'pix_per_cell', color space and etc., but none of them gave me a clean and accurate pridiction on test images, even though I always got 98% to 99% test accuracy when training the classifier. I also suspect that the Hog Sub-sampling Window Search method may distort the window image, so the window feature may not be very clean. I even tried suggestions mentioned in the forum to use confidence score over 99% confidence and played around with the heat map, but still getting too many false positives and partially detected vehicles.  

So eventually, I decided to switch to CNN approach in this project, because I think the performance of CNN is much better than HOG - fewer false positives and more accurate predictions.  

Anyway, I still attached the codes for the HOG approach in this repo: [p5_HOG_SVM.py](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/p5_HOG_SVM.py) trains the Linear SVC and [p5_HOG_test.py](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/p5_HOG_test.py) implements Hog Sub-sampling Window Search method to test on sample frames. It'll be great if reviewer could help me to figure out how to improve the codes to get better results.  

**This project writeup will introduce the approch of CNN with Keras, instead of HOG feature extraction.**  

---
### Model Training  

[p5_CNN.py](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/p5_CNN.py) shows the CNN training and testing process with Keras.  
[model_1.h5](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/model_1.h5) is the trained model.  

#### 1. Training and Testing Data  

The given GTI and KITTI datasets and some extractions from the project video are used for vehicle class. The given 'Extras' dataset is used for nonvehicle class. Traning and testing data are split by the ratio of 0.8:0.2.

#### 2. CNN Model Architecture

Model architecture generated by Keras:  
![model_1](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/images/model_1.png)  

#### 3. Training and Testing Results

Training accuracy = 98.42%  
Training loss = 0.0192

Testing accuracy = 99.06%  
Test loss = 0.0112  

---  

### Sliding Window Search on Sample Frame Images  

[p5_CNN_search_win.py](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/p5_CNN_search_win.py) describes window searching process on test images.   

#### 1. Sliding window search

The size of the sliding window is (64, 64). I used 3 scales: 1.2, 1.5 and 1.8. Window overlap is set at 0.6.

#### 2. Examples

Window search result:  
![win_search](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/images/win_search.png)  

Heat map and final result:  
![heatmap](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/images/heatmap.png)  

---

### Video Implementation

[p5_CNN_video.py](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/p5_CNN_video.py) shows the process of tracking vehilces in a video.  

#### 1. Link to the final video output

[test_output.mp4](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/test_output.mp4): https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/test_output.mp4    


#### 2. Filter and threshold for false positives

Class HotMap is used to store the past 10 box list which are predicted as a vehicle. For every frame prediction, it adds up the past 10 box lists and the current box list to make the heap map. A threshold of 28 is used to make the final prediction.

#### 3. Example  

![demo](https://github.com/yulongl/p5_VehicleDetectionAndTracking/blob/master/images/demo.gif)  

---

### Discussion

#### 1. What is the advantage of HOG feature extraction?  

The first problem I have is, I don't really understand the advantage of HOG features extraction, performance wise. Maybe speed wise it's better than heavy CNN approach, though I still get around 8000 features for HOG and other histogram combination. However, the thing is, I cannot accept the performance of the HOG approach, which generates too many false positives, even after I tried tuning the confidence scores, many combinations of HOG parameters and differenc classifier kernals.  

Comparing to HOG, CNN is much easier to implement and the performance is way better. The only few false positives I got are from the opposite direction vehicles coming by, which maybe not a bad thing. Speed wise, it's not that slower than HOG, considering a small step size and 3 sliding window sizes. I'm sure if I work more on simplifying the structure of the CNN model, it could be even faster.  

#### 2. Overlapped vehicles  

When the black car and the white car overlapped with each other, it combines into one big box. But I think a better algorithm should be able to track two cars seperately.  

#### 3. Smooth tracking  

I think the tracking box is still not smooth enough. Some work on the final predicted boxes after the hot map process may work.  


