---
title: 開始使用 Adobe PDF 服務 API 與 Java
description: 開發者只需幾分鐘即可開始使用，並提供可使用的所有網路服務範例檔案
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6676
thumbnail: KT-6676.jpg
exl-id: 4a8f2119-c464-496b-bdc8-35dd387bef25
TQID: https://experienceleague.adobe.com/ikQahJSwQ9NQPSB1m-DoaTAOHObGXTBr0mZYDMGu3QI
product_v2: id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2: id: b1809bd0-a86b-4991-8083-2e3b517fc3b8
subfeature_v2: id: c4b1e8f2-d9a8-4792-b5e4-be52bd870028id: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 541
ht-degree: 0%

---

# 開始使用 Adobe PDF 服務 API 與 Java

![建立 PDF 英雄圖片](assets/GettingStartedJava_hero.jpg)

開發者只需幾分鐘即可開始使用，提供可執行的範例檔案，方便存取所有可用的網路服務。 本教學將帶你一步步開始使用 PDF Services Java SDK 執行範例：

## 步驟 1：取得憑證並下載範例檔案

第一步是取得憑證（API 金鑰）以解鎖使用權限。 [請在此](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 註冊免費試用，並點擊「開始使用」以建立您的新憑證。

![步驟一](assets/GettingStartedJava_step1.png)

選擇「個人帳號」以註冊免費試用非常重要：

![個人生活](assets/GettingStartedJava_personal.png)

下一步你會選擇 PDF 服務 API 服務，然後為你的憑證加上名稱和描述。

有一個「建立個人化程式碼範例」的勾選框。 選擇此選項，即可自動將新憑證加入範例檔案，省去手動將憑證加入專案的步驟。

接著，選擇 Java 作為你的語言以接收 Java 專屬範例，然後點擊「建立憑證」按鈕。

![資歷](assets/GettingStartedJava_credentials.png)

你會收到一個名為 PDFToolsSDK-JavaSamples.zip 的.zip檔案，可以儲存到你本地的檔案系統。

## 步驟 2：設定你的 Java 環境

1. 如果你還沒安裝 Java 8 或更高](https://www.oracle.com/java/technologies/javase-downloads.html)版本，建議[安裝。
1. 跑去 `javac -version` 驗證你的安裝。
1. 確認 JDK bin 資料夾是否包含在 PATH 變數中（方法依作業系統而異）。
1. 如果你還沒安裝 Maven](https://maven.apache.org/install.html)，請[用你偏好的工具安裝。

個人化範例涵蓋從現成範例程式碼、嵌入的憑證 json 檔案，到預先設定的連線與相依關係。

1. 下載 [示範專案](https://github.com/adobe/pdftools-java-sdk-samples)。
1. 用 Maven 建立範例專案：mvn clean install。
1. 在命令列或你偏好的 IDE 上測試範例程式碼。

## 總結感想

PDF 服務 API 能幫助您自動化常見工作流程，將處理負擔轉移至雲端，從而消除手動流程。 在每個瀏覽器對 PDF 的處理方式都不一樣的世界裡，利用 Adobe PDF 嵌入 API 與 PDF 服務 API，你可以打造流暢、可靠且可預測的流程，無論平台或裝置如何，都能&#x200B;**每次正確**&#x200B;執行與顯示。

## 資源與後續步驟

* 如需更多協助與支援，請造訪 Adobe [[!DNL Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all) 社群論壇

* PDF 服務 API [文件](https://www.adobe.com/go/pdftoolsapi_doc)

* [](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) PDF 服務 API 常見問題

* [如有關於授權與價格的問題，歡迎聯絡我們](https://www.adobe.com/go/pdftoolsapi_requestform)

* 相關文章

  [新的 PDF 服務 API 為文件工作流程提供更多功能](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [七月發布 [!DNL Adobe Acrobat Services]：PDF 嵌入與 PDF 服務](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
