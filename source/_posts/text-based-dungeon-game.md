---
title: C++ Text-Based Dungeon Game
top_img: transparent
date: 2024-04-30 18:12:50
tags:
    - C++
    - OOP
    - DS
    - Game
categories:
    - Personal-Project
cover: Personal-Project/text-based-dungeon-game/dungeon_room.png
description: 利用C++，基於物件導向做成的地下城遊戲
---

# Overview

此專案製作 "基於文字的地下城遊戲"，在遊戲中，將隨機生成的地下城，玩家需要擊敗裡面的所有怪物及探索完所有房間以獲得勝利

遊戲採用回合制戰鬥，設有裝備系統與技能系統，設計了25種裝備以及10種技能，每種技能都有自己的戰吼效果，以及不同的傷害計算、buff疊加效果

在地下城中，會遇到不同的怪物，打敗怪物時，可以從怪物身上學到技能，也有菁英怪，可以學到特別強力的技能。 一路上也會遇到不同的NPC，可以與之交易，或是觸發NPC特殊劇情，學會特殊技能。

## CoreTech

- C++
- OOP (Object-Oriented Programming)
- DS (Data Structure)

示意圖:
![](/Personal-Project/text-based-dungeon-game/dungeon_room.png)
![](/Personal-Project/text-based-dungeon-game/dungeon_battle.png)


介紹影片:
{% youtube GfCF2AjKM00 %}

# Background

於大一下學期，修習"資料結構與物件導向程式設計"之作業，要求使用C++及物件導向方法，建構一個地下城的遊戲。

# Feature

## Item

有**3類共25種的物品**，分別為 藥水、食物、裝備，可以從寶箱或是NPC得到。

## Skill

有**10種技能**，使用virtual function讓每個技能能有不同的傷害計算公式
每種技能都有**獨特施放效果(戰吼)**，並且不同技能會帶來**5種不同的buff**。

## Arena

以回合制式呈現現，可在戰鬥中施放技能、使用物品、逃跑等等。
打贏怪物後，怪物會掉落經驗值，並且玩家有**機率學到怪物的技能**

## Room

有怪物、寶箱、NPC等物件可互動。且怪物的生成隨著玩家等級，強度會改變。
有**4種不同的房間**，每種房間有對應的房間事件，在進入房間時有機會觸發，帶來不同的效果及buff, debuff。

## NPC

有**2種NPC**：商人以及特殊NPC
玩家可以與商人交易，獲得裝備、藥水、食物，商人販售的物品是隨機決定，有機會從裡面買到很好的物品。
特殊NPC則會觸發**特殊事件**，有劇情，並且在劇情結束後，玩家會從特殊NPC身上學到技能。

## Monster

實作了**5種不同的怪物**，每種怪物有不同的數值成長曲線，並且各自擁有獨特的技能。
其中也包含了精英怪物，擁有combo類型的技能，為地下城中的一大挑戰。

## Player

實作了 背包系統、裝備系統、等級系統，讓玩家可以如一般RPG一樣，做物品的裝備、使用，並且能透過打怪升級。

# UML

![](/Personal-Project/text-based-dungeon-game/dungeon_UML.jpg)


# Ohter

本文僅簡單概述Project的部分成果，詳細的內容、分析詳見:
https://github.com/youzhe0305/Text-Based-Dungeon-Game
裡面有完整的程式碼，以及報告、簡報



