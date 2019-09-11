Performance Evaluation
===========================


================================
Experimental Configuration
================================

The video encoding time is measured on Intel NUC

==========================================================
Performance of Reference AV1 Encoder Compared with HEVC
==========================================================

Different videos are encoded with both AV1 and HEVC to compare the encoding efficiency 

=================================================
Evaluation of CNN models with different dataset
=================================================


In the following sections, the dataset with 11 frames with single qp is used for evaluating the performance of different models. All 11 frames are from different videos. For 64 and 32 blocks, the image will be down-scaled to 16x16 first. The rest structure of the model remains the same for each model.

datasets for 64 and 32 block are smaller than 16 block.

The distribution of partition modes of different block sizes is shown below. 


.. image:: img/4K_11f_mix_distribution_64.jpg
   :width: 49%  
.. image:: img/4K_11f_mix_distribution_32.jpg
   :width: 49%


.. image:: img/4K_11f_mix_distribution_16.jpg
   :width: 50%
   
   
----------------------------------------------------------------------------
Performance with trimmed dataset (equal number of samples for each class)
----------------------------------------------------------------------------

To avoid a biased training model, the dataset is trimmed so that each class can have equal samples. 

The training result for block size 16x16, 32x32, 64x64 is shown below:

model1

64

.. image:: img/m1_qp120_64_acc_ecf.jpg
   :width: 49%
.. image:: img/m1_qp120_64_loss_ecf.jpg
   :width: 49%


32

.. image:: img/m1_qp120_32_acc_ecf.jpg
   :width: 49%
.. image:: img/m1_qp120_32_loss_ecf.jpg
   :width: 49%

16

.. image:: img/m1_qp120_16_acc_ecf.jpg
   :width: 49%
.. image:: img/m1_qp120_16_loss_ecf.jpg
   :width: 49%

..........

model2

64

.. image:: img/mnist_qp120_64_acc_ecf.jpg
   :width: 49%
.. image:: img/mnist_qp120_64_loss_ecf.jpg
   :width: 49%

32

.. image:: img/mnist_qp120_32_acc_ecf.jpg
   :width: 49%
.. image:: img/mnist_qp120_32_loss_ecf.jpg
   :width: 49%

16


.. image:: img/mnist_qp120_16_acc_ecf.jpg
   :width: 49%
.. image:: img/mnist_qp120_16_loss_ecf.jpg
   :width: 49%



The accuracy is low for all the models 

Test on Expanded Model
^^^^^^^^^^^^^^^^^^^^^^^



-------------------------------------
Performance with full dataset 
-------------------------------------

A full dataset is then tested to see the performance 

The distribution of classes is shown in the figure.

It can be seen that the accuray is quite close to the highest distribution of classes.

The training result with full dataset for block size 16x16, 32x32, 64x64 is shown below:


model1

64

.. image:: img/m1_qp120_64_acc_f.jpg
   :width: 49%
.. image:: img/m1_qp120_64_loss_f.jpg
   :width: 49%

32

.. image:: img/m1_qp120_32_acc_sh.jpg
   :width: 49%
.. image:: img/m1_qp120_32_loss_sh.jpg
   :width: 49%

16

.. image:: img/m1_qp120_16_acc_f.jpg
   :width: 49%
.. image:: img/m1_qp120_16_loss_f.jpg
   :width: 49%

model2

64

.. image:: img/mnist_qp120_64_acc_f.jpg
   :width: 49%
.. image:: img/mnist_qp120_64_loss_f.jpg
   :width: 49%

32

.. image:: img/mnist_qp120_32_acc_sh.jpg
   :width: 49%
.. image:: img/mnist_qp120_32_loss_sh.jpg
   :width: 49%

16

.. image:: img/mnist_qp120_16_acc_f.jpg
   :width: 49%
.. image:: img/mnist_qp120_16_loss_f.jpg
   :width: 49%

Training with weighted cross entropy 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

model1

64

.. image:: img/m1_qp120_64_acc_fw.jpg
    :width: 49%
.. image:: img/m1_qp120_64_loss_fw.jpg
    :width: 49%

32

.. image:: img/m1_qp120_32_acc_fw.jpg
    :width: 49%
.. image:: img/m1_qp120_32_loss_fw.jpg
    :width: 49%

16

.. image:: img/m1_qp120_16_acc_fw.jpg
    :width: 49%
.. image:: img/m1_qp120_16_loss_fw.jpg
    :width: 49%

model2

64

.. image:: img/mnist_qp120_64_acc_fw.jpg
    :width: 49%
.. image:: img/mnist_qp120_64_loss_fw.jpg
    :width: 49%

32

