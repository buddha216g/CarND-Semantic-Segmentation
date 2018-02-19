# Semantic Segmentation
### Introduction
In this project, you'll label the pixels of a road in images using a Fully Convolutional Network (FCN).

### Model

A pre-trained VGG-16 network was used to extract input, keep probability, layer 3, layer 4 and layer 7. Final fully connected layer was converted to a 1x1 convolution with depth 2 (2 classes: road vs not-road). Performance is improved through the use of skip connections, performing 1x1 convolutions ( layers 3 and 4) and adding them element-wise (through transposed convolution). Every created convolutional and deconvolutional layer use a random-normal kernel initializer with standard deviation 0.01 and a L2 kernel regularizer with L2 0.001.

The following layers were created from pre-trained VGG.
- Convolutional layer (conv_1x1_7) with kernel 1 from VGG's layer 7 to maintain spatial information.
- Deconvolutional layer (upsample_1) with kernel 4 and stride 2 from the conv_1x1_7 layer
- Convolutional layer (conv_1x1_4) with kernel 1 from VGG's layer 4 (vgg_layer4_out)
- skip layer (skip_1) created by adding conv_1x1_4 and upsample_1
- Deconvolutional layer(upsample_2) with kernel 4 and stride 2 from skip_1
- Convolutional layer (conv_1x1_3) with kernel 1 from VGG's layer 3 (vgg_layer3_out)
- skip layer (second_2) created by adding conv_1x1_3 and upsample_2
- Deconvolutional layer (nn_last_layer) with kernel 16 and stride 8 from the second skip layer (second_2).

The cross-entropy loss functon, and Adam optimizer were used to train the network.

Several hyper parameter combinations epochs (5,30,48 and 50), learning rate (0.00008,0.00005,0.00001) were used for training.

After trial and error the following hyperparameters were selected:
- keep_prob: 0.5
- learning_rate: 0.00001
- epochs: 50
- batch_size: 5

Average Loss decreased from 0.65 (5 epochs) to 0.035 (50 epochs). See the sample images below to notice the segmentation enhacement as epochs increase.


### Sample Images after training
##### After 5 Epochs
![5 epochs um_000000](https://user-images.githubusercontent.com/15799394/36393884-8badeda2-15d7-11e8-9afb-5022c1aee3e1.png)
##### After 30 Epochs
![30 epochs um_000000](https://user-images.githubusercontent.com/15799394/36393885-8bdb4f0e-15d7-11e8-85fe-74fb5b3c91c2.png)
##### After 48 Epochs
![48 epochs um_000000](https://user-images.githubusercontent.com/15799394/36393886-8c0933b0-15d7-11e8-91f1-4d3233233a65.png)
##### After 50 Epochs
![50 epochs um_000000](https://user-images.githubusercontent.com/15799394/36393888-8c3a4fd6-15d7-11e8-9af4-50f3a65bff8d.png)



### Setup
##### Frameworks and Packages
Make sure you have the following is installed:
 - [Python 3](https://www.python.org/)
 - [TensorFlow](https://www.tensorflow.org/)
 - [NumPy](http://www.numpy.org/)
 - [SciPy](https://www.scipy.org/)
##### Dataset
Download the [Kitti Road dataset](http://www.cvlibs.net/datasets/kitti/eval_road.php) from [here](http://www.cvlibs.net/download.php?file=data_road.zip).  Extract the dataset in the `data` folder.  This will create the folder `data_road` with all the training a test images.

### Start
##### Implement
Implement the code in the `main.py` module indicated by the "TODO" comments.
The comments indicated with "OPTIONAL" tag are not required to complete.
##### Run
Run the following command to run the project:
```
python main.py
```
**Note** If running this in Jupyter Notebook system messages, such as those regarding test status, may appear in the terminal rather than the notebook.

### Submission
1. Ensure you've passed all the unit tests.
2. Ensure you pass all points on [the rubric](https://review.udacity.com/#!/rubrics/989/view).
3. Submit the following in a zip file.
 - `helper.py`
 - `main.py`
 - `project_tests.py`
 - Newest inference images from `runs` folder  (**all images from the most recent run**)
 
 ### Tips
- The link for the frozen `VGG16` model is hardcoded into `helper.py`.  The model can be found [here](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/vgg.zip)
- The model is not vanilla `VGG16`, but a fully convolutional version, which already contains the 1x1 convolutions to replace the fully connected layers. Please see this [forum post](https://discussions.udacity.com/t/here-is-some-advice-and-clarifications-about-the-semantic-segmentation-project/403100/8?u=subodh.malgonde) for more information.  A summary of additional points, follow. 
- The original FCN-8s was trained in stages. The authors later uploaded a version that was trained all at once to their GitHub repo.  The version in the GitHub repo has one important difference: The outputs of pooling layers 3 and 4 are scaled before they are fed into the 1x1 convolutions.  As a result, some students have found that the model learns much better with the scaling layers included. The model may not converge substantially faster, but may reach a higher IoU and accuracy. 
- When adding l2-regularization, setting a regularizer in the arguments of the `tf.layers` is not enough. Regularization loss terms must be manually added to your loss function. otherwise regularization is not implemented.
 
### Using GitHub and Creating Effective READMEs
If you are unfamiliar with GitHub , Udacity has a brief [GitHub tutorial](http://blog.udacity.com/2015/06/a-beginners-git-github-tutorial.html) to get you started. Udacity also provides a more detailed free [course on git and GitHub](https://www.udacity.com/course/how-to-use-git-and-github--ud775).

To learn about REAMDE files and Markdown, Udacity provides a free [course on READMEs](https://www.udacity.com/courses/ud777), as well. 

GitHub also provides a [tutorial](https://guides.github.com/features/mastering-markdown/) about creating Markdown files.
