
CNN for Partition Decisions for Intra Frame
======================================

As mentioned in chapter \ref{Related Work}, there are many ways of using CNN to replace some encoding steps in the encoder as shown in Fig \ref{fig:CNN for encoder}. However, applying CNN for partition may save the most encoding time compared to other steps, since prediction and transform functions are sub-functions of partition functions. Replacing partition function means skipping the whole RDO process, which includes prediction, transform, quantization, dequantization and inverse transform and entropy coding. classification on CU splitting decision making can maximally save the encoding time compared to classification for PU and TU. 

And according to the results of previous researches, using CNN model to predict the partition decisions for intra frames shows better performance than other solutions. It may because that, unlike inter frames, intra frame doesn't need information from other frames. Although encoding intra frames requires pixel values from adjacent blocks, the contents of each image block also contain crucial information for selecting the partition mode. 
And since CNN is very powerful in image recognition, it is very suitable to decide block splitting. For example, flat area can be coded in bigger block, so the better partition decision is not to split. CNN may connect these features with the final partition decisions through training large amount of data.


.. image:: img/CNN_for_partition.png
