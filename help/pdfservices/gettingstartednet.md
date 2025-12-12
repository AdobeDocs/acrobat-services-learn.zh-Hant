---
title: Adobe PDF服務API和.Net入門
description: 開發人員只需幾分鐘就可以開始使用，即可運行提供用於訪問所有可用Web服務的示例檔案
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6675
thumbnail: KT-6675.jpg
keywords: 特色
exl-id: 22c59c75-fd99-4467-a6f6-917fb246469a
source-git-commit: bd53d86abb0e5f9ee302c39e07c00101e5a1f8ed
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---

# Adobe PDF服務API和.Net入門

![建立PDF英雄影像](assets/GettingStartedJava_hero.jpg)

開發人員只需幾分鐘就可以開始工作，隨時可以運行為訪問所有可用Web服務而提供的示例檔案。 本教程將指導您完成開始使用PDF服務.Net SDK運行示例的所有步驟：

## 步驟1：獲取憑據並下載示例檔案

第一步是獲取憑據（API密鑰）以解鎖使用。 [在此處註冊免費試用版](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)，然後按一下「開始」以建立新憑據。

![步驟1](assets/GettingStartedJava_step1.png)

選擇「個人帳戶」註冊免費試用非常重要：

![個人](assets/GettingStartedJava_personal.png)

在下一步中，您將選擇PDF服務API服務，然後為憑據添加名稱和說明。

此時會出現「建立個性化代碼示例」複選框。 選擇此選項可將新憑據自動添加到示例檔案中，這將保存將其添加到項目中的手動步驟。

接下來，選擇Node.js作為接收Node.js特定示例的語言，然後按一下「建立憑據」按鈕。

![憑據](assets/GettingStartedJava_credentials.png)

您將收到一個名為PDFToolsSDK-.NetSamples.zip的.zip檔案下載，該檔案可保存到您的本地檔案系統。

## 步驟2：設定.Net環境並運行示例代碼

1. 下載並安裝[.Net SDK](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install)
1. 解壓縮下載的&#x200B;**[!UICONTROL PDFToolsSDK-.NetSamples.zip]**&#x200B;並解壓縮內容
1. cd到示例根目錄&#x200B;**[!UICONTROL adobe-DC.PDFTools.SDK.NET.Samples]**
1. 從示例根目錄運行`dotnet build`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>dotnet內部版本

   現在，您已準備好運行示例檔案！

   以下最後步驟說明如何使用「從Word建立PDF」操作運行第一個示例：

1. 從示例根目錄更改目錄到CreatePDFFromDocx資料夾，cd CreatePDFFromDocx/

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Samples>cd CreatePDFFromDocx/

1. 運行`dotnet run CreatePDFFromDocx.csproj`

   C:\Temp\PDFToolsAPI\ PDFToolsSDK-.NetSamples\adobe-DC.PDFTools.SDK.NET.Saples\CreatePDFFromDocx>dotnet運行CreatePDFFromDocx.csproj

您的PDF將在輸出中指定的位置建立，預設位置是同一資料夾。

## 最後的想法

PDF服務API可通過自動化常見工作流和將處理負擔轉移到雲來幫助您消除手動流程。 在每個瀏覽器都以不同方式處理PDF的世界中，利用Adobe PDF嵌入式API和PDF服務API，您可以建立流線型、可靠且可預測的進程，這些進程每次&#x200B;**運行並正確顯示**，而不考慮平台或設備。

## 資源和後續步驟

* 有關其他幫助和支援，請訪問[[!DNL Adobe Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all)社區論壇

* PDF服務API [文檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [FAQ](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197)以瞭解PDF服務API問題

* [請與我們聯繫](https://www.adobe.com/go/pdftoolsapi_requestform)以瞭解有關許可和定價的問題

* 相關文章

  [新PDF服務API為文檔工作流提供了更多功能](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [ [!DNL Adobe Acrobat Services]的7月版：PDF嵌入和PDF服務](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)

