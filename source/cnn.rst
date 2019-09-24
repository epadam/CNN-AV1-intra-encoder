Convolutional Neural Network
===================================================


Convolutional Neural Network(CNN) is the most popular branch of Deep Learning. The most difference between normal neural network and CNN is the convolutional layer. 


The explanation of each layer is presented as follows.

---------------------
Convolutional Layer
---------------------

Convolutional layer uses kernel map or filter to extract the features from raw data. 

The operation is defined as:

.. math::

  (I \ast K)_{ij} &= \sum_{m = 0}^{k_1 - 1} \sum_{n = 0}^{k_2 - 1} I(i-m, j-n)K(m,n)

-------------------
Pooling Layer
-------------------

Sometimes, to reduce the spatial size of input data, pooling 

----------------------
Fully Connected Layer
----------------------

Fully connected layer works just like other neural network. 


----------------------
Activation Function
----------------------

Normally, the activation function is applied to the output of each layer. The reason is introduce nonlinearity to the network, and make it possible to approximate any nonlinear function.

The most commen used activation functions include sigmoid, Relu and softmax. The details description is as follows.

ReLu (rectified linear unit)/leaky ReLu}

---------------------
Training CNN Model
---------------------



Loss (Error) Function
---------------------


Backpropagation
----------------------

Many optimization algothrims for backpropagation are proposed 
