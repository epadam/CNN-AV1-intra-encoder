Conclusion and Future Perspective
===================================

=============
Conclusion 
=============

In this work, a CNN model is used to predict partition modes for AV1 to accelerate the encoding speed under intra mode.

The results show it can greatly save encoding time by 60%.

====================
Future Perspective
====================

To be able to improve the prediction, pixels of neighbor block may also need to be considered. 

As it can be seen one third of the time is wasting on communication between c and python. Further speed improvement can be achieved by porting keras model into C/C++ and integrate into encoder.

Test it on modern SoC with dedicate AI and Video Encoding chip will be interesting 

No matter under the traditional encoding structure or innovative structure, deep learning based solutions are challenging the traditional coding algorithms and start to show some achievements. In the near future, it will play more and more important roles in the video codec.
