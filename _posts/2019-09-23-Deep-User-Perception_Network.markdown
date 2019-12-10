---
layout:     post
title:      "Deep User Perception Network"
subtitle:   "Perceive Your Users in Depth: Learning Universal User Representations from Multiple E-commerce Tasks"
date:       2019-09-23 10:24:00
author:     "Xindi"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - PaperNotes
    - 推荐系统
---



论文地址: https://arxiv.org/pdf/1805.10727.pdf




用户行为序列:
按时间排序的行为序列
$$x=\{x_1,x_2,\cdots,x_N\}$$

$x_i$: 按照时间排序的第i个行为
$$x_i=<item_i, property_i>$$
$item_i$: 和$i_{th}$ 行为对应的item
泛化特征: shop ID, brand, category, item tags (主要对于长尾商品和新商品起作用)
个性化特征: item ID (对于流行商品起作用)

$property_i$: 描述用户对于某一特定商品行为的特性
包括行为的类型: 点击,收藏,加购物车,购买
行为的场景: 描述行为发生的地方, 比如搜索, 推荐以及广告
