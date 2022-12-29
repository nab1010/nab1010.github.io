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

Plugin này cho phép DeepStream pipeline sử dụng thư viện low-level tracker để track các đối tượng đã được phát hiện bằng ID (có thể là duy nhất) của nó theo thời gian. Nó hỗ trợ một số thư viện low-level có thể implements `NvDsTracker`API, bao gồm các implementation tham khảo được cung cấp bởi thư viện `NvMultiObjectTracker`: NvDCF, DeepSORT, và IOU Tracker. Là một phần của API này, plugin truy vấn thư viện low-level để có các khả năng và yêu cầu liên quan đến ***input format***, ***memory type*** và ***batch processing support***. Dựa trên các truy vấn này, sau đó plugin chuyển đổi input frame buffers sang định dạng mà thư viện low-level tracker yêu cầu. Ví dụ, `NV12` hoặc `RGBA`, trong khi IOU hoàn toàn không yêu cầu frame buffers.

Các khả năng của thư viện low-level tracker cũng bao gồm ***support for batch processing*** (hỗ trợ cho xử lý batch) trên multiple input stream (nhiều đầu vào). Batch processing thường hiệu quả hơn so với xử lý độc lập đặc biệt là khi GPU-based acceleration (tăng tốc độ phần cứng dựa trên GPU) được thực hiện bởi thư viện low-level. Nếu một thư viện low-level ***support batch processing***, thì đó sẽ là chế độ hoạt động được chọn bởi plugin; tuy nhiên tùy chọn này có thể được ghi đè với tùy chọn cấu hình `enable-batch-process` nếu thư viện low-level hỗ trợ cả theo batch và theo luồng (per-stream-modes).

Các khả năng của low-level cũng bao gồm hỗ trợ ***passing the past-frame*** (truyền frame trước đó), bao gồm dữ liệu theo dõi đối tượng được tạo trong các khung hình trước đây nhưng chưa được report là đầu ra. Đây có thể là trường hợp khi low-level tracker lưu trữ dữ liệu theo dõi đối tượng được tạo trong các khung hình trước đây chỉ trong nội bộ do độ tin cây theo dõi thấp, nhưng sau đó đã quyết định report do độ tin cây cao. Nếu dữ liệu khung hình trong quá khứ - past frames được truy xuất từ low-level tracker, nó sẽ được report là *user-meta* gọi là `NvDsPastFrameObjBatch`. Nó có thể được kích hoạt bằng tùy chọn cấu hình `enable-past-frame`.



