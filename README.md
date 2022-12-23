# Adaptive-Malicious-Detector-on-a-Kafka-Stream
## OverView
Soon...

## Dataset
Soon...

## Methodology
Soon...

## Installation instructions:

To achieve the real time data delivery, I used Kafka and Dockers to 
simulate an independent server which will send information.

![im1](Image/Screenshot_1.png)

1. To install the local Docker and Docker Compose programs on our 
computers. Please use the following link to download it just take into consideration your operative 
system. https://docs.docker.com/get-docker
2. Once your installation is complete, you will have the docker’s desktop interface as shown 
in the following image. It is not necessary to create an account or to perform any upgrade, 
with the free version we will be able to complete the assignment
3. Once it is fully installed download and unzip the folder provided that contains the scripts 
and the .csv with the data.

![im2](Image/Screenshot_2.png)

Once unzipped in your computer, run one of the following files depending on your 
operative system:
- If you are using windows then run the docker_script.bat file 
- Otherwise run the docker_script.sh file on Linux/MacOS

This file will create two docker images wurstmeister/kafka and wurstmeister/zookeeper. 
For this activity you don’t have to worry about what they are or how they run, as everything 
is already configured. At the end of the installation

To validate that the container images were created successfully open Dockers and you 
should see two images running like this

![im3](Image/Screenshot_3.png)

Now you are ready to capture the data stream and start the data treatment for your model. 

## EDA
### Validate data imbalanced with justifications
The dataset is slightly imbalanced as there is a difference of 10% between the false and true classes.

### Statistical analysis of data
Feature distributions

| Histograms               | Boxplots (for outliers and skewness) |
|--------------------------|--------------------------------------|
| ![](Image/Picture1.png)  | ![](Image/Picture2.png)              |

Skewness

| Right skew feature using KDE | Right skew feature using KDE | Statical skewness scores |
|------------------------------|------------------------------|--------------------------|
| ![](Image/Picture3.png)      | ![](Image/Picture4.png)      | ![](Image/Picture5.png)  |

- The highly negative score represents the left skew, and the positive represents the right skew. 



