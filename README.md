# CLO-C
## : a compound word of clothes and celsius
![image](https://github.com/CLO-C/Wiki/assets/127665166/bfb00318-1c1c-43f7-9b58-386b8f88cfb4)

![image](https://github.com/CLO-C/Wiki/assets/127665166/92c59234-14c2-477b-b9cb-6e1adf5a55ac)

## Motivation
Just as it is often warm despite the recent December weather, it is difficult to choose clothes because it is often different from the previously known weather. In addition, as the season returned, there were many times when I wondered how to dress this time last year. To solve this problem, we came up with CLO-C.

## Object
Based on the user's location and current temperature, the focus is on recommending clothes suitable for the individual. Using the object recognition function of the YOLOv8 model, it analyzes the clothes currently worn by the user and suggests the optimal clothes accordingly. This will allow users to choose the right clothes for the current climate conditions, regardless of the season.


<details>
<summary><h1> 🙋🏻 구성원/관심분야 </h1></summary>
<div markdown="1">       

|이름|학번|이메일|관심분야|
|---|---|---|---|
|김민규|202035507|mkkim01@gachon.ac.kr|데이터|
|구도연|202135705|kdy1021@gachon.ac.kr|컴퓨터비전|
|김민선|202135726|minseon9286@naver.com|카메라 인식 / 프론트엔드|
|심서현|202135791|angelsh0805@gachon.ac.kr|백엔드|
|정승민|202135832|taky0315@naver.com|비전/데이터|

</div>
</details>

# System Architecture

![image](https://github.com/CLO-C/Wiki/assets/127665166/69b50d4f-c10b-48a0-af1b-a50ababeec3a)

1. In the React Native App, the user uploads the image.
2. Save photos in Firebase storage, create a collection via link, and return the id to the app.
3. Change the app to a form that is good to deliver to fastAPI and deliver the value.
4. Get the ID value with the yolo python code and access the firebase storage to obtain the image
5. Use the obtained image as an input to make a prediction
6. Store the prediction result image and the cropped picture in firebase storage and receive the link
7. Create a collection with the link you received and save it in the firestore.
8. In React native, app can get an image by accessing the stored firestore.


# Model(Yolo_v8)
![image](https://github.com/CLO-C/Wiki/assets/127665166/6eceebc9-d287-4203-b49b-00751cbf495b)

is the latest version of YOLO by Ultralytics. As a cutting-edge, state-of-the-art (SOTA) model, YOLOv8 builds on the success of previous versions, introducing new features and improvements for enhanced performance, flexibility, and efficiency. YOLOv8 supports a full range of vision AI tasks, including [detection],  [segmentation], [pose estimation], [tracking, and [classification]. This versatility allows users to leverage YOLOv8's capabilities across diverse applications and domains.


![image](https://github.com/CLO-C/Wiki/assets/105360496/489311ed-009e-422e-9457-a22a5d945453)


url : https://docs.ultralytics.com/#yolo-a-brief-history

## Tunning

we can modify config -> model -> yolov8 -> yolov8.yaml file. Change number of classification 80 to 10.
and make we use model -n the smallest model in yolov8


<img width="311" alt="스크린샷 2023-12-13 오후 2 53 05" src="https://github.com/CLO-C/Wiki/assets/105360496/755af9ee-08d3-4b17-82e5-aac1ea98d4cf">


And we Train model with yolov8n.yaml and data.yaml


## Yolov8_cocoDataset
<img width="736" alt="스크린샷 2023-12-13 오후 3 25 01" src="https://github.com/CLO-C/Wiki/assets/105360496/a9140bc1-ac4d-4a03-8012-2b265e2b66e9">

## Our Dataset
<img width="995" alt="스크린샷 2023-12-13 오후 3 35 07" src="https://github.com/CLO-C/Wiki/assets/105360496/4dfc80ee-e3ba-4a50-a14e-a999f40f7dda">
<img width="853" alt="스크린샷 2023-12-13 오후 3 36 30" src="https://github.com/CLO-C/Wiki/assets/105360496/ceff4ef4-1dad-4827-a463-361cb184e444">


Dataset for detecting clothes in various portraits.



## Model structure
<yolov8.yaml>
- Ultralytics YOLO 🚀, AGPL-3.0 license
YOLOv8 object detection model with P3-P5 outputs. For Usage examples see https://docs.ultralytics.com/tasks/detect
train: /Users/jeongseungmin/Desktop/20231206/Train_Result/train
val: /Users/jeongseungmin/Desktop/20231206/Train_Result/test

nc: 10 
names: ['sunglass','hat','jacket','shirt','pants','shorts','skirt','dress','bag','shoe']


scales: model compound scaling constants, i.e. 'model=yolov8n.yaml' will call yolov8.yaml with scale 'n'
     [depth, width, max_channels]
  n: [0.33, 0.25, 1024]  # YOLOv8n summary: 225 layers,  3157200 parameters,  3157184 gradients,   8.9 GFLOPs
  s: [0.33, 0.50, 1024]  # YOLOv8s summary: 225 layers, 11166560 parameters, 11166544 gradients,  28.8 GFLOPs
  m: [0.67, 0.75, 768]   # YOLOv8m summary: 295 layers, 25902640 parameters, 25902624 gradients,  79.3 GFLOPs
  l: [1.00, 1.00, 512]   # YOLOv8l summary: 365 layers, 43691520 parameters, 43691504 gradients, 165.7 GFLOPs
  x: [1.00, 1.25, 512]   # YOLOv8x summary: 365 layers, 68229648 parameters, 68229632 gradients, 258.5 GFLOPs

- YOLOv8.0n backbone
backbone:
    [from, repeats, module, args]
  - [-1, 1, Conv, [64, 3, 2]]  
  - [-1, 1, Conv, [128, 3, 2]] 
  - [-1, 3, C2f, [128, True]]
  - [-1, 1, Conv, [256, 3, 2]]  
  - [-1, 6, C2f, [256, True]]
  - [-1, 1, Conv, [512, 3, 2]]  
  - [-1, 6, C2f, [512, True]]
  - [-1, 1, Conv, [1024, 3, 2]] 
  - [-1, 3, C2f, [1024, True]]
  - [-1, 1, SPPF, [1024, 5]]  

- YOLOv8.0n head
head:
  - [-1, 1, nn.Upsample, [None, 2, 'nearest']]
  - [[-1, 6], 1, Concat, [1]]   cat backbone P4
  - [-1, 3, C2f, [512]]  # 12

  - [-1, 1, nn.Upsample, [None, 2, 'nearest']]
  - [[-1, 4], 1, Concat, [1]]   cat backbone P3
  - [-1, 3, C2f, [256]]   15 (P3/8-small)

  - [-1, 1, Conv, [256, 3, 2]]
  - [[-1, 12], 1, Concat, [1]]   cat head P4
  - [-1, 3, C2f, [512]]   18 (P4/16-medium)

  - [-1, 1, Conv, [512, 3, 2]]
  - [[-1, 9], 1, Concat, [1]]   cat head P5
  - [-1, 3, C2f, [1024]]   21 (P5/32-large)

  - [[15, 18, 21], 1, Detect, [nc]]   Detect(P3, P4, P5)


## Result

We progress in Colab because of GPU and we get 100 Epoch in Colab for 3 hours
And save model parameter "yolov8n-Epoch100.pt".

So we can get result in seconds

<img width="410" alt="스크린샷 2023-12-13 오후 3 08 13" src="https://github.com/CLO-C/Wiki/assets/105360496/42d433b5-42ea-4471-84c7-5e00b475f6a0">

# App
![image](https://github.com/CLO-C/Wiki/assets/127665166/5991819c-69ad-48d2-9fca-b21585c97bb6)


## Home Screen

![image](https://github.com/CLO-C/Wiki/assets/127665166/b6248d88-da42-4072-a25e-79d907f21ef3)

1. top
Output weather data in real time (emulator is positioned as San Francisco)
Outputs the five-day weather forecast every three hours

2. center
Print a cropped recommended picture

3. Last
Showing pictures of your outfit that meet the requirements

## Calendar Screen

![image](https://github.com/CLO-C/Wiki/assets/127665166/2483b8ea-e11e-463c-a13d-66b9b895100b)

Print your own clothes to the calendar
The picture I took is expressed on the calendar (expressed with dots)
When choosing a date, you can see your own picture


## Add Screen

![image](https://github.com/CLO-C/Wiki/assets/127665166/05770664-25e1-43bc-8a96-87a02ac7c8d8)

If you post a picture of your outfit and choose a variety of evaluations
Saved on Firebase


## Closet Screen

![image](https://github.com/CLO-C/Wiki/assets/127665166/f487c50f-585d-483c-85ab-1b5e9dcee2bc)

You can take pictures of your outfit and save it
Classifying and showing in the closet

# Firebase
![image](https://github.com/CLO-C/Wiki/assets/127665166/fc2365f8-ff6b-4374-b4b7-c4b6ce1b3a2d)

<br/>

![image](https://github.com/CLO-C/Wiki/assets/127665166/cc5717c6-a0b8-4026-b821-fb6693c5ff63)
<br/>:Users uploading photos, leaving feedback, and then saved to Firebase with weather and date information

<br/>

![image](https://github.com/CLO-C/Wiki/assets/127665166/730f027b-288f-4d63-8515-823cae930f17)
<br/>: The ID of the feedback collection above is received and stored, and the result predicted by the model is uploaded to the firebase storage.

<br/>

![image](https://github.com/CLO-C/Wiki/assets/127665166/818dcfef-4426-4c0c-87b4-2af8a50c764f)
<br/>: The result is saved in a collection called modelResult.
