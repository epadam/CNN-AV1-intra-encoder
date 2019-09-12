
Motivation
==============================
According to Cisco's prediction (Visual Networking Index), over 83% of the internet traffic will be multimedia content. This is mainly due to the increasing demand of high quality videos such as 4K/8K footage. Besides, more and more applications including AR/VR videos, self-driving car or live stream gaming generate and deliver huge amount of videos to users . Thus, decreasing the bandwidth consumption of videos while maintaining the quality is always a challenge. To save storage space and ease the internet traffic, video codec with higher compressitivity is always required. However, the cost of higher encoding efficiency is normally in proportion to longer encoding time and more energy consumption. Reducing the encoding time while still maintaining the quality and compression efficiency is the main goal in this research.

Among all the video codecs on the market, `AV1 <https://aomedia.org/>`_, as an open source and royalty-free video codec, is chosen in this work because of its higher encoding efficiency. 
However, AV1 achieves higher efficiency by introducing more encoding tools which leads to very long encoding time. Even though AV1 has been implemented with many accelerating functions, including some machine learning approaches, the encoding speed is still falling behind its competitors. Thus, exploring other acceleration strategies is still desired. 

Among all the new acceleration strategies, convolutional neural network (CNN) has been proved to have superior performance in many fields such as image recognition or natural language processing. Many researches have also shown CNN can improve the encoding speed with minor encoding efficiency loss. Inspired by these researches, incorporating CNN into AV1 to improve the encoding speed is the main goal in this work.

