---
layout: post
title: "Recommendation System"
subtitle: "Recommendation System"
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
  - rs
  - Recommendation System 
---

# Abstract

# Content-based recommnndation

* Predict 
* 



$$
\hat{v}_i^I=\frac{1}{\left|\mathbb{N}_i^I\right|} \sum_{j=1}^{\left|\mathbb{B}_i^I\right|} \rho_v^1\left(\left[e_{i j}^I, v_j^I\right]\right)
$$
ributes from its adjacent label nodes as
$$
\bar{v}_i^I=\frac{1}{\left|\mathbb{N}_i^L\right|} \sum_{j=1}^{\left|\mathbb{E}_i^A\right|} \rho_v^2\left(\left[e_{i j}^A, v_j^L\right]\right)
$$
outes of instance edge and assignment edge of the instance edge and label edge of the
1ode $\mathbf{v}_i^L$ in the label graph is adjacent with 1 edges and label edges, respectively. Simila
es, we also design two aggregation function er adjacent nodes as
$$
\begin{aligned}
&\hat{v}_i^L=\frac{1}{\left|\mathbb{N}_i^L\right|} \sum_{j=1}^{\left|\mathbb{E}_i^L\right|} \rho_v^3\left(\left[e_{i j}^L, v_j^L\right]\right) \\
&\bar{v}_i^L=\frac{1}{\left|\mathbb{N}_i^I\right|} \sum_{j=1}^{\left|\mathbb{E}_1^A\right|} \rho_v^4\left(\left[e_{i j}^A, v_j^I\right]\right)
\end{aligned}
$$