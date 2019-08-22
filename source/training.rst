
Training CNN Model
======================================

========================================
Labels Extraction from the Encoder
========================================

---------------
Encoding Mode
---------------

AV1 provides 4 different rate control modes, including Variable Bit Rate (VBR), Constant Bit Rate (CBR), Constrained Quality (CQ), Constant Quality (Q) modes. 
Constant quality means
Under VBR modes, the bit rate for each frame will be variate according to the frame contents. Under CBR mode, each frame will be encoded with the same bit rate. For these two modes. the quantization parameters (QP) will be selected based on preset's bit rate. For CQ modes, QP will be set in certain range. For Q modes, a constant QP will be applied. 

-----------------------
Code change in Encoder
-----------------------


The partition decisions of the original encoder are collected processed as follows:
1. For each frame, when it goes to the function ''rd\_pcik\_partition'', the partition decisions of each 64x64, 32x32 and 16x16 blocks are recorded and stored in a txt file. Partition decisions are indexed from 0 to 9 as labels for each block. The qp of the frame is also stored.

-----------------------
Data Processing
-----------------------

2. The txt file is further processed with excel to delete repeated decisions and store partition decisions of different block in different sheets.
3. Based on the excel file, Partition decisions (labels) and qp are stored in separate txt files.
4. The raw frame is reordered in block-based and stored in a txt file. Since some blocks are skipped during the encoding process, these blocks are deleted.
These three txt files (reordered raw images, labels, qps) are all in the same order. 

========================================
Training and Evaluation Setting
========================================

--------------
Loss Function
--------------

--------------
Optimizer
--------------


----------------------------
Evaluation and Test Setting
----------------------------
