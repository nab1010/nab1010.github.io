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


Plugin chấp nhận dữ liệu khung hình có định dạng `NV12` hoặc `RGBA` từ phần tử upstream và scales (và/hoặc converts) input buffer thành buffer trong plugin tracker dụa trên định dạng được yêu cầu bởi thư viện low-level, với độ phân giải frame đưcọ chỉ định bởi `tracker-width` và `tracker-height` trong tệp cấu hình của phần `[tracker]`. Đường dẫn đến thư viện low-level tracker được chỉ định thông qua tùy chọn cấu hình `ll-lib-file` trong cùng phần. Thư viện low-level đươc sử dụng cũng có thể yêu cầu tệp cấu hình riêng, tệp này có thể được chỉ định thông qua tùy chọn `ll-config-file`. Nếu `ll-config-file` không được chỉ định, thư viện low-level tracker có thể tiếp tục với các giá trị tham số mặc định của nó. Việc triển khai low-level tracker tham khảo được cung cấp bởi thư viện `NvMultiObjectTracker` hỗ trợ các thuật toán theo dõi khác nhau:
- **NvDCF**: **NvDCF** tracker là một  NVIDIA®-adapted Discriminative Correlation Filter (DCF) tracker - Bộ lọc tương quan sử dụng sử dụng thuật toán học phân biệt trực tuyến (online discriminative learning algorithm) dựa trên bộ lọc tương quan (correlation filter-based)cho khả năng theo dõi đối tượng trực quan, đồng thời sử dụng thuật toán liên kết dữ liệu (data association algorithm) và ước tính trạng thái (state estimator) để theo dõi nhiều đối tượng (multi-object tracking). 
- **DeepSORT**: **DeepSORT** tracker là một re-implementation của DeepSORT tracker chính thức, sử dụng phương pháp học chỉ số deep cosine với một Re-ID neural network. Implementation này cho phép người dùng sử dụng bất kỳ mạng Re-ID nào miễn là nó được hỗ trợ bởi framework NVIDIA TensorRT™.
- **IOU Tracker**:  Intersection-Over-Union (IOU) tracker sử dụng các giá trị IOU trong các bounding box của detector giữa 2 frame liên tiếp để thực hiện liên kết giữa chúng hoặc chỉ định ID mục tiêu mới nếu không tìm thấy kết quả khớp. Tracker này bao gồm một logic để xử lý các false positives và false negatives từ object detector; tuy nhiên, đây có thể coi là bare-minimum object tracker, chỉ có thể đóng vai trò là baseline.

Chi tiết hơn về từng thuật toán và triển khai của nó có thể tìm thấy tại phần [NvMultiObjectTracker : A Reference Low-Level Tracker Library](https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_plugin_gst-nvtracker.html#nvmultiobjecttracker-a-reference-low-level-tracker-library).


![DeepStream Plugin Gst-nvtracker](/img/in-post/2022/Dec/Knowledge/DS_plugin_gst-nvtracker_6.0GA.png "DeepStream Plugin Gst-nvtracker")

_**Hình 1:** DeepStream Plugin Gst-nvtracker_


# Inputs anb Outputs

Phần này sẽ tổng hợp các inputs và outputs và phương tiện giao tiếp của plugin Gst-nvtracker.

- Input
  - Gst Buffer (dưới dạng frame batch từ các luồng source có sẵn)
  - `NvDsBatchMeta`

- Output
  - Gst Buffer (được cung cấp như một input)
  - `NvDsBatchMeta` ()