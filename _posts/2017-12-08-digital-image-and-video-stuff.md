---
layout: post
ads: true
comments: true
published: true
title: Digital Image and Video Stuff
categories:
  - misc
tags:
  - digital image
  - digital video
---
#### Term
- [Understanding MPEG-2, MPEG-4, H.264, AVCHD and H.265](https://wolfcrow.com/blog/understanding-mpeg-2-mpeg-4-h-264-avchd-and-h-265/)
- [Program Stream vs Transport Stream](https://wolfcrow.com/blog/program-stream-vs-transport-stream-the-simple-difference/)

#### Image memory representation
```
https://www.collabora.com/news-and-blog/blog/2016/02/16/a-programmers-view-on-digital-images-the-essentials/
```
![image layout]({{site.baseurl}}/media/image=layout.png)

- Multi-Planar/single-planar

```
In the case of planar data, such as YUV420, linesize[i] contains stride for the i-th plane.

For example, for frame 640x480 data[0] contains pointer to Y component, data[1] and data[2] contains pointers to U and V planes.
In this case, linesize[0] == 640, linesize[1] == linesize[2] == 320 (because the U and V planes is less than Y plane half)

In the case of pixel data (RGB24), there is only one plane (data[0]) and linesize[0] == width * channels (640 * 3 for RGB24)
```
Credit: [Stackoverflow](https://stackoverflow.com/questions/13286022/can-anyone-help-in-understanding-avframe-linesize?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)


#### Intro
- [Definitive Workshop for video developers](https://docs.google.com/presentation/d/17Z31kEkl_NGJ0M66reqr9_uTG6tI5EDDVXpdPKVuIrs/edit#slide=id.p)
- [A hands-on introduction to video technology](https://github.com/leandromoreira/digital_video_introduction)


#### FFmpeg
- Resource collection
	- [books-on-ffmpeg-and-related-topics](https://www.whoishostingthis.com/compare/ffmpeg/resources/#books-on-ffmpeg-and-related-topics)

- Commands usage:
	- [INTRODUCTION TO FFMPEG](http://slhck.info/ffmpeg-encoding-course/#/1)


- Developmet using ffmpeg's libs

```
https://github.com/leandromoreira/ffmpeg-libav-tutorial
http://leixiaohua1020.github.io/#ffmpeg-development-examples
https://stackoverflow.com/questions/3199489/meaning-of-ffmpeg-output-tbc-tbn-tbr
https://stackoverflow.com/questions/43333542/what-is-video-timescale-timebase-or-timestamp-in-ffmpeg
```

#### Picture vs Frame vs Slice
```
Picture := Frame | Field
Frame := a complete image
Field := set of odd-numbered or even-numbered scan lines composing a partial image.

Slice := spatially distinct region of a frame that is encoded separately from any other region in the same frame.
```
Credit: [wikipedia](https://en.wikipedia.org/wiki/Video_compression_picture_types)

#### H264/AVC bitstreams format/hierarchy of layers
![H264-AVC-syntax.jpg]({{site.baseurl}}/media/H264-AVC-syntax.jpg)

Credit: [www.vocal.com](https://www.vocal.com/video/h-264-avc-syntax/)

- VLC NAL unit vs non-VLC NAL unit

```
VCL NAL units := that contain encoded data of video pictures
non-VLC NAL units := that contain any associated additional information (such as SPS, PPS, SEI)
```

- Parameter sets

```
sequence parameter sets (SPS) := which apply to a series of consecutive coded video pictures called a coded video sequences.
picture parameter sets (PPS) := which apply to the decoding of one or more individual pictures within a coded video sequence
```

- Access unit

```
Access unit := A set of NAL units for a coded frame or field in a specified form/order.
(The decoding of each access unit results in one decoded picture)
```

![NAL_Acess_Unit.JPG]({{site.baseurl}}/media/NAL_Acess_Unit.JPG)

- Coded Video Sequences
	- Series of access units that are sequential in the NAL unit stream and use only one SPS.
    - Start with instantaneous decoding refresh (IDR) access unit. All following video frames or fields are coded as slices
    
Credit: 
- [wikipedia](https://en.wikipedia.org/wiki/Network_Abstraction_Layer) 
- [www.vocal.com](https://www.vocal.com/video/h-264-avc-syntax/)

#### Bitstreams format: AnnexB (h264_mp4toannexb), AVCC
- [Annex B](https://stackoverflow.com/questions/24884827/possible-locations-for-sequence-picture-parameter-sets-for-h-264-stream/24890903#24890903)

#### RTP Payload Format for H.264 Video
- [https://tools.ietf.org/html/rfc6184#section-1](https://tools.ietf.org/html/rfc6184#section-1)
