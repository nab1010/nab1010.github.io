---
layout: post
title: "Collaborative Filtering Recommender Systems "
subtitle: "J. Ben Schafer, Dan Frankowski, Jon Herlocker, and Shilad Sen"
author: "NAB"
header-img: "img/in-post/2022/Oct/yolov1.png"
# header-style: text
header-mask: 0.4
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

# Abstract
Một trong những công nghệ các nhân hóa mạnh mẽ cung cấp sức mạnh cho web là collaborative filtering (CF) - lọc cộng tác. CF là quá trình lọc hoặc đánh giá các items thông qua ý kiến của người khác. Công nghệ CF tập hợp ý kiến của các cộng đồng lớn được kết nối với nhau trên web, hỗ trợ lọc số lượng dữ liệu đáng kể. Trong bài này, các tác giả giới thiệu các khái niệm cốt lõi về lọc cộng tác dùng trong adaptive web, lý thuyết và thực hành thuật toán CF, và các quyết định thiết kế liên quan đến hệ thống đánh giá và thu thập đánh giá, Các tác giả cũng thảo luận về cách đánh giá hệ thống CF và sự phát triển của các giao diện tương tác.
# Introduction

Collaborative Filtering là quá trình lọc hoặc đánh giá các items sử dụng ý kiến của người khác. Mặc dù thuật ngữ lọc cộng tác (CF) mới chỉ xuất hiện được hơn một tập kỷ, CF bắt nguồn từ một việc mà con người đã làm trong nhiều thế kỷ - chia sẻ ý kiến với người khác.

Trong nhiều năm, mọi người đã đọc và thảo luận về những cuốn sách, những nhà hàng họ đã thử và những bộ phim họ đã xem - sau đó sử dụng những cuộc thảo luận này để đưa ra ý kiến. Ví dụ, Khi có đủ các đồng nghiệp của Amy nói rằng họ thích bộ phim mới nhất của Hollywood, cô ấy có thể quyết định rằng mình cũng nên xem nó. Tương tự như vậy, nếu nhiều người trong số họ thất nó là một thảm họa, cô ấy có thể quyết định tiêu tiền của mình vào nơi khác. Tốt hơn nữa, Amy có thể nhận thấy rằng Matt giới thiệu những thể loại phim mà cô ấy thấy thú vị, Paul có lịch sử giới thiệu những bộ phim mà cô ấy không thích, và Margaret dường như chỉ dưới thiệu mọi thứ. Theo thời gian, cô ấy biết được mình nên lắng nghe ý kiến của ai và cách áp dụng những ý kiến này để giúp cô ấy xác định được chất lượng của một item. 

Máy tính và trang web cho phép chúng ta tiến xa hơn nhưng lời truyền miệng. Thay vì giới hạn bản thân trong hàng chục hoặc hàng trăm cá nhân, internet cho phép chúng ta xem xét ý kiến của hàng nghìn người. Tốc độ của máy tính cho phép chúng ta xử  lý những ý kiến này trong thời gian thực và xác định không chỉ cộng đồng lớn hơn nhiều nghĩ gì về một sản phẩm, mà còn phát triển chế độ xem thực sự được cá nhân hóa về mặt hàng đó bằng cách sử dụng các ý kiến phù hợp nhất cho một người dùng hoặc nhóm người dùng nhất định.


![MovieLens sử dụng tính năng lọc cộng tác để dự đoán rằng người dùng này có khả năng xếp hạng phim "Holes" 4 trên 5 sao.](img/in-post/2022/Nov/Knowledge/MovieLens_use_CF.png "MovieLens sử dụng tính năng lọc cộng tác")
## Core Concept

Trong khi chương này xem xét nhiều hệ thống CF, chúng tôi giới thiệu chủ đề thông qua MovieLens. MovieLens là một hệ thống lọc cộng tác cho phim. Một user trong MovieLens đánh giá các bộ phim sử dụng 1 đến 5 sao, trong đó 1 là "Awful - Kinh khủng" và 5 là "Must See - Nên xem". MovieLens sau đó sử dụng các đánh giá của cộng động để đề xuất phim khác người dùng có thể quan tâm
