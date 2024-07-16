---
title: 新聞推播Discord Bot
top_img: transparent
date: 2021-08-19 21:25:36
tags:
    - Python
    - Discord Bot
    - Crawler
    - RSS
categories:
    - Personal-Project
cover: Personal-Project/newspaper-dc-bot/newspaper-UI.png
description: 利用爬蟲、RSS獲得新聞資訊，並分類分tag，用Discord Bot定時推播
---

# Overview

此專案製作 "定時推播新聞的Discord Bot"。透過bs4爬蟲、RSS每天收集最新新聞更新資料庫，並定期在Discord頻道做新聞的推播，使用者可以自選想看的新聞種類。新聞資訊將帶有標題、連結

### CoreTech
- Crawler
    - bs4
- RSS   
- Discord Bot

示意圖:
![](/Personal-Project/newspaper-dc-bot/newspaper-UI.png)

# Background

SITCON CAMP 2021的黑客松作品，主題為Discord Bot的應用。

# Apporach

## Data Collection

使用**bs4**製作爬蟲，輔以新聞的RSS系統，把**7家新聞網站**的最新資料抓下來。存取了種類、標題、tag等等資訊。

## Filter

將各家不同的種類、tag整合為特定的幾個類別，以便整合之後，供使用者選取。

## Discord Bot

每天定時推播，使用者可以透過emoji的選擇，讓Discord把相關種類的新聞播送出來

# Contribution

我: 負責爬蟲、RSS爬取新聞，並分出類別、tag，貢獻Discord Bot的部分介面
組員: 完成Discord Bot的介面

# Ohter

本文僅簡單概述Project的部分成果，詳細的內容、分析詳見:
https://github.com/youzhe0305/newspaper-discord-bot
裡面有完整的程式碼


