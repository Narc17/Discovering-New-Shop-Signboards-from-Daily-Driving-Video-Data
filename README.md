# Discovering New Shop Signboards from Daily Driving Video Data
This repo is one of the project description of Machine Discovery and Social Network Mining Lab, from the Department of Computer Science and Information Engineering, National Taiwan University. This project collaborated with an innovative AI application company, Omnieyes, in terms of ***discovering new shop signboards within the driving records of delivery box trucks.***

# Introduction
Logistic companies usually arrange for a delivery box truck to deliver packages in a specific district. Each box truck installed a dashcam with GPS recording function for providing video evidences if it involved in car accidents. Besides of being car accident evidences, these daily driving records contain precious landscape information and corresponding GPS coordinates of its delivering district. The idea of this project is to ***"compare"*** the shop signboards in a particular location between recent recorded driving video and the videos recorded several months ago. 

Driving records are provided by our collaborating company, Omnieyes. They organized each daily driving record into numerous 1-minute video segments and a corresponding GPS coordinates list according to the video second of every segment. A video segment sample is illustrated as below:

<figure>
    <p align="center"><img src="/imgs/driving_record_sample.PNG" alt="Video segment sample">  
    <p align="center">1-minute driving record video segment (a sample image, not a video)
    <p align="center"><img src="/imgs/gps_record_sample.PNG" alt="GPS record sample">  
    <p align="center">corresponding GPS coordinates 
</figure>


# Methodology
