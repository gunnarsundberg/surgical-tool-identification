# Arthroscopic Tool Classification using Deep Learning

![Alt text](.img/tools.png)

The goal of this project is to automatically differentiate between a small subset of surgical tools during the cleaning phase of shoulder arthroscopic surgery. Shoulder Arthroscopy is a surgical procedure that uses a small camera called an arthroscope to visualize, diagnose, and treat tears and other issues inside of the shoulder area. This procedure is used as a minimally invasive option as opposed to open surgery and is very common, with over 1.4 million arthroscopy surgeries performed per year. The larger goal is to create an automated system that is able to differentiate between expert and novice surgeons for teaching and performance assessment. Automated tool differentiation is a step towards that goal.

## Data Acquisition

Data acquisition began with acquiring a set of footage of arthroscopy rotator cuff procedures from partner surgeons. The footage selected was performed by expert surgeons (expert being defined as surgeons who had undergone fellowship programs for rotator cuff repair procedures and had performed the surgery more than two hundred times, and observed the surgery performed more than two hundred times in the last six months). The footage was then parsed for segments containing the target tools: the shaver tool and the electrocautery tool, as well as segments in which no tools were present. A Python script was implemented utilizing OpenCV library to split the video frames into images.

Splitting the videos into frames yielded 17,572 images from the shaver tool video segments, 25,679 images from the electrocautery tool segment, and 4,834 images with no tools. Frames with heavy motion blur and significant occlusions were dropped, although images with some motion blur and slight occlusion were kept, allowing for a resilient model that performs in real-life surgical conditions. From the 17,572 shaver tool images, 8,691 were selected for training. Of the 25,679 electrocautery tool images, 7,773 were selected for training. All 4,834 images with no tools were used for training. The dataset included images of both tools at a wide variety of poses and angles.

## Model

A CNN (Convolutional Neural Network) was implemented with a ReLU activation function on all layers but the output layer which used Softmax for multiclass classification. The goal of using a CNN was to find the features that differentiated whether an image contained an electrocautery tool, shaver tool, or no tools. Our CNN model used Max Pooling for down-sampling, as well as implemented L2 regularization and Dropout layers to prevent overfitting/underfitting.The CNN was trained with K-Fold Cross Validation with k=10 and was set to run for 200 epochs each fold with an early stopping condition set to end the current fold’s training if the accuracy didn’t improve within five epochs to prevent overtraining.

Overall, the CNN used a total of 26 layers. There was a total of three convolutional blocks made up of seven layers each starting from 32 filters and ending at 128 filters to aid in finding much more detailed patterns. The last five layers were made up of a Flatten layer to organize the data from the convolutional layers to be used in the dense layers, two dense layers with 128 nodes and 64 nodes respectively, a dropout layer to help prevent overfitting/underfitting, and a dense layer with three nodes for output.

## Performance

Evaluation of the model was performed with 10-fold cross validation. The average accuracy of the model across folds was 99.1(+/- 0.49) %.

| Tool | Precision | Sensitivity | Number of Images |
| --- | --- | --- | --- |
| Electrocautery | 0.988 | 0.988 | 777 |
| Shaver | 0.993 | 0.988 | 868 |
| No Tool | 1.00 | 1.00 | 483 |

## Citation

The work in this repository came out of Dr. Bayazit Karaman and Dr. Doga Demirel's lab at Florida Polytechnic University in 2020. The results were published in ICISDM 2020: Proceedings of the 2020 the 4th International Conference on Information System and Data Mining. If you make use of this repository or the paper in which the results were published, please cite our paper. BibTeX citation:

```
@inproceedings{10.1145/3404663.3404672,
author = {Palmer, Bryce and Sundberg, Gunnar and Dials, James and Karaman, Bayazit and Demirel, Doga and Abid, Muhammad and Halic, Tansel and Ahmadi, Shahryar},
title = {Arthroscopic Tool Classification Using Deep Learning},
year = {2020},
isbn = {9781450377652},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
url = {https://doi.org/10.1145/3404663.3404672},
doi = {10.1145/3404663.3404672},
abstract = {Shoulder arthroscopy is a common surgery to diagnose and treat tears to improve patient's quality of life. Quality of cleaning the tear during shoulder arthroscopy significantly affects the outcome of the surgery. Appropriate cleaning is necessary to reduce healing time and avoid feature pain in the area. In this paper, we used convolutional neural networks to automatically differentiate between two tools-electrocautery and shaver tools- that are used during the cleaning phase of a shoulder arthroscopy. We captured images from the actual shoulder arthroscopy videos. We used 8,691 images that contain the shaver tool, 7,773 images that contain the electrocautery tool, and 4,834 images that contain no tools. Our results showed that average accuracy of our model is 99.1(+/- 0.49) %. For the electrocautery tool precision and sensitivity was calculated as 0.988 and 0. 988, respectively. For the shaver tool precision and sensitivity was calculated as 0.993 and 0. 988, respectively. For the no tool scenes precision and sensitivity was calculated as 1.0 and 1. 0, respectively.},
booktitle = {Proceedings of the 2020 the 4th International Conference on Information System and Data Mining},
pages = {96–99},
numpages = {4},
keywords = {Deep Learning, Convolutional Neural Network, Image Recognition},
location = {Hawaii, HI, USA},
series = {ICISDM 2020}
}
```

