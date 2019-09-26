Video Codec Acceleration
======================================
For acceleration of video codec, there are two main solutions. One is hardware acceleration, which offers very low latency, low power consumption and can achieve real time encoding. The downside of the hardware acceleration is the cost and it is less flexible. 

The other solution is software-based acceleration and has more flexibilities. These acceleration solutions include parallelization, statistics based or machine learning based acceleration. Parallelization of encoding algorithms to maximally use multi-core processors or SIMD units is an effective solution for viedo encoding. Statistics or machine learning based acceleration are also becoming more poplular. In the following paragraph, the related works of software-based acceleration are presented.


=======================================
Parallelism
=======================================

Parallelism can be divided into Task level and Data level parallelism. Task level parallelism means assign different functions to different computing unit. Since the different complexity of the functions, distributing the task into different multi-core processors is a challenge _[#].

For data level parallelism, data can be processed on many units running the same program. It can be further divided into different levels, from Group of Pictures (GOP), frame, tile, block to instruction level. Among all the parallelisms, GOP level offers more flexibility and can preserve higher compressibility. There are already several research using GOP parallelism to improve the encoding speed _[#]. Downside of GOP parallelism is that it would consume a lot of memory. The other strategy is tile level parallelism, which divides each frame into several tiles and encodes in parallel. Finally, block level parallelism is also possible but rarely used, since the communication and synchronization between blocks will consume too much time. Data in each block can also be processed in parallel by instruction level parallelism, also known as SIMD. SIMD is the most important and effective acceleration solutions and is supported by most modern processors _[#]. 

In AV1, it supports tile level parallelism (multi-threading) and instruction level parallelism (SIMD). Since most operation are pixel-wise which can be accelerated by SIMD, most coding functions in AV1 have SIMD support. 


===============================
Statistic Based Approaches
===============================
  
The idea of using statistics is to discover the some signs for early termination so unnecessary computation can be avoided. Normally these algorithms collect intermediate encoding data on-line or off-line and use them to prune impossible encoding modes or steps. 

Many researchers try to use statistic analysis to eliminate the less possible splitting or prediction modes, thus can terminate the recursive process earlier _[#]. This strategy is based more explicit equations designed by human beings.

===================================
Machine Learning Based Approaches
===================================

Using machine learning in video codecs already has a long history. 

_[#]
_[#] used SVM for both CU and PU splitting decisions with some selected features including Sum of absolute differences (SAD) between blocks, depth of current block, quantization parameters. 

In recent years, deep learning obtains more attentions due to their impressive performance in many fields.

There are many researches trying to use CNN to classify CU splitting. _[#] achieve averagely 65\% reduction of HEVC encoding time under inter mode by using CNN and Long Term Short Term Memory (LSTM). 

(Rate-Distortion Optimized Quantization: A Deep Learning Approach) used CNN for prediction of Quantization.

(Texture-Classification Accelerated CNN Scheme for Fast Intra CU Partition in HEVC) also use CNN for partition classification

A DenseNet Based Approach for Multi-Frame In-Loop Filter in HEVC used DenseNet for Loop filter 


.. [#] M. Yang, J. Tham, S. Rahardja and D. Wu, "Real-time H.264 encoder implementation on a low-power digital signal processor," 2009 IEEE International Conference on Multimedia and Expo, New York, NY, 2009, pp. 1150-1153.

.. [#] Sreeramula, Dr. Sankaraiah & Lam, H.S. & Eswaran, C. & Abdullah, Junaidi. (2011). GOP level parallelism on H.264 video encoder for multicore architecture. International Conference on Circuits, System and Simulation IPCIST. 7. 127-132. 

.. [#] C. C. Chi, M. Alvarez-Mesa, B. Bross, B. Juurlink and T. Schierl, "SIMD Acceleration for HEVC Decoding," in IEEE Transactions on Circuits and Systems for Video Technology, vol. 25, no. 5, pp. 841-855, May 2015.

.. [#] J. Xiong, H. Li, Q. Wu and F. Meng, "A Fast HEVC Inter CU Selection Method Based on Pyramid Motion Divergence," in IEEE Transactions on Multimedia, vol. 16, no. 2, pp. 559-564, Feb. 2014.

.. [#] G. Correa, P. Assuncao, L. A. da Silva Cruz and L. Agostini, "Classification-based early termination for coding tree structure decision in HEVC," 2014 21st IEEE International Conference on Electronics, Circuits and Systems (ICECS), Marseille, 2014, pp. 239-242.

.. [#] L. Zhu, Y. Zhang, Z. Pan, R. Wang, S. Kwong and Z. Peng, "Binary and Multi-Class Learning Based Low Complexity Optimization for HEVC Encoding," in IEEE Transactions on Broadcasting, vol. 63, no. 3, pp. 547-561, Sept. 2017.

.. [#] M. Xu, T. Li, Z. Wang, X. Deng, R. Yang and Z. Guan, "Reducing Complexity of HEVC: A Deep Learning Approach," in IEEE Transactions on Image Processing, vol. 27, no. 10, pp. 5044-5059, Oct. 2018.





