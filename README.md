# fuji-xt3-on-linux
Tips to improve the way the fujifilm xt3 works on linux

## Use the xt3 as a video source

1. Install the required video packages (adapt depending on your distrib)
```
apt install gphoto2 v4l2loopback-utils v4l2loopback-dkms ffmpeg
```

2. Load kernel module
```
modprobe v4l2loopback exclusive_caps=1 max_buffers=2
```

3. Configure the video resolution with the camera plugged on USB and then turned on
```
gphoto2 --get-config /main/capturesettings/liveviewsize
gphoto2 --set-config-index liveviewsize="2"
gphoto2 --get-config /main/capturesettings/liveviewsize
```
_NB : get-config commands to check the quality has been changed_

4. Launch video feed on /dev/video0
```
gphoto2 --stdout --capture-movie | ffmpeg -i - -vcodec rawvideo -pix_fmt yuv420p -threads 0 -f v4l2 /dev/video0
```

5. Video feed is available on /dev/video0 with 1024x768 resolution

_can be checked with vlc_
_jitsi on FF82 does not find the camera but does on Chromium_


sources :
- https://medium.com/nerdery/dslr-webcam-setup-for-linux-9b6d1b79ae22
- https://www.fujirumors.com/fujifilm-x-webcam-how-to-use-your-fujifilm-camera-with-obs-and-fujifilm-x-webcam-on-linux/
