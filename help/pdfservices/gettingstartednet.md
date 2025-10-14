---
title: 開始使用 Adobe PDF Services API 和 .Net
description: 開發人員只需幾分鐘即可開始使用，並準備好執行為存取所有可用 Web 服務所提供的範例檔案
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6675
thumbnail: KT-6675.jpg
keywords: 特色
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---

# 開始使用 Adobe PDF Services API 和 .Net

![製作 PDF 主圖影像](assets/GettingStartedJava_hero.jpg)

開發人員只需幾分鐘即可開始使用，並準備好執行可存取所有可用 Web 服務的範例檔案。 本教學課程將逐步引導您完成所有步驟，以使用 PDF Services .Net SDK 開始執行範例：

## 步驟 1：取得憑證和下載範例檔案

第一步是取得憑證 （API金鑰） 以解除鎖定使用。 [在這裡註冊免費試用](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 版，然後按兩下「開始使用」以建立您的新認證。

![步驟 1](assets/GettingStartedJava_step1.png)

請務必選擇「個人帳戶」來註冊免費試用版：

![個人](assets/GettingStartedJava_personal.png)

在下一個步驟中，您將選擇「PDF Services API Service」，然後新增認證的名稱和說明。

勾選「建立個人化程式代碼範例」。 選擇此選項可讓您的新認證自動新增至範例檔案，如此即可將您手動新增認證新增至專案中的步驟。

接下來，選擇Node.js語言接收Node.js特定範例，然後按下「建立認證」按鈕。

![憑據](assets/GettingStartedJava_credentials.png)

您會收到一個要下載的.zip檔案，名為 PDFToolsSDK-.NetSamples.zip，可以儲存到您的本機文件系統。

## 步驟 2：設定 .Net 環境並執行範例程序代碼

1. 下載並安裝 [.Net SDK](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. 將下載 **[!UICONTROL 的PDFToolsSDK-.NetSamples.zip]** 解壓縮，然後將內容解壓縮
1. cd 至範例根目錄 **[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. 從範例根目錄執行 `dotnet build`

   C：\Temp\PDFToolsAPI\ PDFToolsSDK-.netSamples\adobe-DC.PDFTools.SDK.NET.samples>dotnet 組建

   現在您已準備好執行範例檔案了！

   這些最後步驟會顯示如何使用 Word作的「建立 PDF」執行第一個樣本：

1. 從範例根目錄變更目錄到 CreatePDFFromDocx 資料夾，cd CreatePDFFromDocx/

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. 跑 `dotnet run CreatePDFFromDocx.csproj`

   C：\Temp\PDFToolsAPI\ PDFToolsSDK-.netSamples\adobe-DC.PDFTools.SDK.NET.Samples\CreatePDFFromDocx>dotnet 執行 CreatePDFFromDocx.csproj

您的 PDF 將會建立在輸出中指定的位置，預設是相同的資料夾。

## 最終想法

PDF Services API可將一般工作流程自動化，並將處理負擔轉移至雲端，以協助您消除手動流程。 在每個瀏覽器都以不同的方式處理 PDF 的世界中，利用Adobe PDF內嵌API以及 PDF 服務API，您可以建立精簡、可靠、可預測的流程，不論使用何種平臺或裝置，都可以隨時&#x200B;**正確**&#x200B;執行和顯示。

## 資源和後續步驟

* 如需其他協助和支援，請造訪 [[!DNL Adobe Acrobat Services] API 社](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all) 群論壇

* PDF 服務API [檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [PDF 服務常見問題](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) API問題

* [如有關於授權和定價的問題，請聯絡我們](https://www.adobe.com/go/pdftoolsapi_requestform)

* 相關文章

  [新的 PDF 服務API為檔工作流程提供更多功能](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [7 月版 [!DNL Adobe Acrobat Services]:P DF 內嵌與 PDF 服務](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
