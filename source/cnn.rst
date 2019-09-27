Convolutional Neural Network
==================================


Convolutional Neural Network(CNN) is the most popular branch of Deep Learning. The most difference between normal neural network and CNN is the convolutional layer. 

--------------------------
General Structure of CNN
--------------------------

The explanation of each layer is presented as follows.

Convolutional Layer
---------------------

Convolutional layer uses kernel map or filter to extract the features from the input data. 

For an input image I, the kernal map K with dimensions :math:`k_1×k_2` applies operation 

The general operation is defined as:

.. math::

  (I ∗ K)_{ij}= \sum_{m = 0}^{k_1 - 1} \sum_{n = 0}^{k_2 - 1} I(i-m, j-n)K(m,n)


Pooling Layer
-------------------

Pooling layer is used to reduce the spatial size of input data.

The operation is defined as follows:

.. math::

  (I \ast K)_{ij} &= \sum_{m = 0}^{k_1 - 1} \sum_{n = 0}^{k_2 - 1} I(i-m, j-n)K(m,n)


Fully Connected Layer
----------------------

Fully connected layer works just like normal neural network. 

.. math::

  (I \ast K)_{ij} &= \sum_{m = 0}^{k_1 - 1} \sum_{n = 0}^{k_2 - 1} I(i-m, j-n)K(m,n)


Activation Function
----------------------

Normally, the activation function is applied to the output of each layer. The reason is to introduce nonlinearity to the network, and make it possible to approximate any nonlinear function.

The most commen activation functions include sigmoid, Relu and softmax, which is shown below.




ReLu (rectified linear unit)/leaky ReLu}

---------------------
Training CNN Model
---------------------

Neural network learns from the gradient 


Loss (Error) Function
---------------------

Loss function gives


Backpropagation
-----------------

Backpropagation is the esense of neural network. It passes the gradient based on the loss function. The way it works is to try to achieve the minimum of loss function.

Many minimum searching algorithms are proposed. 


Regularization
-----------------
