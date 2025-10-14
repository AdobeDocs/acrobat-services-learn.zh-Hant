---
title: 在幾分鐘內使用 PDF Services API 和Node.js從 HTML 或 MS Office 建立 PDF
description: 在 PDF 服務 API 中，有幾個可用服務可以建立和作 PDF，或從 PDF 轉存為 MS Office 和其他格式
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6673
thumbnail: KT-6673.jpg
exl-id: 1bd01bb8-ca5e-4a4a-8646-3d97113e2c51
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '673'
ht-degree: 0%

---

# 在幾分鐘內使用 PDF Services API 和Node.js從 HTML 或 MS Office 建立 PDF

![製作 PDF 主圖影像](assets/createpdffromhtml_hero.jpg)

新的「Adobe PDF服務」API讓開發人員可以自由地挑選和選擇多個強大的 PDF 處理服務，以滿足複雜的業務工作流程需求，讓檔工作流程數位化檔工作流程從未如此簡單。 透過這些隨時可用的雲端網路服務，可以簡化複雜的架構、實施策略和技術升級。

在 PDF 服務 API 中，有數項可用服務可用來建立和作 PDF，或從 PDF 轉存為 MS Office 和其他格式。

* 從靜態或動態 HTML、MS Word、PowerPoint、Excel 等格式建立 PDF 檔案
* Export PDF MS Word、PowerPoint、Excel 等
* OCR 可辨識 PDF 檔案中的文字並啟用文件搜尋
* 開啟檔時以密碼保護 PDF
* 將 PDF 頁面或 PDF 檔案合併為單一 PDF
* 壓縮 PDF 以縮減透過電子郵件或在線共用的大小
* 線性化以優化 PDF 以便在網頁上快速檢視
* 使用插入、取代、重新排序、刪除和旋轉服務來組織 PDF 頁面

開發人員只需幾分鐘即可開始使用，並準備好執行可存取所有可用 Web 服務的範例檔案。 以下是開始著手的說明。

## 取得憑證和下載範例檔案

第一步是取得憑證 （API金鑰） 以解除鎖定使用。 [在這裡註冊免費試用](https://www.adobe.com/go/dcsdks_credentials) 版，然後按兩下「開始使用」以建立您的新認證。

![API鍵](assets/apikey.png)

請務必選擇「個人帳戶」來註冊免費試用版：

![個人帳戶](assets/personalaccount.png)

在下一個步驟中，您將選擇「PDF Services API Service」，然後新增認證的名稱和說明。

勾選「建立個人化程式代碼範例」。 選擇此選項可讓您跳過手動步驟，將新的認證自動新增至範例檔案。

接下來，選擇Node.js語言接收Node.js特定範例，然後按下「建立認證」按鈕。

![建立認證](assets/createcredentials.png)

您會收到一個要下載的.zip檔案，名為 PDFToolsSDK-Node.jsSamples.zip，可以儲存到您的本機文件系統。

## 將您的認證新增至程式代碼範例

如果您選擇「建立個人化程式代碼範例」選項，就不必手動將用戶端 ID 新增至程式代碼範例檔案，也可以略過下一步，直接前往下方的「執行程式代碼範例」區段。

如果您未選擇「建立個人化程式代碼範例」選項，則必須從 Adobe.io Console 複製用戶端 ID （API 鍵）：

![程式代碼範例](assets/codesample.png)

將PDFToolsSDK-Node.jsSamples.zip的內容解壓縮。

前往 adobe-dc-pdf-tools-sdk-node-samples 檔案夾下的根目錄。

使用任何文字編輯器或 IDE 開啟pdftools-api-credentials.json。

將認證貼入程式代碼中的用戶端 ID 欄位：

```javascript
{
 "client_credentials": {
  "client_id": "abcdefghijklmnopqrstuvwxyz",
```

儲存盤案並繼續執行下一個步驟，以執行程式碼範例。

## 執行您的第一個程式代碼範例

使用命令提示字元，前往 adobe-dc-pdf-tools-sdk-node-samples 檔案夾下的根目錄。

輸入 npm 安裝：

C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>npm install

現在您已準備好執行範例檔案了！

對於第一個範例，請建立 PDF：

當仍處於命令提示字元中時，請使用下列命令執行建立 PDF 範例：

C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>node src/createpdf/create-pdf-from-docx.js

輸出範例：

![輸出範例](assets/exampleoutput.png)

您的 PDF 將會建立在輸出中指定的位置，預設為 pdfServicesSdkResult 目錄。

## 資源和後續步驟

* 如需其他協助和支援，請造訪 Adobe [[!DNL Acrobat Services] API 社](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all) 群論壇

PDF 服務API [檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [PDF 服務常見問題](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) API問題

* [如有關於授權和定價的問題，請聯絡我們](https://www.adobe.com/go/pdftoolsapi_requestform)

* 相關文章：
  [新的 PDF 服務API為檔工作流程提供更多功能](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [7 月版 [!DNL Adobe Acrobat Services]:P DF 內嵌與 PDF 服務](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
