
Encoder and Decoder
===================================
Most modern video codecs have similar structure and the encoding process is block-based. Each frame of the input video is normally  sequentially encoded starting from the block in the upper left corner. And each block will try one prediction mode as a first step, as shown in Figure \ref{fig:encoding process}. Intra prediction uses edge pixels of adjacent block (left and above) for prediction and inter prediction searches similar pattern in the same frame or encoded frame called reference frame for prediction. For intra or key frame, only intra prediction is possible and inter frame will try both inter and intra predictions. The pixels in the original block will substract the one in the prediction block built by predictor to obtain the differences called residuals. And step 2, the residuals are transformed from space domain to freqeuncy domain to obtain transformed coefficients. 


Step 3, the quantizer will eliminate the high frequency informations of the block, since human eyes are less sensitive to these informations. However, this step will cause irreversible quality loss of the original image. Then the quantized coefficients and other parameters of this prediction mode are encoded with entropy coding to generate a real bitstream and obtain the bit rate cost, as step 4. 


The function of entropy coding is to use symbols to represent certain length of bitstream, which can further compress the bitstream. On the other hand, to obtain the real distortion of the block, the quantized coefficients are processed with inverse quantization and inverse transform as step 5 and 6. 

And step 7 is to caculate the RD cost. Step 1 to 7 will be repeated for all prediction modes to find the best RD cost.  This process is often called rate-distortion optimization (RDO). The prediction mode with lowest distortion and bit rate will be selected and the bitstream generated in step 4 will be used in the final output bitstream. However, the block can be splitted into smaller blocks and the steps above will be repeated for each subblock again. If the sum of the RD cost of the subblocks is smaller, then this splitting and prediction modes will be encoded in the final output bitstream.

And finally, after a frame or tile is encoded, loop/deblock filter is used to remove the ringing effect due to the block based encoding and then store it as a reference frame for inter prediction of next frame. Loop filtering can in loop or out of loop, depending on that if the filtered frame is used as the reference frame later.  

This encoding process is repeated for every block in every frame of input raw video until all the frames are encoded.


.. image:: img/EncodingProcess.png


Figure \ref {fig:decoding process} shows the general decoding process. It can be easily realized that decoding process is just part of the encoding process. The bitstream will first be unpacked and entropy decoded. The restored residuals are obtained through inverse quantization and inverse transform. In encoding process, these steps are required to obtain the real distortion for RD cost calculation. On the other hand, the prediction mode informations are also parsed to build the prediction block followed by the addition of the residuals to restore the block. Finally, like in the encoding process, loop/deblocking filter is used to remove the ringing effect. The decoded frames are output as part of the video and also used for decoding next frames.

.. image:: img/DecodingProcess.png
