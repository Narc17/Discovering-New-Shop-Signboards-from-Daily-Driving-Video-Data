# Discovering New Shop Signboards from Daily Driving Video Data

This repo is one of the project description of Machine Discovery and Social Network Mining Lab, from the Department of Computer Science and Information Engineering, National Taiwan University. This project collaborated with a consumer technology company, Omnieyes, in terms of ***discovering new shop signboards within the driving records of delivery box trucks.***


## Introduction

Logistic companies usually arrange for a delivery box truck to deliver packages in a specific district. Each box truck installed a dashcam with GPS recording function for providing video evidences if it involved in car accidents. Besides of being car accident evidences, these daily driving records contain precious landscape information and corresponding GPS coordinates of its delivering district. The idea of this project is to ***"compare"*** shop signboards in a particular location between recent recorded driving video and the videos recorded several months ago, as well as locating new shops by finding their brand new shop signboards in that location. 



Driving records are provided by our collaborating company, Omnieyes. They organized each daily driving record into numerous 1-minute video segments and a corresponding GPS coordinates list according to the video second of every segment. A video segment sample is illustrated as below (Fig.1, 2):

<figure>
    <p align="center"><img src="/imgs/driving_record_sample.PNG" alt="Video segment sample">  
    <p align="center">Fig.1) 1-minute driving record video segment (a sample image, not a video)
    <p align="center"><img src="/imgs/gps_record_sample.PNG" alt="GPS record sample">  
    <p align="center">Fig.2) corresponding GPS coordinates 
</figure>


## Methodology
It is very difficult to achieve our goal if we process driving records straight forward. We need to extract useful shop signboard information in these driving records and ***"compare"*** them appropriately. Thus, we bring up with Divide-and-conquer approach to build our system. It takes **ONE** recent (query) driving record and **MULTIPLE** old (reference) driving records as input data. The system architecture is shown below (Fig.3):
<figure>
    <p align="center"><img src="/imgs/system_architecture.PNG" alt="System architecture">  
    <p align="center">Fig.3) system architecture
</figure>


We break our system down to 5 modules for 3 main phases: ***collect data, extract data and compare data***. Collect data phase is to search suitable data between query and reference driving records as GPS pairs. Then it detects shop signboards within these pairs, which extract data phase does. At last, compare data phase  ***compares*** extracted shop signboards and outputs system predictions. We are going to explain these 3 main phases in breif below:

**1. Collect data**
 - As we all knew driving records are videos. There are too much redundant landscape information in a one day long query driving record. For example, the landscape captured by an unloading box truck. Therefore, before we pair query and reference driving records, we need to sample query driving record appropriately. To do so, we sample query video frames by their GPS coordinates with a desired haversine distance gap. (Fig.4) <figure><p align="center"><img src="/imgs/query_sampling.PNG" alt="Query sampling"><p align="center">Fig.4) illustration of query sampling result</figure>
 - After query driving record sampling, we pair these sampled query image frames with video intervals from reference driving records by the haversine distance differences of  GPS coordinates between them. Besides, we also need to check the box truck driving directions between query and paired reference data on each paired result for data consistancy. Two videos at same geographical location but with different driving directions on an intersection, for instance. We utilize haversine formula to compute GPS bearing as driving direction to filter different driving direction reference video interval. As a result, all query and reference data are in similar geographical locations and driving directions in all GPS pairs and each GPS pair includes one query video frame image and one or more reference video intervals. (Fig.5) <figure><p align="center"><img src="/imgs/gps_pairing.PNG" alt="GPS pairing"><p align="center">Fig.5) illustration of pairing result</figure>

**2. Extract data**
 - We utilize an esemble detection model for detecting shop signboards on query video frame image in each GPS pair. It combines the predictions of a faster-RCNN with FPN model and a YOLOv5x model and applying Non Maximum Suppression (NMS) to eliminate redundant predictions. These two detection models are trained on a private dataset including 10,000 dashcam images with shop signboard labels. (Fig.6) <figure><p align="center"><img src="/imgs/detection_res.PNG" alt="Detection result"><p align="center">Fig.6) illustration of query shop signboard detection result</figure>
 - For reference video intervals in each GPS pair, we implement deepSORT tracking algorithm to track all shop signboards in it. It requires a detection model and embedding model for detecting objects we want to track and providing embedding features of the detected objects to achieve better tracking results. We adopt our ensemble detection model and embedding model for shop signboards for the implementation.  (Fig.7) <figure><p align="center"><img src="/imgs/tracking_res.gif" alt="Tracking result"><p align="center">Fig.7) illustration of reference shop signboard tracking result</figure>

**3. Compare data**
 - We have extracted shop signboards from query video frame image on our left hand and tracked shop signboards from reference video intervals on our right hand. Finally we can find out if there are new shop signboards in query video frame image. On comparison task, Triplet Network is the one we want. We trained an embedding model using triplet loss and Euclidean distance metric on another private dataset with a ResNet-50 backbone without fully-connected layer. This private dataset contains 1,800 kinds of shop signboards with different capture angles and weathers. After proper training, embedding model is able to generate embedding features that the embedding distance of two semantically identical shop signboards are small, and vice versa. (Fig.8) <figure><p align="center"><img src="/imgs/comparison_res.PNG" alt="Comparison result"><p align="center">Fig.8) illustration of shop signboard comparison result</figure>
 - After finishing all comparisons between every query and reference shop signboard, system generates ***summary images*** for each query shop signboard to sum up comparison results. A sample of ***summary image*** is shown in Fig.9. On the left of the image, it showed the current comparing query shop signboard and its captured driving view for extra information to users. On the other side, it showed top-10 reference shop signboard candidates with the smallest embedding distances within all tracked reference shop signboards. Therefore, users is able to discover a new shop signboard if top-10 reference shop signboard candidates are semantically different to query shop signboard. <figure><p align="center"><img src="/imgs/system_res.PNG" alt="System result"><p align="center">Fig.9) illustration of system prediction (summary image)</figure>


## Codes
We will release the codes about shop signboard detection and embedding models with their related sample dataset as soon as we got the permission from our cooperating company.
