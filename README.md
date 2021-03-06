# FPVCamera
Describe the Specifications for an ideal digital FPV Camera compatible with OpenHD

### 1. What is a digital FPV Camera
In this context, a digital FPV Camera is a digital camera sensor (for example IMX219 ) paired with a h264/h265 encoder chip that connects via USB to linux and provides a live h264/h265 (digital) stream via the USB interface.
You could also refer to this as an IP Camera / Webcam with integrated encoder.[Example](https://shop.runcam.com/runcam-webcam/)
Other than providing a live video stream via USB, an ideal FPV Camera shall also provide an interface to change the following parameters via USB:
- framerate  (minimum requirement)
- resolution  (minimum requirement)
- bitrate (minimum requirement)
- key frame intervall (minimum requirement)
- white balance / exposure (nice to have)


### 2. Why not use the RPI encoder and a camera that connects via CSI instead of USB (like Pi Cam V2)
Having the encoder as close to the camera sensor as possible has the following advantages for our use case:
1. The stream is compressed as soon as possible and not transfered over a high bitrate interface (CSI interface), which reduces emission of interfering RF signals 
2. The CSI connector is fragile and limits the possibilities for placing the camera inside your RC model aircraft
3. The rpi encoder is slow and can barely do 720p 60 (720p 49). Alternatives to the rpi with a good integrated encoder (like jetson) are generally big and expensive.
4. But paired with a camera that already encodes the video in h264/5 the rpi CM4 makes a great packet for digital FPV.

### 3. Latency requirements:
To provide a good user experience, an Ideal OpenHD FPV Camera must fulfill the following requirements:
1. Encode the raw image sensor data as quick as possible with h264/h265 and forward the h264/h265 NALUs via USB without any buffering
2. Use h264/h265 encoding parameters that not only allow the stream to be encoded quickly by the encoder HW, but also allow it to be **decoded quickly and without any buffering**.
3. As a quideline, the minimum requirements are:
   - Configure the stream such that the encoder only produces I or P frames, ideally only I frames. One way to do this is to use the h264 "Baseline" profile
   - Configure the stream such that the decoder knows that no picture re-ordering is possible. One way to do this is to set pic_order_cnt_type to 2
   - Configure the encoder such that the encoding time is not more than 20ms, ideally lower for both 720p60 / 1080p60
   
### 4. Resolution / Framerate requirements:
To be usable for FPV, the minimum requirements for a live FPV feed are 720p60fps. More expensive configurations like 1080p60 or 720p120 would be nice.

### 4. Size requirements:
The whole unit (image sensor and encoder) shall be as light and small as possible since it is always mounted on an aircraft or "drone"

### 5. Nice to have:
Using 2 cameras when flying FPV is pretty common. In this case, one camera provides the low latency live view while the other camera is only used for recording the experience.However, most modern SOCs used for action cameras or similar can encode the same camera stream twice, once with a smaller bitrate for live preview and once with a higher bitrate for storage. Such a solution might be favorable for the vast majority of users and would justify a higher price.



