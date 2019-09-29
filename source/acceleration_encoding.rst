Video Codec Acceleration
======================================
For acceleration of video codec, there are two main solutions. One is hardware acceleration, which offers very low latency, low power consumption and can achieve real time encoding. The downside of the hardware acceleration is the cost and it is less flexible. 

The other solution is software-based acceleration and has more flexibilities. These acceleration solutions include parallelization, statistics based or machine learning based acceleration. Parallelization of encoding algorithms to maximally use multi-core processors or SIMD units is an effective solution for viedo encoding. Statistics or machine learning based acceleration are also becoming more poplular. In the following paragraph, the related works of software-based acceleration are presented.


=======================================
Parallelism
=======================================

Parallelism can be divided into Task level and Data level parallelism. Task level parallelism means assign different functions to different computing unit. Since the different complexity of the functions, distributing the task into different multi-core processors is a challenge [#]_.

For data level parallelism, data can be processed on many units running the same program. It can be further divided into different levels, from Group of Pictures (GOP), frame, tile, block to instruction level. Among all the parallelisms, GOP level offers more flexibility and can preserve higher compressibility. There are already several research using GOP parallelism to improve the encoding speed [#]_. Downside of GOP parallelism is that it would consume a lot of memory. The other strategy is tile level parallelism, which divides each frame into several tiles and encodes in parallel. Finally, block level parallelism is also possible but rarely used, since the communication and synchronization between blocks will consume too much time. Each pixel in the block can also be processed in parallel by instruction level parallelism, also known as SIMD. SIMD is the most effective acceleration solution and is supported by most modern processors. [#]_ have used SIMD for HEVC decoding. 

AV1 supports both tile level parallelism (multi-threading) and instruction level parallelism (SIMD). Since most operation are pixel-wise which can be accelerated by SIMD, most coding functions in AV1 have SIMD support. 

===============================
Statistic Based Approaches
===============================
  
Statistical analysis is another common strategy to accelerate encoding speed. It uses intermediate encoding data (on-line or off-line) to decide if some encoding steps can be skipped. By analyzing these statistical data, signs for early termination of encoding process may be discovered. Thus, unnecessary steps can be avoided. These algorithms need profound understanding of the encoding process. [#]_ have used the relation between R-D costs and variances of pixel motion vectors to early skip the specific inter CUs in HEVC. 


===================================
Machine Learning Based Approaches
===================================

Using machine learning in video codecs already has a long history. Similar to statistical analysis, machine learning can also use intermediate encoding data as input. The major difference is the relation between input information and the output decisions is obtained by training with many data. 

[#]_ has used data mining to build decision trees to decides the best coding tree structure for HEVC. Their results show 
In [#]_, SVM is used for both CU and PU splitting decisions with some selected features including Sum of absolute differences (SAD) between blocks, depth of current block, quantization parameters. 

In recent years, deep learning obtains more attentions due to their impressive performance in many fields. Thus many research groups start to apply deep learning to video coding. Some are targeting acceleration of encoding and is disgussed below.

[#]_ has achieved averagely 65\% reduction of HEVC encoding time under inter mode by using CNN and Long Term Short Term Memory (LSTM). 

[#]_ has used CNN for Quantization prediction.

[#]_ also use CNN for partition classification

[#]_ used DenseNet for Loop filter 

The power of deep learning model is it can learn more situations when given more data which is almost impossible for traditional algorithms to consider all the situations. 


.. [#] M. Yang, J. Tham, S. Rahardja and D. Wu, "Real-time H.264 encoder implementation on a low-power digital signal processor," 2009 IEEE International Conference on Multimedia and Expo, New York, NY, 2009, pp. 1150-1153.

.. [#] Sreeramula, Dr. Sankaraiah & Lam, H.S. & Eswaran, C. & Abdullah, Junaidi. (2011). GOP level parallelism on H.264 video encoder for multicore architecture. International Conference on Circuits, System and Simulation IPCIST. 7. 127-132. 

.. [#] C. C. Chi, M. Alvarez-Mesa, B. Bross, B. Juurlink and T. Schierl, "SIMD Acceleration for HEVC Decoding," in IEEE Transactions on Circuits and Systems for Video Technology, vol. 25, no. 5, pp. 841-855, May 2015.

.. [#] J. Xiong, H. Li, Q. Wu and F. Meng, "A Fast HEVC Inter CU Selection Method Based on Pyramid Motion Divergence," in IEEE Transactions on Multimedia, vol. 16, no. 2, pp. 559-564, Feb. 2014.

.. [#] G. Correa, P. A. Assuncao, L. V. Agostini and L. A. da Silva Cruz, "Fast HEVC Encoding Decisions Using Data Mining," in IEEE Transactions on Circuits and Systems for Video Technology, vol. 25, no. 4, pp. 660-673, April 2015.

.. [#] L. Zhu, Y. Zhang, Z. Pan, R. Wang, S. Kwong and Z. Peng, "Binary and Multi-Class Learning Based Low Complexity Optimization for HEVC Encoding," in IEEE Transactions on Broadcasting, vol. 63, no. 3, pp. 547-561, Sept. 2017.

.. [#] M. Xu, T. Li, Z. Wang, X. Deng, R. Yang and Z. Guan, "Reducing Complexity of HEVC: A Deep Learning Approach," in IEEE Transactions on Image Processing, vol. 27, no. 10, pp. 5044-5059, Oct. 2018.

.. [#] Nguyen Canh, Thuong & Xu, Motong & Jeon, Byeungwoo. (2018). Rate-Distortion Optimized Quantization: A Deep Learning Approach. 

.. [#] Y. Zhang, G. Wang, R. Tian, M. Xu and C. C. J. Kuo, "Texture-Classification Accelerated CNN Scheme for Fast Intra CU Partition in HEVC," 2019 Data Compression Conference (DCC), Snowbird, UT, USA, 2019, pp. 241-249.

.. [#] T. Li, M. Xu, R. Yang and X. Tao, "A DenseNet Based Approach for Multi-frame In-loop Filter in HEVC," 2019 Data Compression Conference (DCC), Snowbird, UT, USA, 2019, pp. 270-279.


.. [#] M. Xu, T. Li, Z. Wang, X. Deng, R. Yang and Z. Guan, "Reducing Complexity of HEVC: A Deep Learning Approach," in IEEE Transactions on Image Processing, vol. 27, no. 10, pp. 5044-5059, Oct. 2018.





