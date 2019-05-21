### About the Project
Given 30000 food images from 59 classes I have performed image classification task. Due to confidentiality I am unable to upload data here.
The goal of the project is improve the performance by introducing the strategy to capture high intra-class variance and low inter-class variance.<br>
There are a lot of algorithms that can be utilised such as :
* Center Loss (*A Discriminative Feature Learning Approach for Deep Face Recognition* - https://ydwen.github.io/papers/WenECCV16.pdf), 
* Large-margin Gaussian Mixture Loss (*Rethinking Feature Distribution for Loss Functions in Image Classification* - http://openaccess.thecvf.com/content_cvpr_2018/papers/Wan_Rethinking_Feature_Distribution_CVPR_2018_paper.pdf),etc.
<br><br><br>

### Center Loss
As given food data have low-inter class and high intra-class variance, besides Softmax loss function there is a need to enhance the discriminative power of the deeply learned features. Center Loss learns a center for deep features of each class and penalizes the distances between the deep features and their corresponding class centers. Deeply learned features are required to be discriminative and generalized enough for identifying new unseen data.

<p align="center">
  <img width="400" height="300" src="readme_pictures/DLF.png">
</p>

The proposed Center Loss function minimizing the intra-class variations while keeping the features of different classes separable and the function looks as below:

<p align="center">
  <img width="300" height="80" src="readme_pictures/DistancetoCenter.png">
</p>

Cyi - yi class center of deep features. The centers are updated based on each mini-batch, in each iteration the centers are computed by averaging the features of the corresponding classes. In addition, to avoid large shifts caused by misclassified classes, the alpha scalar is used to control the learning rate of the centers.<br>
Both loss functions are combined to make the model more robust and generalize better for unseen data.<br>

<p align="center">
  <img width="500" height="120" src="readme_pictures/Lost_Function.png">
</p>

From the above Center Loss function it is obvious that optimization can be done easily to that function. In the combined loss function lambda scalar is used for balancing the two loss functions which also make run only Softmax loss by setting lambda to zero. With proper lambda value the discriminative power of deep features can be significantly enhanced.<br>


<p align="center">
  <img width="350" height="350" src="readme_pictures/deep_features_1.png"><img width="400" height="350" src="readme_pictures/deep_features_2.png">
</p>

For the Food data lambda=0.001 value was used to balance between Softmax Loss and Center Loss. For both functions Adagrad optimizer was used with learning rate = 0.001. 

<p align="center">
  <img width="500" height="120" src="readme_pictures/lost_food_data.png">
</p>

*The highest validation accuracy was achieved with epoch = 17*
<p align="center">
  <img width="500" height="120" src="readme_pictures/best_accuracy.png">
</p>

#### Justification for Center Loss not performing as good as Softmax Loss
Even though Center loss indeed taking into account intra-class compactness, but it does not affect inter-class separability. For given Food data set there are many dishes from the same ingredients such as pork dishes which is quite difficult to differentiate, in addition drink classes adding additional confusion because almost all of them look similar. 
For the future work, I-Center Loss could be used that propose not only better intra-class compactness but also better inter-class separability. 
