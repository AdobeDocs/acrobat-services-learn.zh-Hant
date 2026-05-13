---
title: 開始使用 Adobe PDF Services API 與 .NET
description: 開發者只需幾分鐘即可開始使用，並提供可使用的所有網路服務範例檔案
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6675
thumbnail: KT-6675.jpg
keywords: 特色
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
TQID: https://experienceleague.adobe.com/H10l4sa6OGC15UBDjMHlVFfANtjFro9YnRnGU17K2kM
product_v2: id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2: id: b1809bd0-a86b-4991-8083-2e3b517fc3b8
subfeature_v2: id: c4b1e8f2-d9a8-4792-b5e4-be52bd870028id: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 566
ht-degree: 0%

---

# 開始使用 Adobe PDF Services API 與 .Net

![建立 PDF 英雄圖片](assets/GettingStartedJava_hero.jpg)

開發者只需幾分鐘即可開始使用，提供可執行的範例檔案，方便存取所有可用的網路服務。 這個教學會帶你一步步開始使用 PDF Services .Net SDK 執行這些範例：

## 步驟 1：取得憑證並下載範例檔案

第一步是取得憑證（API 金鑰）以解鎖使用權限。 [請在此](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 註冊免費試用，並點擊「開始使用」以建立您的新憑證。

![步驟一](assets/GettingStartedJava_step1.png)

選擇「個人帳號」以註冊免費試用非常重要：

![個人生活](assets/GettingStartedJava_personal.png)

下一步你會選擇 PDF 服務 API 服務，然後為你的憑證加上名稱和描述。

有一個「建立個人化程式碼範例」的勾選框。 選擇此選項，即可自動將新憑證加入範例檔案，省去手動將憑證加入專案的步驟。

接著，選擇Node.js語言以接收Node.js特定範例，並點擊「建立憑證」按鈕。

![資歷](assets/GettingStartedJava_credentials.png)

你會收到一個.zip檔案，叫做 PDFToolsSDK-.NetSamples.zip，可以儲存到你本地的檔案系統。

## 步驟 2：設定你的 .Net 環境並執行範例程式碼

1. 下載並安裝 [.Net SDK](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. 解壓下載的 **[!UICONTROL PDFToolsSDK-.NetSamples.zip]** 並解壓內容
1. cd 轉為 samples 根目錄 **[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. 從 samples 的根目錄執行 `dotnet build`

   C：\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>dotnet build

   現在你準備好執行樣本檔案了！

   以下最後步驟示範如何使用從 Word 建立 PDF 操作來執行第一個範例：

1. 從範例根目錄變更目錄到 CreatePDFFromDocx 資料夾，CD CreatePDFFromDocx/

   C：\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. 跑 `dotnet run CreatePDFFromDocx.csproj`

   C：\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples\CreatePDFFromDocx>dotnet run CreatePDFFromDocx.csproj

你的 PDF 會建立在輸出中指定的位置，預設是同一個資料夾。

## 總結感想

PDF 服務 API 能幫助您自動化常見工作流程，將處理負擔轉移至雲端，從而消除手動流程。 在每個瀏覽器對 PDF 的處理方式都不一樣的世界裡，利用 Adobe PDF 嵌入 API 與 PDF 服務 API，你可以打造流暢、可靠且可預測的流程，無論平台或裝置如何，都能&#x200B;**每次正確**&#x200B;執行與顯示。

## 資源與後續步驟

* 如需更多協助與支持，請造訪 [[!DNL Adobe Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all) 社群論壇

* PDF 服務 API [文件](https://www.adobe.com/go/pdftoolsapi_doc)

* [](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) PDF 服務 API 常見問題

* [如有關於授權與價格的問題，歡迎聯絡我們](https://www.adobe.com/go/pdftoolsapi_requestform)

* 相關文章

   * [新的 PDF 服務 API 為文件工作流程提供更多功能](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)
   * [七月發布 [!DNL Adobe Acrobat Services]：PDF 嵌入與 PDF 服務](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
