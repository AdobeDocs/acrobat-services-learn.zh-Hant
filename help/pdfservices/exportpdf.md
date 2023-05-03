---
title: 使用 PDF Services API將 PDF 轉存為 Word、PowerPoint 等
description: 瞭解如何使用 Node.js、JAVA 和 .Net 語言的範例檔案執行 PDF Services API轉存作業
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6674.jpg
kt: 6674
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 2%

---

# 使用 PDF Services API將 PDF 轉存為 Word、PowerPoint 等

![製作 PDF 主圖影像](assets/ExportPDF_hero.jpg)

Adobe PDF服務API使用 API 將 PDF 檔案轉換為 MS Office、文字和影像。 有許多常見的使用案例可解鎖現有的 PDF 以進行內容編輯和分析，而有了 PDF 服務，API開發人員可以輕鬆地將此功能整合到現有的系統和應用程式中。 將 PDF 檔案轉換為 MS Word，以編輯內容、核准和後續傳送以供簽署，以建立自訂的合約工作流程。 或者將 PDF 內容匯出為 MS Excel 格式，以供發票和財務計算或資料分析。

「轉存」操作支援下列 PDF 檔案轉換：

* PDF 轉換為 Microsoft Word （DOC、DOCX）
* PDF 轉換為 Microsoft PowerPoint （PPTX）
* PDF 轉換為 Microsoft Excel （XLSX）
* PDF 轉換為文字 （RTF）
* PDF 轉換為影像 （JPEG、PNG）

在本教學課程中，瞭解如何使用 Node.js、JAVA 和 .Net 語言的範例檔案，API執行第一個 PDF 服務轉存作業的基本知識。

## 步驟 1：建立認證並設定環境：

使用下列快速入門教學課程，建立您的API認證、下載範例檔案，並設定您的環境。

[PDF Services 快速入門 API 和 JAVA ](gettingstartedjava.md) [ 開始使用 PDF Services API 和 .Net ](gettingstartednet.md) [ 開始使用 PDF Services API 和 Node.js

](createpdffromhtml.md)

## 步驟 2：使用範例檔案執行轉存 pdf 作業

**Java**

1. 開啟「命令提示字元」。

1. 將目錄變更為範例程式碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples

1. 執行下列命令：

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

您的 PDF 是在 src/main/resources 目錄中建立。

**。網**

1. 開啟「命令提示字元」。

1. 將目錄變更為範例程式碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. 將目錄再次變更為 ExportPDFToDocx 目錄。

1. 執行下列命令：

   `dotnet run ExportPDFToDocx.csproj`

您的 PDF 是在同一個目錄中建立。

**Node.js**

1. 開啟「命令提示字元」。

1. 將目錄變更為範例程式碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. 執行下列命令：

   `node src/ocr/ocr-pdf.js`

您的 PDF 是建立在輸出中指定的位置，預設情況下是 pdfServicesSdkResult 目錄。

## 最終想法

您現在應該有一個可行的範例，可以匯入到現有的應用程式中，以開始概念驗證。 在每個範例目錄中，您可以看到另一個將 PDF 檔案轉存為影像格式的樣本。 上述相同步驟也可讓您執行該範例。 若要變更為其他格式，您可以將程式碼更新為您想要的新格式：

SupportedTargetFormats.PPTX

以及目的地結果：

output/exportPdfOutput.PPTX

轉換為其他格式。

## 資源和後續步驟

* 如需其他協助和支援，請造訪 [[!DNL Adobe Acrobat Services]  API 社 ](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 群論壇

* PDF 服務API [ 檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [PDF 服務常見問題 ](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) API問題

* [如有關于授權和定價的問題，請聯絡我們 ](https://www.adobe.com/go/pdftoolsapi_requestform)
