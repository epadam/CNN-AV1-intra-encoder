Performance Evaluation
===========================

============================== 
Experimental Configuration
============================== 

==========================================================
Performance of Reference AV1 Encoder Compared with HEVC
==========================================================




=================================================
Evaluation of CNN models with different dataset
=================================================

A medium-sized dataset with 11 frames with single qp is used for evaluating the performance of different models. For 64 and 32 blocks, the image will be down-scaled to 16x16 first. The rest structure of the model remains the same for each model.

datasets for 64 and 32 block are smaller than 16 block.


----------------------------------------------------------------------------
Performance with trimmed dataset (equal number of samples for each class)
----------------------------------------------------------------------------

To avoid a biased training model, the dataset is trimmed so that each class can have equal samples. 

The training result is showing below

The accuracy is low for all the models 

Test on Expanded Model
^^^^^^^^^^^^^^^^^^^^^^^




Test on larger dataset
^^^^^^^^^^^^^^^^^^^^^^^


-------------------------------------
Performance with full dataset 
-------------------------------------

A full dataset is then tested to see the performance 

The distribution of classes is shown in the figure.

It can be seen that the accuray is quite close to the highest distribution of classes.

Training with weighted cross entropy 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Test on Expanded Model
^^^^^^^^^^^^^^^^^^^^^^^


To further inspect the relation between classes. Only two classes are selected to see if the model can tell the difference between classes.


--------------------------------------------
Training results for None and Split classes
--------------------------------------------


-------------------------------------------- 
Training results for Horz and Vert classes
--------------------------------------------




--------------------------------------
Performance with Larger Dataset
--------------------------------------
It can be seen that the accuracy is becoming lower with larger dataset, which may suggest the model is more confused by the dataset.

------------------------------------------------
Dataset from videos with other resolutions
------------------------------------------------

As shown in Figure. For the same frame, at the same qp, lower resolution tend to have finer partition because the scene is more compact. Thus, if the dataset is from 720p videos, the partition modes distribution will tend to split

---------------------------------------------
Comparison between seperate qp and mixed qps
---------------------------------------------

From figure x, it can be seen that qp affect the partition decision tremendously. 


Models trained with single qp (120) and mixed qp data are tested with a test set including one 4K frame, 




====================================
Performance of CNN Intra Encoder
====================================

overhead, complexity reduction, running time
