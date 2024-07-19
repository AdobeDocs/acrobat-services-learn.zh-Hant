---
title: 使用 Adobe PDF 服務 API 轉化為 OCR PDF 檔案
description: 使用 OCR（光學字元識別），您可以解鎖掃描的 PDF 以擷取文字並創建可搜尋的檔
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6677
thumbnail: KT-6677.jpg
exl-id: 61a9a2d1-94c3-41c2-8f90-a56a938ef245
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# 使用 Adobe PDF 服務 API 到 OCR PDF 檔案

![Create PDF 英雄影像](assets/OCR_hero.jpg)

使用 OCR（光學字元識別），您可以解鎖掃描的 PDF 以擷取文字並創建可搜尋的檔。 使用我們強大的基於雲端 的 API，將 OCR 集成到任何檔 工作流程中，以獲得存檔、複製文本和創建可搜尋檔索引的完美解決方案。 從掃描的 PDF 儲存庫建立可搜尋的檔案，以解鎖重要資訊並快速搜尋來節省時間。 或是從上傳的掃描內容中將 OCR 套用至 PDF，以供編輯以用於入門工作流程。

開發人員只需幾分鐘即可開始使用，並準備好執行為 OCR 提供的範例檔案。

本教學課程介紹如何使用 Node.js、Java 和 .Net 語言的範例檔案，API執行第一個 PDF Services API操作的基本知識。

## 步驟 1：建立認證並設定環境

請使用下方的快速入門教學課程，建立您的API認證、下載範例檔案，並設定您的環境。

[PDF Services API 和 Java 快速入門](gettingstartedjava.md)

[PDF Services API和 .Net 快速入門](gettingstartednet.md)

[開始使用 PDF Services API和Node.js](createpdffromhtml.md)

## 執行範例檔案中提供的 OCR 範例

我們的 OCR 作業預設提供英文地區設定，但也支援德文、法文、丹麥文和其他 [語言](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#ocr-with-explicit-language)。 默認值為「en-us」地區設定。

當您使用 OCR 操作傳入選項（包括特定區域設置）時，該方法還接受具有兩個選項的「type」參數：

* SEARCHABLE_IMAGE：在清理過程中修改原始圖像（例如，糾偏原始圖像），然後在其上放置不可見的文本圖層。 此類型會刪除不需要的專案，在某些情況下可能會導致檔更具可讀性。

* SEARCHABLE_IMAGE_EXACT：確保文本可搜索和可選。 此選項會保留原始影像，並在影像上放置一個不可見文字圖層。 建議用於需要對原始圖像具有最大保真度的情況。

**爪哇島**

1. 打開命令提示符。

1. 將目錄更改為範例代碼目錄。

   例如 C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples>。

1. 執行以下命令：

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.ocrpdf.OcrPDF`

您的 PDF 將在 src/main/resources 目錄中創建。

**。網**

1. 開啟命令提示字元。

1. 將目錄變更為範例程式代碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. 將目錄再次變更為 OcrPDF 目錄。

1. 執行下列命令：

   `dotnet run OcrPDF.csproj`

您的 PDF 將會在同一個目錄中建立。

**Node.js**

1. 開啟命令提示字元。

1. 將目錄變更為範例程式代碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. 執行下列命令：

   `node src/ocr/ocr-pdf.js`

您的 PDF 將在輸出中指定的位置創建，預設情況下是輸出目錄。

## 結語

通過使用示例文件的這些簡單步驟，您應該有一個可以版本編號的工作示例。 除了我們在此教學課程中使用的 OCR 示例之外，還有另一個使用前面討論的受支持的類型和區域設置選項的 OCR 示例。

從這裡，您可以簡單地替換位於示例中的輸入和輸出文件，以使用您自己的 PDF 為您自己的用例完成概念證明。

![概念驗證](assets/OCR_poc.png)

## 資源和後續步驟

* 如需其他協助和支援，請造訪 Adobe [[!DNL Acrobat Services] API 社](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 群論壇

* PDF 服務API [檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [PDF 服務常見問題](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) API問題

* [如有關於授權和定價的問題，請聯絡我們](https://www.adobe.com/go/pdftoolsapi_requestform)
