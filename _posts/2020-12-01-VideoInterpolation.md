---
title: "Implementing and Extending SuperSloMo (CVPR 2018)"
date: 2020-12-01
permalink: /projects/video-interpolation/

tags:
  - December
---

This is a snapshot of a portion of the work done as part of my MS project back in 2019 at UMass. While the larger project was focused on self-supervised representation learning, a secondary goal was to extend and improve the performance of a state of the art video interpolation model by exploiting rich spatio-temporal cues in a video.

The code is on [github!](https://github.com/SreenivasVRao/SuperSloMo-VideoInterpolation-PyTorch)

Here's a quick preview example using a video from the DAVIS dataset. The original video is 30FPS, and using a modified version of the neural net architecture from [SuperSloMo (CVPR 2018)](https://openaccess.thecvf.com/content_cvpr_2018/papers/Jiang_Super_SloMo_High_CVPR_2018_paper.pdf), I'm able to slow down the footage 8x.

<video style="display:block; margin: 0 auto;" src="https://sreeni-demo-bucket.s3.amazonaws.com/teaser.mp4" width="640" height="400" controls preload autoplay loop></video>
<br>
If the video doesn't work, right-click [here](https://sreeni-demo-bucket.s3.amazonaws.com/teaser.mp4) and click "Save-As" to download it.

Here are some cool visualizations and comparisons with a couple of other methods.

#  Comparisons 
* [Click Here](/video-interp-examples/sepconv) to see more video comparisons with SepConv [Niklaus et. al.] 
<video style="display:block; margin: 0 auto;" src="https://sreeni-demo-bucket.s3.amazonaws.com/examples/sepconv_example1.mp4" width="640" height="240" controls preload autoplay loop></video> 
* [Click Here](/video-interp-examples/toflow) to see more video comparisons with TOFlow [Xue et. al.]                                                                                           
 <video style="display:block; margin: 0 auto;" src="https://sreeni-demo-bucket.s3.amazonaws.com/examples/toflow_example1.mp4" width="640" height="240" controls preload autoplay loop></video> 
* [Click Here](/video-interp-examples/superslomo) to see more video comparisons with SuperSloMo [Jiang et. al.]                                                                                 
 <video style="display:block; margin: 0 auto;" src="https://sreeni-demo-bucket.s3.amazonaws.com/examples/ssm2.mp4" width="640" height="240" controls preload autoplay loop></video>


# The Good and The Bad
This method of video interpolation is able to handle global optical flow estimation very well. Camera motion creates slow rigid motion, and linear interpolation works fine for this case.

However, occlusions, fast motion, and deformable motion are a big challenge. Here's an example from the DAVIS dataset of [a man doing parkour.](https://sreeni-demo-bucket.s3.amazonaws.com/parkour.mp4) As you can see, the interpolation quality suffers when there is large motion. One key reason is that the optical flow estimation from the first stage of the model is based on plain convolutional layers, but state of the art optical flow approaches use cost volumes. (e.g. PWC-Net).

# Quo Vadis, Video Interpolation?

This was work done in early 2019. Video interpolation is a hot topic with plenty of commercial value. Since then, there have been multiple improvements with incorporating [depth information](https://sites.google.com/view/wenbobao/dain), forward warping instead of backward warping [SoftSplat](https://arxiv.org/abs/2003.05534), and most recently [real-time approaches](https://arxiv.org/pdf/2011.06294.pdf) by estimating intermediate optical flows directly from the images (RIFE).

Interestingly, the modified version of SuperSloMo with a ConvLSTM bottleneck is competitive on the Vimeo benchmark with RIFE (35.55dB on the Vimeo90k benchmark vs. 35.69dB).


# Technical Challenges

The key technical challenge in getting this approach to work was [applying the warping layers](https://github.com/SreenivasVRao/SuperSloMo-VideoInterpolation-PyTorch/blob/843ba19334537361e115b870afcce8f645a8a110/scripts/models/layers.py#L73) correctly for backward optical flow in combination with bilinear interpolation. Fortunately, there were examples from other approaches like PWCNet which I could adopt. 

Another major challenge was in parallelizing the model for multi-GPU training, while ensuring an even distribution of the memory load on each GPU in PyTorch. (This was handled through some careful tensor reshaping - if you use my code, I'd love to hear if it works for you as well as it does for me!) I was able to achieve an almost uniform GPU memory load distribution during training, which allowed larger batch sizes, and faster training times, even though the compute resources were not the latest and greatest.  

# References:
  1. Jiang, Huaizu, et al. "Super slomo: High quality estimation of multiple intermediate frames for video interpolation." Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2018.
  2. Niklaus, Simon, Long Mai, and Feng Liu. "Video frame interpolation via adaptive separable convolution." Proceedings of the IEEE International Conference on Computer Vision. 2017.
  3. Xue, Tianfan, et al. "Video enhancement with task-oriented flow." International Journal of Computer Vision 127.8 (2019): 1106-1125. 