.. image:: img/mnist_qp120_32_acc_fw.jpg
    :width: 49%
.. image:: img/mnist_qp120_32_loss_fw.jpg
    :width: 49%

16

.. image:: img/mnist_qp120_32_acc_fw.jpg
    :width: 49%
.. image:: img/mnist_qp120_32_loss_fw.jpg
    :width: 49%


Test on Expanded Model
^^^^^^^^^^^^^^^^^^^^^^^


--------------------------------------
Performance with Larger Dataset
--------------------------------------
We further use dataset mixed with data from different resolution.

datasets from videos with other resolution
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It can be seen in the figure, that videos with different resolution have slightly different partition mode distributions. For lower resolution videos, there is higher chance to be encoded in smaller blocks since the contents of the video is more compact. 

Videos with higher resolution like 4K videos, on the other hand, will have more smooth area that can be encoded with larger blocks.




It can be seen that the accuracy is becoming lower with larger dataset, which may suggest the model is more confused by the dataset.


To further inspect the relation between classes. Only two classes with equal number of samples are selected to see if the model can tell the difference between classes. 

------------------------------------------------------------
Training results of None and Split partition modes only
------------------------------------------------------------

It can be seen that the accuracy can reach around 90% for both models when it is only trained with NONE and SPLIT partition modees.

model1

64

.. image:: img/m1_qp120_64_acc_NS.jpg
    :width: 49%
.. image:: img/m1_qp120_64_loss_NS.jpg
    :width: 49%

32

.. image:: img/m1_qp120_32_acc_NS.jpg
    :width: 49%
.. image:: img/m1_qp120_32_loss_NS.jpg
    :width: 49%

16

.. image:: img/m1_qp120_16_acc_NS.jpg
    :width: 49%
.. image:: img/m1_qp120_16_loss_NS.jpg
    :width: 49%

model2

64

.. image:: img/mnist_qp120_64_acc_NS.jpg
    :width: 49%
.. image:: img/mnist_qp120_64_loss_NS.jpg
    :width: 49%

32

.. image:: img/mnist_qp120_32_acc_NS.jpg
    :width: 49%
.. image:: img/mnist_qp120_32_loss_NS.jpg
    :width: 49%


16

.. image:: img/mnist_qp120_16_acc_NS.jpg
    :width: 49%
.. image:: img/mnist_qp120_16_loss_NS.jpg
    :width: 49%

--------------------------------------------------------  
Training results of Horz and Vert partition modes only
-------------------------------------------------------- 

model1

64

.. image:: img/m1_qp120_64_acc_HV.jpg
    :width: 49%
.. image:: img/m1_qp120_64_loss_HV.jpg
    :width: 49%

32

.. image:: img/m1_qp120_32_acc_HV.jpg
    :width: 49%
.. image:: img/m1_qp120_32_loss_HV.jpg
    :width: 49%
    
16

.. image:: img/m1_qp120_16_acc_HV.jpg
    :width: 49%
.. image:: img/m1_qp120_16_loss_HV.jpg
    :width: 49%

model2

64

.. image:: img/mnist_qp120_64_acc_HV.jpg
    :width: 49%
.. image:: img/mnist_qp120_64_loss_HV.jpg
    :width: 49%

32

.. image:: img/mnist_qp120_32_acc_HV.jpg
    :width: 49%
.. image:: img/mnist_qp120_32_loss_HV.jpg
    :width: 49%

16

.. image:: img/mnist_qp120_16_acc_HV.jpg
    :width: 49%
.. image:: img/mnist_qp120_16_loss_HV.jpg
    :width: 49%

However, it can only reach 60% for the Horz and Vert datasets.


From the tests above, it can be seen that the model can not really learn the features of some classes. The reason is  


You can check the following jupyter notebook to see to see the partition modes of the dataset.  


--------------------------------------------------------  
Training results of three partition modes
--------------------------------------------------------


---------------------------------------------
Comparison between seperate qp and mixed qps
---------------------------------------------

From figure x, it can be seen that qp affect the partition decision tremendously. 

.. image:: img/library64.jpg
    :width: 49%
.. image:: img/library32.jpg
    :width: 49%

.. image:: img/library16.jpg
    :width: 49%

Models trained with single qp (120) and mixed qp data are tested with a test set including one 4K frame, 




====================================
Performance of CNN Intra Encoder
====================================



---------------------------------------------
Encoding Performance
---------------------------------------------

Comparison of Encoding Time
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Overhead


Comparison of Video Quality
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
models trained with different dataset is used to test the encoding efficiency

trimmed dataset

full dataset

full dataset with weighted cross entropy

Encoder preformance between seperate qp and mixed qps

