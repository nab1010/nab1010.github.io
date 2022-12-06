---
layout: post
title: "GStreamer 01"
subtitle: ""
author: "NAB"
header-img: "img/in-post/2022/Nov/Knowledge/MovieLens_use_CF.png"
# header-style: text
header-mask: 0.6
lang: vi
catalog: true
# hidden: false
section:  Knowledge
seo-keywords:
  - RS
  - recommendation
  - system
  - content
  - item
tags:
  - Recommendation System 
  - Collaborative Filtering
---


# GStreamer

GStreamer là một framework cực kỳ mạnh mẽ và linh hoạt để tạo các ứng dụng truyền phát trực tiếp. Nhiều ưu điểm của framework GStreamer đến từ tính module của nó: GStreamer có thể kết hợp liền mạch các modules plugin mới. Nhưng vì tính module và sức mạnh thường đi kèm với tính phức tạp, nên việc viết các ứng dụng mới không phải lúc nào cũng dễ dàng.

# Prerequisites

Trước khi làm theo những hướng dẫn này, bạn cần phải cài đặt môi trường phát triển theo nền tảng của mình. Nếu bạn chưa hoàn thành nó, đến trang [cài đặt GStreamer](https://gstreamer.freedesktop.org/documentation/installing/index.html?gi-language=c) và quay lại đây sau đó.

Các hướng dẫn hiện nay chỉ được viết bằng ngôn ngữ lập trình C, do đó bạn cần cảm thấy thoải mái với nó. Mặc dù C không phải một ngôn ngữ hướng đối tượng (Object-Orients OO), framework GStreamer sử dụng các `GObject`, do đó vài kiến thức về OO sẽ có ích. Kiến thức về `GObject` và thư viện `GLib` là không bắt buộc, nhưng sẽ giúp bạn học dễ dàng hơn.

# Source code

Mỗi hướng dẫn đại diện cho một dự án riêng biệt, với tất cả mã nguồn viết bằng C. Các đoạn mã nguồn được giới thiệu cùng với văn bản và nguồn đầy đủ (cùng với bất kỳ tệp bắt buộc nào khác như makefiles tệp hoặc tệp dự án) được phân phối với GStreamer, cũng như được giải thích trong hướng dẫn cài đặt. 

Mã nguồn cho các hướng dẫn ở [đây](https://gitlab.freedesktop.org/gstreamer/gstreamer-rs/-/tree/master/tutorials/src/bin)

# A short note on GObject and GLib

GStreamer được xây dựng dựa trên các thư viện `GObject` (để hướng đối tượng) và `GLib` (cho thuật toán phổ biến), điều này có nghĩa thỉnh thoảng bạn sẽ phải gọi hàm của các thư viện này. Mặc dù các hướng dẫn sẽ đảm bảo rằng bạn không cần phải có kiến ​​thức sâu về các thư viện này, nhưng việc làm quen với chúng chắc chắn sẽ giúp quá trình học GStreamer trở nên dễ dàng hơn.

Bạn có thể biết mình đang gọi thư viện nào vì tất cả các hàm của GStreamer, cấu trúc và kiểu có tiền tố `gst_`, trong khi `GLib` và `GObject` sử dụng `g_`

# Sources of documentation

# Structure

Các hướng dẫn được sắp xếp theo từng phần, xoay quanh một chủ đề chung:
 
 * [Hướng dẫn cơ bản](): Mô tả các chủ đề chung cần thiết để hiểu phần còn lại của hướng dẫn trong GStreamer.
 * Hướng dẫn phát lại: Giải thích mọi thứ bạn cần biết để tạo ứng dụng phát lại phương tiện bằng GStreamer.
 * Hướng dẫn về Android: Hướng dẫn xử lý một số chủ đề dành riêng cho Android mà bạn cần biết.
 * Hướng dẫn iOS:Hướng dẫn xử lý một số chủ đề dành riêng cho iOS mà bạn cần biết.