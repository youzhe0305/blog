---
title: Berkeley CV (5項project)
top_img: transparent
date: 2024-09-19 18:00:00
tags:
    - Computer Vision
    - 
    - RNN
    - Online
    - Python
categories:
    - Personal-Project
cover: Personal-Project/Berkeley-CV/sift_box.png
description: 改進CVPR 2023論文DSTNet，使其能做到Online Real Time Inference

---

# Overview

此專案為學習電腦視覺的自主練習，以Bekerley CS194-26的電腦視覺課作業作為練習對象
包含以下5個主題及技術:
1. **Image Aligment & Colorize**
    - Image Aligmnet、Basic CV Package
2. **Image Sharpen & Image Hybrid**
    - Space Frequency、Laplacian Pyramid、Multiresolution Spline
3. **Image Warping & Midway Image**
    - Warping Matrix
4. **Image Stiching**
    - Warping Matrix、Image Blending
5. **Auto Image Stiching**
    - SIFT、RANSAC


# Background

因為要做電腦視覺相關的研究，因此找Berkeley的公開作業來做練習，累積電腦視覺相關知識及技術經驗。

# Projects

## Image Aligment & Colorize

把三張分別紀錄RGB channel資訊的圖片，使用loss function減小圖片疊合起來的差異，將圖片合併並且把顏色塗上
如圖，將RGB三通道圖對齊合併成彩色圖
![Lake](/Personal-Project/Berkeley-CV/lake.png)

## Image Edge Detection & Image Hybrid

這個作業主要分成2部分

第一部分是做圖片的銳化，銳化圖片的本質是加強邊界的效果，因此要先做edge dectection，利用圖片中的edge的顏色變化會很大的特性，以梯度、Difference of Gaussian (DoG) 等方法找出圖片的edge，效果如圖:
![Camera](/Personal-Project/Berkeley-CV/camera.png)
接著再將得到的邊界依比例加成到原本的圖片上，即可達到圖片銳化的效果
![TAJ](/Personal-Project/Berkeley-CV/taj.png)

第二部分是做圖片的hybrid，首先使用Gaussian Kernel將圖片分成不同space frequency的Laplacian Pyramid，為了不同的空間頻率上做blending做準備。接著使用Multiresolution Spline的方法，因為分成不同resolution處理能對於細節或是整體去做不同的混合，因此分成不同resolution能讓邊界變得更不明顯。
如圖，把橘子跟蘋果混合，兩種水果間的邊界不明顯，且水果上的紋路是漸進變換的
![Hybrid](/Personal-Project/Berkeley-CV/hybrid.png)

## Image Warping & Midway Image

這個作業使用Warping Martix，根據不同圖片的對應點做圖片的形變，這個作業中，我手刻了Warping Matrix的計算，並且把兩張臉部表情的圖算出中間圖，讓他可以得到漸變的GIF，如下:
![Midway Face](/Personal-Project/Berkeley-CV/mid_way_face.gif)

## Image Stiching

這個作業做的是場景的拼接，因為一個場景可能太大，不能用一張照片拍完，因此對於一個場景拍多個照片，再找到他們的對應點，就可以透過Warping Matrix形變，做場景的拼接。
如圖，我將我的書桌透過手動找特徵點的方式，拼接在一起:
![Desk](/Personal-Project/Berkeley-CV/desk.jpg)
(看起來沒有很疊合是因為，Warping Matrix適用於2D空間，因此對於3D空間的書桌效果會稍微差一點)

## Auto Image Stiching

承接上一個作業，但是這次不是手動點特徵點，而是使用SIFT算法自動尋找特徵點，再利用RANSAC算法來排除SIFT找到的outlier，讓Warping Matrix可以被計算的更精準:
如圖:
![SIFT Desk](/Personal-Project/Berkeley-CV/sift_desk.png)
![SIFT Box](/Personal-Project/Berkeley-CV/sift_box.png)
# Ohter

本文僅簡單概述Project的部分成果，詳細的內容、分析詳見:
https://github.com/youzhe0305/Berkeley-Intro-to-Computer-Vision-and-Computational-Photography
裡面有完整的程式碼
