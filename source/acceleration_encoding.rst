Acceleration Strategies of Video Codec
======================================
In this section we briefly review the tools and strategies used to accelerate encoding speed.

For acceleration of video codec, there are two major domains. One is pure hardware solution like ASIC or FPGA, which normally offer very low latency and low power consumptions. AISC normally has higher performance, but is for market with high volume demands like mobile. FPGA on the other hand is suitable for low volume market. Today, most mobile devices and PC have dedicated hardware for real time video encoding.

However, compared to dedicated hardware, software solutions offer more flexibility and also have very low cost. Thanks to the improvement of general purpose CPU, multi-threading or SIMD, software encoder can also have competitive encoding speed. Thus, many researches focus on the parallelization of the algorithm to maximally use modern multi-core CPU or SIMD hardware. Besides parallelism, simplified or other accelerating algorithms based on statistics or machine learning strategies are also the interest of many research groups. In the following paragraph, related works are presented.


\subsection{Parallelism}
Parallelism can be divided into Task level and Data level parallelism. Task level parallelism means assign different functions to computing unit and is more achievable through hardware design, since the different complexity of the functions, distributing the task into different multi-core processors is a challenge\cite{Yang2009}.

For data level parallelism, data can be processed on many units running same program. It can be further divided into different levels, from GOP(Group of Pictures), frame level, tile, block to instruction level. Among all the parallelisms, GOP level offers more flexibility and can preserve higher compressibility. There are already several research using GOP parallelism to improve the encoding speed \cite{Sankaraiah,Bahri2014}. Downside of GOP parallelism is that it would consume a lot of memory. And for higher resolution, it is very difficult for GOP to reach real time performance due to huge amount of data in one GOP. The other strategy is tile level parallelism, which means dividing each frame into several tiles and encoding in parallel. Finally, block level parallelism is also possible but rarely used, since the communication and synchronization between blocks will consume too much time.

Data in the block can also be processed in parallel by instruction level parallelism, which is also called SIMD. SIMD is often encoding process since it is supported by most modern processors \cite{Chi2015}.

\subsection{Simplified Algorithm}
Besides maximizing parallelism among all data level (GOP, Frame, Tiles, Blocks), the other main strategy is to lower the complexity of encoder algorithm itself.
Rate-Distortion cost calculation needs to go through transform/quantization and inverse quantization/inverse transform and these recursive steps account for most of encoding time. Some researches use simplified RD cost calculation such as absolute transformed differences (SATD) to reduce the complexity\cite{Yu-MingLee2010}. Normally These strategies are not related to the contents of input video.




\subsection{Statistic based approaches}
The idea of using statistics is to discover the some signs for early termination so unnecessary computation can be avoided. Normally these algorithms collect intermediate encoding data on-line or off-line and use them to prune impossible encoding modes or steps. 

Many researchers try to use statistic analysis to eliminate the less possible splitting or prediction modes, thus can terminate the recursive process earlier \cite{Xiong2014}. This strategy is based more explicit equations designed by human beings.



\subsection{Machine Learning based approaches}\label{ML}

Using machine learning in video codecs already has a long history. Some solutions target the reductions of bit-rate, some focus on increasing the quality, and some aims at reducing encoding time.

Like statistic based solutions, machine learning can use human selected features but also raw data as input. However, unlike statistic based methods, these models are less interpretable. The model's ability to make inferences comes from the result of training with large amount of data. These machine learning algorithms include data mining, decision tree, Beysian Decision, Neural Network, Support Vector Machine(SVM)and Reinforcement Learning. These algorithms can be applied to different steps in the encoder.

\cite{Correa2014} 
\cite{Zhu2017} used SVM for both CU and PU splitting decisions with some selected features including Sum of absolute differences (SAD) between blocks, depth of current block, quantization parameters. 

In recent years, deep learning obtains more attentions due to their impressive performance in many fields.

There are many researches trying to use CNN to classify CU splitting.
\cite{Xu2018b} achieve averagely 65\% reduction of HEVC encoding time under inter mode by using CNN and Long Term Short Term Memory (LSTM). 

(Rate-Distortion Optimized Quantization: A Deep Learning Approach) used CNN for prediction of Quantization.

(Texture-Classification Accelerated CNN Scheme for Fast Intra CU Partition in HEVC) also use CNN for partition classification

A DenseNet Based Approach for Multi-Frame In-Loop Filter in HEVC used DenseNet for Loop filter 
