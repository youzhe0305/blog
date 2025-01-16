---
title: Anime Helper
top_img: transparent
date: 2024-12-29 23:29:18
tags:
    - Web
    - Frontend
    - Backend
    - React.js
    - Express.js
    - PostgreSQL
categories:
    - Team-Project
cover: Team-Project/Anime-Helper/home.png
description: 使用React.js, Express.js, PostgreSQL建構的現代化設計動畫資訊網站
---

# Overview

此專案製作"Anime Helper"，一個基於MyAnimeList網站資料集的動畫資訊整合網站。功能涵蓋：動畫搜尋、分類、排序、登入登出、收藏清單、留言、評分
![Home](/Team-Project/Anime-Helper/home.png)

### CoreTech
- Frontend
    - React.js   
- Backend
    - Express.js
- Database
    - PostgreSQL

介紹影片:
{% youtube evQajoC1g4c %}

# Background

於大二上學期，修習"資料庫系統概論"之專題

# Functionality

## Ranking

對於頁面列出的動畫，可以用評分高低或是播出時間來做排序
如圖，可以看到評分最高的動畫有鋼鍊、死神等
![Ranking](/Team-Project/Anime-Helper/ranking.png)

## Category

將動畫分成喜劇、動作...等種類
如圖，可以看到各個經典喜劇動畫
![Category](/Team-Project/Anime-Helper/category.png)

## Search

可以用關鍵字搜尋動畫
如圖，搜尋"輝夜"，可以看到"輝夜姬想讓人告白"、"輝夜姬物語"等等的動畫
![Search](/Team-Project/Anime-Helper/search.png)

## Auth

可以申請帳號、登入登出，利用token保持登入狀態，以使用一些使用者專用的功能(收藏、評分、留言等等)
如圖，可以登入並且查看使用者介面
![Auth](/Team-Project/Anime-Helper/auth.png)

## Information

每一個動畫都會有一個主頁，這個主頁呈現了動畫的名稱、評分、播出時間、集數、敘述...等訊息，如圖:
![Information](/Team-Project/Anime-Helper/info.png)

## Favorite

在動畫的主頁中，可以將動畫加入快速收藏清單中，或者是依據選單，加入自定義的清單中，如圖:
![Favorite1](/Team-Project/Anime-Helper/favorite1.png)
而從首頁可以進到收藏清單的頁面，可以看到自己的各個收藏清單及動畫，並且連結到動畫的資訊頁面
![Favorite2](/Team-Project/Anime-Helper/favorite2.png)

## Grading

在動畫的主頁中，可以透過點擊星星的方式，給予動畫評分，並且評分也會影響到主頁上呈現的動畫分數
![Grading](/Team-Project/Anime-Helper/grading.png)

## Comment

也可在喜歡的動畫的頁面下面留言或看到他人的留言，也有刪除留言的功能
![Comment](/Team-Project/Anime-Helper/comment.png)

# Contribution

**後端:**
**我: 資料清洗、資料庫建構、搜尋、收藏、留言API**
組員1: 資訊&排序、分類、登入登出、評分API
**前端:**
組員2: 主頁、註冊登入、評分系統、留言
組員3: Favorite List製作、評分系統、註冊登入(介面)、Profile頁面
組員4: Favorite List製作、網頁排版

# Ohter

本文僅簡單概述Project的部分成果，詳細的內容、分析詳見:
https://github.com/youzhe0305/Anime-Helper
裡面有完整的程式碼，以及報告、簡報
