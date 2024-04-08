---
title: 檔產生API教學課程
description: 檔產生概覽API教學課程
feature: Document Generation API
role: Developer
level: Beginner, Intermediate, Experienced
type: Tutorial
jira: KT-7480
thumbnail: KT-7480.jpg
exl-id: 519a41a2-33af-4022-8919-2cb69995c46c
source-git-commit: 5354dc45fe27cd4dccbbe62d502277bc44441d9b
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---


# 檔產生API教學課程

「檔案產生」API從 Word 範本和 JSON 資料建立 PDF 和 Word 檔。

>[!NOTE]
>
>PDF 服務API包含「檔案產生API」。

<table style="table-layout:fixed">
<tr>
 <td>
   <a href="automate-doc-gen.md">
      <img alt="自動化文件產生" src="assets/automate-doc-gen.png" />
   </a>
  </td>
    <td>
    <img alt="間隔" src="../assets/WhiteBanner_Placeholder.png" />
    <div>
    <br>
  </td>
   <td>
    <img alt="間隔" src="../assets/WhiteBanner_Placeholder.png" />
    <div>
    <br>
  </td>
  </td>
   <td>
    <img alt="間隔" src="../assets/WhiteBanner_Placeholder.png" />
    <div>
    <br>
  </td>
</tr>
</table>

## 建立範本

產生API檔會接受檔範本 （包含範本標籤） 以及輸入數據，以產生最終檔。 系統會根據對應至數據輸入的實際值，將文件範本中的所有範本標籤替換為動態內容，藉此產生最終檔。

<table style="table-layout:fixed">
<tr>
 <td>
   <a href="taggeroverview.md">
      <img alt="Adobe檔產生索引標籤概覽" src="assets/Taggeroverview_thumb.png" />
   </a>
    <div>
   <a href="taggeroverview.md"><strong>Adobe檔產生索引標籤概覽</strong></a>
    </div>
    <em>取得專為使用 Adobe 檔產生工具所設計的 Adobe Document Generation Tagger 概覽API</em>
    <br>
  </td>
  <td>
   <a href="taggeraddtexttags.md">
      <img alt="新增文字標籤" src="assets/Taggertexttags_thumb.png" />
   </a>
    <div>
   <a href="taggeraddtexttags.md"><strong>新增文字標籤</strong></a>
    </div>
    <em>瞭解如何使用 Adobe Document Generation Tagger 將文字標籤新增至 Microsoft Word 範本，以搭配 Adobe 產生檔API</em>
    <br>
  </td>
  <td>
   <a href="taggeraddimagetags.md">
      <img alt="新增影像標籤" src="assets/Taggerimagetags_thumb.png" />
   </a>
    <div>
   <a href="taggeraddimagetags.md"><strong>新增影像標籤</strong></a>
    </div>
    <em>瞭解如何使用 Adobe Document Generation Tagger 將影像卷標新增至 Microsoft Word 範本，以使用「Adobe產生檔」將影像動態推送至檔API</em>
    <br>
  </td>
  <td>
   <a href="taggertables.md">
      <img alt="新增表格和清單標籤" src="assets/Taggertables_thumb.png" />
   </a>
    <div>
   <a href="taggertables.md"><strong>新增表格和清單標籤</strong></a>
    </div>
    <em>瞭解如何使用 Adobe Document Generation Tagger 將表格和清單標籤新增至 Microsoft Word 範本，以使用「檔案產生Adobe」功能，根據數據動態新增表格或清單列API</em>
    <br>
  </td>
</tr>
<tr>
  <td>
   <a href="taggercalculations.md">
      <img alt="設定數值計算標籤" src="assets/Taggercalculations_thumb.png" />
   </a>
    <div>
   <a href="taggercalculations.md"><strong>設定數值計算標籤</strong></a>
    </div>
    <em>瞭解如何使用 Adobe Document Generation Tagger 在 Microsoft Word 範本中設定數位計算標籤，以使用「文件產生預設」Adobe計算匯整或整理數據值API</em>
    <br>
  </td>
  <td>
   <a href="taggerconditional.md">
      <img alt="設定條件式內容" src="assets/Taggerconditional_thumb.png" />
   </a>
    <div>
   <a href="taggerconditional.md"><strong>設定條件式內容</strong></a>
    </div>
    <em>瞭解如何使用 Adobe Document Generation Tagger 在 Microsoft Word 範本中設定區段，以使用「Adobe產生檔」功能，根據數據動態納入或排除檔區段API</em>
    <br>
  </td>
  <td>
    <img alt="間隔" src="../assets/GrayBanner_Placeholder.png" />
    <div>
    <br>
  </td>
   <td>
    <img alt="間隔" src="../assets/GrayBanner_Placeholder.png" />
    <div>
    <br>
  </td>
</tr>
</table>
