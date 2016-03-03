# Use tensorflow based CNN to detect facial keypoints

If you don’t have a GPU, you can install a CPU only version of tensowflow, and the tutorial can be found [here](https://www.tensorflow.org/versions/r0.7/get_started/os_setup.html#pip-installation). 
   
This network contains 2 convolutional layers and 2 fully connected layers, and the output layer is linear. Each convolutional layer is followed by a max-pooling layer, and use relu as activation function.

The loss of this system is squared errors(l2 loss), and use AdamOptimizer to minimize the loss. The doc of AdamOptimizer can be found [here](https://github.com/jikexueyuanwiki/tensorflow-zh/blob/master/SOURCE/api_docs/python/train.md#AdamOptimizer).  
   
When I use GradientDescentOptimizer and MomentumOptimizer, parameters can’t converge, do you ever meet similar problem?

I plot learning curve of the system(not typical learning curve, but training/validation loss change with epochs), and see a sign of overfitting: training loss keep dropping while validation loss not. So I add some regularization, including l2 regularization, drop out, and early stopping. 

Now the learning curve look like this: 

![this](https://github.com/saber1988/facial-keypoints-detection/blob/master/learning_curve1.png) 

See that there is a big gap between train loss and validation loss, for the reason of dropout and regularization?

I make a comparison of different parameters: 
  
batch size | initial learning rate | early stop patience | validation size | l2 regularization coeffient | learning rate decay rate | optimizer | best loss
---|---|---|---|---|---|---|---
64 | 1.00E-03 | 50 | 100 | 2.00E-07 | 0.95 | AdamOptimizer | 0.015985    
64 | 5.00E-04 | 50 | 100 | 2.00E-07 | 0.98 | AdamOptimizer | 0.016929  
64 | 5.00E-04 | 50 | 100 | 2.00E-07 | 0.99 | AdamOptimizer | 0.021319  

Some other parameter combinations are not listed here. Some things to pay attention to are:  
 
* The initial learning rate should not be too large, or the system will fail to converge   
* Large batch size will generate more stable result, but large batch size will consume more memory   
* Learning rate decay rate should not be too small, or the step will shrink too fast, and it's hard for the loss to get to minimum value   

Now the final score is not good, a lot can be down to promote it, like train one model for each keypoint, deal with null value, and flip the image   

Welcome to communicate
shi.dou8@gmail.com






