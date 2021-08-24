---
layout: post
ads: true
comments: true
published: true
title: Annex B h264/h265 bitstreams tools
categories:
  - linux
  - program
  - tooltips
tags:
  - h264
  - h265
  - annexb
---
### Get data

- Extracting h264 raw video stream from mp4 or flv with ffmpeg (https://stackoverflow.com/a/19304516)

```bash
To extract a raw video from an MP4 or FLV container you should specify the -bsf:v h264_mp4toannexb or -vbfs h264_mp4toannexb option.

ffmpeg -i test.flv -vcodec copy -an -bsf:v h264_mp4toannexb test.h264

The raw stream without H264 Annex B / NAL cannot be decode by player. With that option ffmpeg perform a simple "mux" of each h264 sample in a NAL unit.
```

### Tools
- A H264 frame data viewer

	https://github.com/shi-yan/H264Naked
	
    ![](https://raw.githubusercontent.com/shi-yan/H264Naked/master/H264Naked_screenshot.png)

- HEVCESBrowser is a tool for analyzing hevc(h265) bitstreams

	https://github.com/virinext/hevcesbrowser
    
    ![](https://cloud.githubusercontent.com/assets/10683398/6995983/2f0a3974-db20-11e4-8d8f-cd6db7a954c4.png)
    
- Gitl HEVC/H.265 Analyzer based on Qt. Custom filters supported.

	https://github.com/lheric/GitlHEVCAnalyzer
    
    ![](https://github.com/lheric/GitlHEVCAnalyzer/blob/master/screenshots/screenshot_win.png?raw=true)
    

