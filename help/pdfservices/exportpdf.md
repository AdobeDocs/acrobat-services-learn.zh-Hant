---
title: 使用PDF服務API將PDF導出到Word、PowerPoint等
description: 瞭解如何使用Node.js、Java和.Net語言的示例檔案運行PDF服務API導出操作
feature: PDF Services API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-6674
thumbnail: KT-6674.jpg
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
source-git-commit: ba73105ecf0bd27b7445ec4388fc4009eec273b8
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# 使用PDF服務API將PDF導出到Word、PowerPoint等

![建立PDF英雄影像](assets/ExportPDF_hero.jpg)

Adobe PDF服務API使用API將PDF檔案轉換為MS Office、文本和影像。 有許多常用用例可解鎖現有PDF以進行內容編輯和分析，而且PDF服務API開發人員可以輕鬆地將此功能整合到現有系統和應用程式中。 將PDF檔案轉換為MS Word以編輯內容、批准，然後發送簽名以建立自定義合同工作流。 或將PDF內容導出為MS Excel格式，用於發票和財務計算或資料分析。

導出操作支援以下PDF檔案轉換：

* PDF到MicrosoftWord(DOC、DOCX)
* PDF到MicrosoftPowerPoint(PPTX)
* PDF到MicrosoftExcel(XLSX)
* PDF到文本(RTF)
* PDF到影像(JPEG、PNG)

在本教程中，瞭解如何使用Node.js、Java和.Net語言的示例檔案運行第一個PDF服務API導出操作的基礎知識。

## 步驟1：建立憑據並設定環境：

使用下面的入門教程建立API憑據、下載示例檔案和設定環境。

[PDF服務API和Java入門](gettingstartedjava.md)

[PDF服務API和.Net入門](gettingstartednet.md)

[PDF服務API和Node.js入門](createpdffromhtml.md)

## 步驟2：使用示例檔案運行導出pdf操作

**Java**

1. 開啟命令提示符。

1. 將目錄更改為示例代碼目錄。

   例如，C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples

1. 運行以下命令：

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

您的PDF在src/main/resources目錄中建立。

**.Net**

1. 開啟命令提示符。

1. 將目錄更改為示例代碼目錄。

   例如，C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. 再次將目錄更改為ExportPDFtoDocx目錄。

1. 運行以下命令：

   `dotnet run ExportPDFToDocx.csproj`

您的PDF建立在同一目錄下。

**Node.js**

1. 開啟命令提示符。

1. 將目錄更改為示例代碼目錄。

   例如，C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. 運行以下命令：

   `node src/ocr/ocr-pdf.js`

您的PDF是在輸出中指定的位置建立的，預設為pdfServicesSdkResult目錄。

## 最後的想法

現在，您應該有一個工作示例，可以導入到現有應用程式中，以開始概念驗證。 在每個示例目錄中，您都可以看到另一個示例將PDF檔案導出為影像格式。 上面的相同步驟也允許您運行該示例。 要更改為其他格式，可以將代碼更新為所需的新格式：

SupportedTargetFormats.PPTX

目標結果是：

output/exportPdfOutput.PPTX

轉到其他格式。

## 資源和後續步驟

* 有關其他幫助和支援，請訪問[[!DNL Adobe Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all)社區論壇

* PDF服務API [文檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [FAQ](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197)以瞭解PDF服務API問題

* [請與我們聯繫](https://www.adobe.com/go/pdftoolsapi_requestform)以瞭解有關許可和定價的問題
