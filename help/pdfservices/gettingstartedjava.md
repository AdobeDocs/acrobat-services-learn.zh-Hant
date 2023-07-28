---
title: 開始使用 Adobe PDF Services API 和 JAVA
description: 開發人員只需幾分鐘即可開始使用，並準備好執行為存取所有可用 Web 服務所提供的範例檔案
type: Tutorial
role: Developer
level: Beginner
feature: PDF Services API
thumbnail: KT-6676.jpg
kt: 6676
exl-id: 4a8f2119-c464-496b-bdc8-35dd387bef25
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 0%

---

# 開始使用 Adobe PDF Services API 和 JAVA

![製作 PDF 主圖影像](assets/GettingStartedJava_hero.jpg)

開發人員只需幾分鐘即可開始使用，並準備好執行可存取所有可用 Web 服務的範例檔案。 本教學課程將逐步引導您完成所有步驟，以使用 PDF Services JAVA SDK 開始執行範例：

## 步驟 1：取得憑證和下載範例檔案

第一步是取得憑證 （API金鑰） 以解除鎖定使用。 [在這裡註冊免費試用 ](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 版，然後按一下「開始使用」以建立您的新認證。

![步驟 1](assets/GettingStartedJava_step1.png)

請務必選擇「個人帳戶」來註冊免費試用版：

![個人](assets/GettingStartedJava_personal.png)

在下一個步驟中，您將選擇「PDF Services API Service」，然後新增認證的名稱和說明。

勾選「建立個人化程式碼範例」。 選擇此選項可讓您的新認證自動新增至範例檔案，如此即可將您手動新增認證新增至專案中的步驟。

接著，選擇以 JAVA 作為您的語言來接收 JAVA 特定範例，然後按一下「建立認證」按鈕。

![憑據](assets/GettingStartedJava_credentials.png)

您會收到一個 .zip 檔案，要下載名為 PDFToolsSDK-JAVASamples.zip 的檔案，該檔案可以儲存到您的本機檔案系統。

## 步驟 2：設定 JAVA 環境

1. 如果您尚未安裝 [ JAVA 8 或更高版本 ](https://www.oracle.com/java/technologies/javase-downloads.html) ，請安裝。
1. 執行 `javac -version` 以驗證安裝。
1. 確認 JDK bin 資料夾包含在 PATH 變數中 （方法會依作業系統而異）。
1. 如果您尚未安裝 [ Maven，請使用您偏好的工具安裝 Maven ](https://maven.apache.org/install.html) 。

個人化範例提供從準備到執行的範例程式碼、嵌入式認證 json 檔案，以及預先設定的連線到相依性的所有功能。

1. 下載 [ 範例專案 ](https://github.com/adobe/pdftools-java-sdk-samples) 。
1. 使用 Maven 建立範例專案：mvn Clean install。
1. 在命令列或您偏好的 IDE 中測試範例代碼。

## 最終想法

PDF Services API可將一般工作流程自動化，並將處理負擔轉移至雲端，以協助您消除手動流程。 在每個瀏覽器都以不同的方式處理 PDF 的世界中，利用Adobe PDF內嵌API以及 PDF 服務API，您可以建立精簡、可靠、可預測的流程，不論使用何種平臺或裝置，都可以隨時 **正確** 執行和顯示。

## 資源和後續步驟

* 如需其他協助和支援，請造訪 Adobe [[!DNL Acrobat Services]  API 社 ](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) 群論壇

* PDF 服務API [ 檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [PDF 服務常見問題 ](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) API問題

* [如有關于授權和定價的問題，請聯絡我們 ](https://www.adobe.com/go/pdftoolsapi_requestform)

* 相關文章

  [新的 PDF 服務API為檔工作流程提供更多功能](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [7 月版  [!DNL Adobe Acrobat Services] ：PDF 內嵌與 PDF 服務](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
