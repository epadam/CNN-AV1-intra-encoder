
Encoding Efficiency Metrics 
==========================================

--------
PSNR
--------

PSNR means peak-to-peak signal-to-noise ratio and is the simplest and most commenly used video quality matric. For each frame in the video, the PSNR is calculated from the following formula:

.. math::
       :label: eq_lin_cost_func
       
       PSNR=10\cdot \log_{10} \frac{MaxErr^2}{MSE},

Where :math:`MSE= \frac{1}{N\cdot M}\sum_{i=1}^{N}{}\sum_{j=1}^{M} {\left(f\left(i,j\right) - f'\left(i,j\right)\right)}^2`.

f' and f represent encoded frame and original frame,  luma and chroma plane can be calculated seperately or together to obtain the average MSE and PSNR.

--------
BD-PSNR
--------

BD-PSNR stands for Bj{\o}ntegaard delta peak signal-to-noise rate. It represents for equivalent bandwidth, the video quality improvement or loss between two video codec or different encoding techniques. It uses a third order logarithmic polyno-mial fitting for each RD curve, and then calculates the difference between the integrals of two fitted RD curves divided by the integration interval.

-------
BD-BR
-------

BD-BR stands for Bj{\o}ntegaard delta bit rate. It means for equivalent quality, the percentage of bit rate savings or loss between two video codec or different encoding techniques. The calculation is similar to BD-PSNR but with some differences.
