Performance Evaluation
===============================================

==========================================================
Performance of Reference AV1 Encoder Compared with HEVC
==========================================================



=================================================
Evaluation of CNN models with different dataset
=================================================


A medium-sized dataset with single qp is first tested for different models to select suitable model. For 64 and 32 blocks, the image will be down-scaled to 16x16 first. The rest structure of the model remains the same for each model.

datasets for 64 and 32 block are smaller than 16 block.


----------------------------------------------------------------------------
Performance with trimmed dataset (equal number of samples for each class)
----------------------------------------------------------------------------

To avoid a biased training model, the dataset is trimmed so that each class can have equal samples. 

The training result is showing below

The accuracy is low for all the models 

Test on Expanded Model
^^^^^^^^^^^^^^^^^^^^^^^


-------------------------------------
Performance with full dataset 
-------------------------------------

A full dataset is then tested to see the performance 

The distribution of classes is shown in the figure.

It can be seen that the accuray is quite close to the highest distribution of classes.


Test on Expanded Model
^^^^^^^^^^^^^^^^^^^^^^^


To further inspect the relation between classes. Only two classes are selected to see if the model can tell the difference between classes.

-------------------------------------------- 
Training results for Horz and Vert classes
--------------------------------------------


--------------------------------------------
Training results for None and Split classes
--------------------------------------------


--------------------------------------
Performance with Larger Dataset
--------------------------------------
It can be seen that the accuracy is becoming lower with larger dataset, which may suggest the model is more confused by the dataset.


---------------------------------------------
Comparison between seperate qp and mixed qps
---------------------------------------------

From figure x, it can be seen that qp affect the partition decision tremendously. 


Models trained with single qp (120) and mixed qp data are tested with a test set including one 4K frame, 




====================================
Performance of CNN Intra Encoder
====================================

overhead, complexity reduction, running time
