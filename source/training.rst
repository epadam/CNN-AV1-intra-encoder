
Training CNN Model
========================

========================================
Labels Extraction from the Encoder
========================================

-----------------------
Video Source
-----------------------

Videos with different resolutions are downloaded from the following websites:

http://medialab.sjtu.edu.cn/web4k/index.html

https://media.xiph.org/video/derf/

The list of video can be seen in the reference. 

---------------
Encoding Mode
---------------

AV1 offers 4 different rate control modes, including Variable Bit Rate (VBR), Constant Bit Rate (CBR), Constrained Quality (CQ) and Constant Quality (Q) mode. Under VBR modes, the bit rate for each frame will be variate according to the frame contents. Under CBR mode, each frame will be encoded with the same bit rate. For these two modes, the quantization parameters (QP) will be selected based on the input bit rate. For CQ mode, QP is set in the required range. For Q mode, a fixed QP is used to encode the whole video. Q mode is used to collect training data in this research, which allows us to encode videos with specified QP.
The example encoding command is shown as follows::

  ./aomenc -o video.ivf --end-usage=q --cq-level=20 --limit=1  -w 3840 -h 2160 video.yuv

--------------------------
Partition Mode Extraction
--------------------------

In the function ''rd_pcik_partition'', the partition decisions of each 64x64, 32x32 and 16x16 blocks are recorded and saved in a txt file. Partition modes are treated as labels and indexed from 0 to 9.

code

-----------------------
Data Processing
-----------------------

The txt file is further processed to remove the repeated partition modes from the re-encoding loop. Partition modes of different block sizes are saved seperately. Labels and QPs are then read from the processed file and saved in separate txt files. The raw pixels of each frame are reordered in a block-wise fashion. Blocks without labels are also removed from the file. 
The data set includes three txt files (reordered raw images, labels, QPs). All three files are in the same order. 

code

=================================== 
Training and Evaluation Setting
=================================== 

CNN models are built in keras with tensorflow as backend.

--------------
Loss Function
--------------

The built-in loss functions including in keras are used for the training. Both categorical cross entropy and binary cross entropy are used as loss function in this research.

focal loss is also tested.

--------------
Optimizer
--------------

Built-in Adam is used.


----------------------------
Evaluation and Test Setting
----------------------------

10% of the data set is split for evaluation. 
