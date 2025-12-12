---
title: 使用Adobe PDF服務API進行OCRPDF檔案
description: 使用OCR（光學字元識別），您可以解鎖掃描的PDF，以提取文本並建立可搜索的檔案
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6677
thumbnail: KT-6677.jpg
exl-id: 61a9a2d1-94c3-41c2-8f90-a56a938ef245
source-git-commit: bd53d86abb0e5f9ee302c39e07c00101e5a1f8ed
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# 使用Adobe PDF服務API對OCRPDF檔案

![建立PDF英雄影像](assets/OCR_hero.jpg)

使用OCR（光學字元識別），您可以解鎖掃描的PDF，以提取文本並建立可搜索的檔案。 使用我們功能強大的基於雲的API，將OCR整合到任何文檔工作流中，從而為歸檔、複製文本和建立可搜索的文檔索引提供完美的解決方案。 從掃描的PDF儲存庫建立可搜索的存檔，以解鎖重要資訊並通過快速搜索節省時間。 或者將OCR應用於上載的PDF，以便編輯掃描，以在上載工作流中使用。

開發人員只需幾分鐘就可以開始使用為OCR提供的示例檔案。

在本教程中介紹了如何使用Node.js、Java和.Net語言的示例檔案運行第一個PDF服務API OCR操作的基本知識。

## 步驟1：建立憑據並設定環境

使用下面的入門教程建立API憑據、下載示例檔案和設定環境。

[PDF服務API和Java入門](gettingstartedjava.md)

[PDF服務API和.Net入門](gettingstartednet.md)

[PDF服務API和Node.js入門](createpdffromhtml.md)

## 運行示例檔案中提供的OCR示例

預設情況下，我們的OCR操作允許使用英語語言環境，但也支援德語、法語、丹麥語和[其他語言](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#ocr-with-explicit-language)。 預設為en-us語言環境。

在將選項與包括特定區域設定的OCR操作一起傳入時，該方法還接受「type」參數，該參數具有兩個選項：

* SEARCHABLE_IMAGE：在清除過程中修改原始影像（例如，解析它），然後將不可見的文本層置於其上。 此類型會刪除不需要的對象，並且在某些情況下可能會生成更易讀的文檔。

* SEARCHABLE_IMAGE_EXACT：確保文本可搜索和選擇。 此選項保留原始影像，並在其上放置不可見的文本圖層。 建議用於要求原始影像保真度最高的情況。

**Java**

1. 開啟命令提示符。

1. 將目錄更改為示例代碼目錄。

   例如，C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples>。

1. 運行以下命令：

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.ocrpdf.OcrPDF`

您的PDF將在src/main/resources目錄中建立。

**.Net**

1. 開啟命令提示符。

1. 將目錄更改為示例代碼目錄。

   例如，C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. 再次將目錄更改為OcrPDF目錄。

1. 運行以下命令：

   `dotnet run OcrPDF.csproj`

您的PDF將建立在同一目錄中。

**Node.js**

1. 開啟命令提示符。

1. 將目錄更改為示例代碼目錄。

   例如，C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. 運行以下命令：

   `node src/ocr/ocr-pdf.js`

您的PDF將在輸出中指定的位置建立，該位置預設為輸出目錄。

## 最後的想法

使用示例檔案執行這些簡單步驟後，您應該有一個可以構建的工作示例。 除了本教程中使用的OCR示例外，還有一個使用前面討論的支援類型和區域設定選項的OCR示例。

在此，您只需替換示例中的輸入和輸出檔案，即可使用您自己的PDF完成您自己的使用案例的概念驗證。

![概念驗證](assets/OCR_poc.png)

## 資源和後續步驟

* 有關其他幫助和支援，請訪問Adobe[[!DNL Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all)社區論壇

* PDF服務API [文檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [FAQ](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197)以瞭解PDF服務API問題

* [請與我們聯繫](https://www.adobe.com/go/pdftoolsapi_requestform)以瞭解有關許可和定價的問題

