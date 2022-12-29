---
layout: post
title: "Gst-nvtracker"
subtitle: "Gst-nvtracker"
author: "NAB"
header-img: "img/in-post/2022/Dec/nvidia_wallpaper.jpg"
# header-style: text
header-mask: 0.4
lang: vi
catalog: true
# hidden: false
section: Knowledge
seo-keywords:
  - NVIDIA
  - GStreamer
  - Install
  - install-gstreamer
  - nvidia-gstreamer
  - GStreamer_tutorial
  - gst-nvtracker
  - tracker
tags:
  - NVIDIA
  - GStreamer
  - DeepStream
---

Plugin này cho phép DeepStream pipeline sử dụng thư viện low-level tracker để track các đối tượng đã được phát hiện bằng ID (có thể là duy nhất) của nó theo thời gian. Nó hỗ trợ một số thư viện low-level có thể implements `NvDsTracker`API, bao gồm các implementation tham khảo được cung cấp bởi thư viện `NvMultiObjectTracker`: NvDCF, DeepSORT, và IOU Tracker. Là một phần của API này, plugin truy vấn thư viện low-level để có các khả năng và yêu cầu liên quan đến input format, memory type và batch processing support. Dựa trên các truy vấn này, sau đó plugin chuyển đổi input frame buffers sang định dạng mà thư viện low-level tracker yêu cầu. Ví dụ, `NV12` hoặc `RGBA`, trong khi IOU hoàn toàn không yêu cầu frame buffers.

