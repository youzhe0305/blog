---
title: 深度學習與實驗(6項project)
top_img: transparent
date: 2024-08-30 23:29:18
tags:
    - Deep Learning
    - Computer Vision
    - Generative
    - Diffusion
    - Transformer
    - AE
    - VAE
    - UNet
    - ResNet
    - CNN
    - Python
categories:
    - Personal-Project
cover: Personal-Project/DLP/MaskGIT_result.png
description: 深度學習課程(DLP)的作業，涵蓋手刻神經網路、CNN、VAE、Transformer、Diffusion...等技術。

---

# Overview

此專案為深度學習課程(DLP)作業，包含以下6個主題及技術
1. **Handcraft Neural Network**
    -  Backpropagation、Neural Network、numpy
2. **Iamge Classification**
    - CNN、SCCNet
3. **Image Segmentation**
    - UNet、ResNet
4. **Conditional Video Generation**
    - VAE、Video Generation
5. **Image Inpainting**
    - Transformer、MaskGIT
6. **Conditional Image Generation**
    - Diffusion

# Background

於大一暑假，修習"深度學習(DLP)"之作業

# LAB

## Handcraft Neural Network

在不使用Pytorch、Tensorflow等深度學習套件的情況下，使用Numpy刻出一個神經網路。神經網路的架構圖如下:
![Neural Network](/Personal-Project/DLP/NN.png)
實作基於矩陣的backpropagation，實作梯度的傳播，優化參數，進行資料點的分類
結果如下:
![Data Point Prediction](/Personal-Project/DLP/NN_pred.png)

## Iamge Classification

構造CNN，用來做基於腦電圖(EEG)的患者動作分類，使用Spatial Component-wise Convolutional Network(SCCNet)，網路架構圖如下:
![SCCNet](/Personal-Project/DLP/SCCNet.png)
使用Pytorch實作，並且對於一般訓練及Fine-Tune等方式做分析

reference paper: [SCCNet](https://www.merl.com/publications/docs/TR2018-210.pdf)

## Image Segmentation

只使用Pytorch來構造UNet以及ResNet34-Unet，用來做寵物圖片的前後景image segmentataion，如圖:
![Image_segmentation](/Personal-Project/DLP/Image_segmentation.png)
建構的網路架構如下:
UNet:
![Unet](/Personal-Project/DLP/Unet.png)
Res34-UNet (以ResNet做Encoder，UNet做Decoder):
![Res34-UNet](/Personal-Project/DLP/Res34_UNet.png)
另外也實作不同的Data Augmentation(包括UNet論文中使用的Elastic Transformation等等)

reference paper: [UNet](https://arxiv.org/abs/1505.04597v1)、[Res34-Unet](https://www.researchgate.net/publication/359463249_Deep_learning-based_pelvic_levator_hiatus_segmentation_from_ultrasound_images)

## Conditional Video Generation

建構Conditional Varitional AutoEncoder (CVAE)模型，用來做圖片的生成
首先，推導出CVAE的objective function:
![CVAE Math](/Personal-Project/DLP/CVAE_math.png)
接著，基於CNN建構出CVAE，模型架構如下:
![CVAE](/Personal-Project/DLP/CVAE.png)
使用CVAE，以肢體骨架圖以及前一幀的影片內容生成影片的下一幀。也就是，在inferece時，可以使用單張frame跟任意數量的骨架圖片生成出一整段的影片
另外，也實作對於loss的adaptive adjustment，這裡實作對於KL Divergence的Annealing。

reference paper: [Everybody Dance Now](https://arxiv.org/abs/1808.07371)、[Stochastic Video Generation with a Learned Prior](https://arxiv.org/abs/1802.07687)、[Cyclical Annealing Schedule](https://arxiv.org/abs/1903.10145)

## Image Inpainting

建構基於Transformer的Masked Generative Image Transformer(MaskGIT)，可以對於部分有缺漏(被mask住)的圖片的inpainting，如圖:
![Inpaint Result](/Personal-Project/DLP/MaskGIT_result.png)
建構的MaskGIT架構如圖:
![MaskGIT](/Personal-Project/DLP/MaskGIT.png)
將圖片encode成一張小圖，並且做離散化的Tokenize，限制embedding的可能性，讓模型更容易去model embedding space。接著使用Transformer來做inpainting，把其中的mask token補成其他的token，再用Decoder還原回圖片。

reference paper: [MaskGIT](https://arxiv.org/abs/2202.04200)

## Conditional Image Generation

使用Hugging Face API來建構Diffusion Model及其model pipeline，基於種類的Condition來生成幾何體，如圖:
![Geometry](/Personal-Project/DLP/Geometry.png)

reference paper: [DDPM](https://arxiv.org/abs/2006.11239)

# Ohter

本文僅簡單概述Project的部分成果，詳細的內容、分析詳見:
https://github.com/youzhe0305/NYCU-DLP
裡面有完整的程式碼及報告

