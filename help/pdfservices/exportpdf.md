---
title: 使用 PDF 服務 API 將 PDF 匯出到 Word、PowerPoint 等格式
description: 學習如何使用範例檔案執行 PDF Services API 的匯出操作，支援 Node.js、Java 和 .Net 語言
feature: PDF Services API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-6674
thumbnail: KT-6674.jpg
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
TQID: https://experienceleague.adobe.com/CV-KH0fg1Tjnr7eCmNaFMtttWO1QUCyjfyIfwg1Vqm0
product_v2: id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2: id: b1809bd0-a86b-4991-8083-2e3b517fc3b8id: c4d07275-6387-4756-8bf7-681e581ffd27
subfeature_v2: id: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 524
ht-degree: 2%

---

# 使用 PDF 服務 API 將 PDF 匯出到 Word、PowerPoint 等格式

![建立 PDF 英雄圖片](assets/ExportPDF_hero.jpg)

Adobe PDF 服務 API 可將 PDF 檔案轉換為 MS Office、文字及圖片，使用 API 進行。 解鎖現有 PDF 進行內容編輯與分析有許多常見的使用案例，透過 PDF 服務 API，開發者可以輕鬆將此功能整合到現有系統與應用程式中。 將 PDF 檔案轉為 MS Word，以便內容編輯、核准，之後再發送簽名以建立自訂合約工作流程。 或將 PDF 內容匯出成 MS Excel 格式，用於發票、財務計算或資料分析。

匯出操作支援以下 PDF 檔案轉換：

* PDF 轉 Microsoft Word（DOC，DOCX）
* PDF 轉 Microsoft PowerPoint（PPTX）
* PDF 轉 Microsoft Excel （XLSX）
* PDF 轉文字（RTF）
* PDF 轉影像（JPEG、PNG）

在這個教學中，學習如何使用範例檔案執行你的第一個 PDF Services API 匯出操作，支援 Node.js、Java 和 .Net 語言。

## 步驟一：建立你的憑證並建立你的環境：

請使用以下入門教學建立 API 憑證、下載範例檔案並設定環境。

[開始使用 PDF Services API 與 Java](gettingstartedjava.md)

[開始使用 PDF Services API 與 .NET](gettingstartednet.md)

[如何開始使用 PDF 服務 API 與Node.js](createpdffromhtml.md)

## 步驟 2：使用範例檔案執行匯出 PDF 操作

**爪哇**

1. 開啟「命令提示字元」。

1. 將目錄改成你的範例程式碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples

1. 執行以下指令：

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

你的 PDF 是在 src/main/resources 目錄中建立的。

**.Net**

1. 開啟「命令提示字元」。

1. 將目錄改成你的範例程式碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. 再把目錄改成 ExportPDFtoDocx 目錄。

1. 執行以下指令：

   `dotnet run ExportPDFToDocx.csproj`

你的 PDF 也是在同一個目錄裡建立的。

**Node.js**

1. 開啟「命令提示字元」。

1. 將目錄改成你的範例程式碼目錄。

   例如，C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. 執行以下指令：

   `node src/ocr/ocr-pdf.js`

你的 PDF 會建立在輸出中指定的位置，預設是 pdfServicesSdkResult 目錄。

## 總結感想

你現在應該有一個可運作的範例，可以匯入現有應用程式，開始概念驗證。 在每個範例目錄中，你可以看到另一個範例，用來匯出 PDF 檔案成影像格式。 上述步驟同樣允許你執行該樣本。 若要更改格式，您可以將程式碼更新為您想要的新格式：

SupportedTargetFormats.PPTX

而目的地的結果是：

輸出/exportPdfOutput.PPTX

換成另一種格式。

## 資源與後續步驟

* 如需更多協助與支持，請造訪 [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all) 社群論壇

* PDF 服務 API [文件](https://www.adobe.com/go/pdftoolsapi_doc)

* [](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) PDF 服務 API 常見問題

* [如有關於授權與價格的問題，歡迎聯絡我們](https://www.adobe.com/go/pdftoolsapi_requestform)
