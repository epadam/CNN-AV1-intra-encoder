
CNN for Partition Decisions for Intra Frame
==============================================

As mentioned in chapter \ref{Related Work}, there are many ways of using CNN to replace some encoding steps in the encoder as shown in Fig \ref{fig:CNN for encoder}. However, applying CNN for partition may save the most encoding time compared to other steps, since prediction and transform functions are sub-functions of partition functions. Replacing partition function means skipping the whole RDO process, which includes prediction, transform, quantization, dequantization and inverse transform and entropy coding. classification on CU splitting decision making can maximally save the encoding time compared to classification for PU and TU. 

And according to the results of previous researches, using CNN model to predict the partition decisions for intra frames shows better performance than other solutions. It may because that, unlike inter frames, intra frame doesn't need information from other frames. Although encoding intra frames requires pixel values from adjacent blocks, the contents of each image block also contain crucial information for selecting the partition mode. 
And since CNN is very powerful in image recognition, it is very suitable to decide block splitting. For example, flat area can be coded in bigger block, so the better partition decision is not to split. CNN may connect these features with the final partition decisions through training large amount of data.


.. image:: img/CNN_for_partition.png



================================================
Brief Theory of Convolutional Neural Network
================================================


Convolutional Neural Network(CNN) is the most popular branch of Deep Learning. Unlike other neural network, not all the layers in CNN are fully connected. The basic structured is described as follows.

\subsection{Basic structure}

The basic structure of CNN can be seen in Figure XX.

\paragraph{Convolutional Layer}
The most difference between normal neural network and CNN is the convolutional layer, which use kernel map or filter to extract the features from raw data. Figure xx gives the simple demonstration of how filter works. 

\paragraph{Pooling Layer}
Sometimes, to reduce the spatial size of input data, pooling 

\paragraph{Fully Connected Layer}

Fully connected layer works just like other neural network. 

\subsection{Activation Functions}

Normally, the activation function is applied to the output of each layer. The reason is introduce nonlinearity to the network, and make it possible to approximate any nonlinear function.

The most commen used activation functions include sigmoid, Relu and softmax. The details description is as follows.


\paragraph{ReLu (rectified linear unit)/leaky ReLu}


================================================
CNN Model in This Research
================================================
