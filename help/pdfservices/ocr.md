---
title: 將 Adobe PDF 服務API用於 OCR PDF 檔案
description: 透過 OCR （光學字元辨識），您可以解除鎖定掃描的 PDF 以擷取文字並建立可搜尋的檔案
type: Tutorial
role: Developer
level: Beginner
feature: PDF Services API
thumbnail: KT-6677.jpg
jira: KT-6677
keywords: 英雄
exl-id: 61a9a2d1-94c3-41c2-8f90-a56a938ef245
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 2%

---

# 將「Adobe PDF服務」API至 OCR PDF 檔案

![製作 PDF 主圖影像](assets/OCR_hero.jpg)

透過 OCR （光學字元辨識），您可以解除鎖定掃描的 PDF 以擷取文字並建立可搜尋的檔案。 使用我們強大的雲端型 API，將 OCR 整合到任何檔工作流程中，為封存、複製文字和建立可搜尋的檔索引提供完美的解決方案。 從掃描的 PDF 儲存庫建立可搜尋的檔案，以解鎖重要資訊並快速搜尋來節省時間。 或是從上傳的掃描內容中將 OCR 套用至 PDF，以供編輯以用於入門工作流程。

開發人員只需幾分鐘即可開始使用，並準備好執行為 OCR 提供的範例檔案。

本教學課程介紹如何使用 Node.js、JAVA 和 .Net 語言的範例檔案，API OCR 作業執行第一個 PDF 服務的基本知識。

## 步驟 1：建立認證並設定環境

請使用下方的快速入門教學課程，建立您的API認證、下載範例檔案，並設定您的環境。

[PDF Services API 和 JAVA 快速入門](gettingstartedjava.md)

[PDF Services API和 .Net 快速入門](gettingstartednet.md)

[PDF Services API 和 Node.js 快速入門](createpdffromhtml.md)

## 執行範例檔案中提供的 OCR 範例

我們的 OCR 作業預設提供英文地區設定，但也支援德文、法文、丹麥文和其他 [ 語言 ](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#ocr-with-explicit-language) 。 預設值為「en-us」地區設定。

當您以 OCR 操作 （包括特定地區設定） 傳遞選項時，該方法也會接受有兩個選項的「type」參數：

* SEARCHABLE_IMAGE：在清理過程中修改原始影像 （例如將原始影像以桌面處理），然後在影像上放置不可見的文字圖層。 此類型可移除不想要的假影，在某些情況下可能會導致更容易閱讀的檔。

* SEARCHABLE_IMAGE_EXACT：確保文字可搜尋及選取。 此選項會保留原始影像，並將不可見的文字圖層放置在影像上。 建議用於要求原始影像最大保真的情況。

**Java**

1. 開啟「命令提示字元」。

1. 將目錄變更為範例程式碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples>。

1. 執行下列命令：

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.ocrpdf.OcrPDF`

您的 PDF 將會建立在 src/main/resources 目錄中。

**。網**

1. 開啟「命令提示字元」。

1. 將目錄變更為範例程式碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. 將目錄再次變更為 OcrPDF 目錄。

1. 執行下列命令：

   `dotnet run OcrPDF.csproj`

您的 PDF 將會在同一個目錄中建立。

**Node.js**

1. 開啟「命令提示字元」。

1. 將目錄變更為範例程式碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. 執行下列命令：

   `node src/ocr/ocr-pdf.js`

您的 PDF 將會建立在輸出中指定的位置，預設是輸出目錄。

## 最終想法

透過這些使用範例檔案的簡單步驟，您應該會有可建立的工作範例。 除了本教學課程所使用的 OCR 範例外，還有另一個使用先前討論的支援類型和地區設定選項的 OCR 範例。

在這裡，您可以直接取代位於範例中的輸入和輸出檔案，以使用自己的 PDF 來完成您自己的使用案例的概念驗證。

![概念證明](assets/OCR_poc.png)

## 資源和後續步驟

* 如需其他協助和支援，請造訪 Adobe [[!DNL Acrobat Services]  API 社 ](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 群論壇

* PDF 服務API [ 檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [PDF 服務常見問題 ](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) API問題

* [如有關于授權和定價的問題，請聯絡我們 ](https://www.adobe.com/go/pdftoolsapi_requestform)
