# Real-Time Human Identification System



### Problem Overview
 Real-Time Human Identification is an essential application of biometric and surveillance systems, especially in security, law enforcement, and KYC Verifications. It is challenging as it aims to identify each human as a unique entity. Our project aims to address this challenge by developing a deep learning-based Real-Time Human Identification System (RHIDS) which detects persons for a given image or frame of video and identifies the persons uniquely with their unique ids or names. 



### Approach and Architecture
We have planned three different approaches that were used to develop the Real-Time Human Identification System (RHIDS). The first approach is Human Extraction, which is used to detect humans in a given image. The second is the Human-to-Human Matching approach, which involves measuring whether two images correspond to the same person or different persons. The third approach is the RHIDS System Architecture, which is based on the Human Extraction and Human-to-Human Matching approach and is used to identify each human uniquely with a unique id in a video or live stream.















### Human Extraction
We use a Trained Object Detection model which is trained to detect humans in a given image. Then detected humans will be extracted from the image using indexing the bounding box values.


### Human-to-Human Matching
We use a trained image feature extractor which can extract features of the human in the given image. Using this feature extractor, the system extracts feature vectors for each image and a cosine similarity function is used to measure the similarity score between two feature vectors. Now using an estimated threshold, the system decides that the images are of the same person if the similarity score is greater than the threshold value otherwise images are of different persons. More about feature extractor and cosine similarity is given below.




**Feature Extractor:** The feature extractor is a deep learning-based model that takes input images and extracts a set of features that are representative of the visual characteristics of the input images. These features are then used to calculate the cosine similarity score between the two input images, which is used to determine whether the two images correspond to the same person or different persons.

Feature extractors are typically based on neural networks, which are trained on large datasets of input data to learn how to identify and extract the most important features of the data. In the case of image-based feature extraction, the neural network may consist of a series of convolutional layers that perform feature extraction and dimensionality reduction on the input images. The extracted features are then passed through one or more fully connected layers to produce a final feature vector that can be used for classification or similarity analysis.

In our use case, we have taken OSNet as the feature extractor to extract features of humans from a given image. This model was trained on the Market1501 dataset. 


Basically, OSNet stands for Omni-Scale Network is a combination of multiple residual blocks of different depths followed by channel-wise adaptive aggregation of results or vectors received from each residual block. 



OSNet Paper: https://openaccess.thecvf.com/content_ICCV_2019/papers/Zhou_Omni-Scale_Feature_Learning_for_Person_Re-Identification_ICCV_2019_paper.pdf <br>
Code: https://github.com/KaiyangZhou/deep-person-reid <br>
Market1501 Dataset: https://zheng-lab.cecs.anu.edu.au/Project/project_reid.html 


**Cosine Similarity:** Cosine similarity is a measure of similarity between two non-zero vectors in a multi-dimensional space. It is often used as a way to compare the similarity between two feature vectors in machine learning applications. Cosine Similarity value ranges between 1 to -1 in which 1 means fully similar and -1 means fully dissimilar. 

**Threshold Verification:** An estimated threshold value is selected to determine whether two vectors can be considered similar or dissimilar. For our use case, we have taken 0.7 as the threshold value.










### RHIDS System Architecture
In our Real-Time Human Identification System, we combine Human Extraction and Human-to-Human Matching in a loop of video or stream inference. Here is the step-by-step approach given - 

1) First, we take an empty list of galleries that will contain feature vectors of different humans. 
2) From the first frame, the Object detector will detect Humans in the frame and use the Human Extractor approach to get images of each human in the frame.  
3) For all humans, the feature extractor extracts the feature vector for each human and stores these vectors in another list of queries. 
4) Now if the list of galleries is empty then all vectors in the list of queries will be stored. 
5) If the list of galleries is not empty then the cosine similarity function based on Human-to-Human matching will be applied to each pair of combinations of vectors in queries and galleries and generate a matrix of similarity scores which size will MxN where M is the number of feature vectors in queries and N is the number of feature vectors in galleries. This matrix can be referred to as a Similarity matrix.
6) Now the threshold verification will be applied to Similarity Matrix and find the pre-image and ID (index number in gallery list) of the pre-image of every feature vector of the query list in the gallery list. If any of the feature vectors in the query list does not have any pre-image in the gallery list i.e. every feature vector in the gallery has a similarity score less than the threshold then than feature vector will be appended to the gallery feature vector. 









