---
title: 利用 PDF 服務 API 和 Node.js，幾分鐘內就能從 HTML 或 MS Office 製作 PDF。
description: 在 PDF 服務 API 中，有多種可用服務可用於建立與操作 PDF，或從 PDF 匯出至 MS Office 及其他格式
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6673
thumbnail: KT-6673.jpg
exl-id: 1bd01bb8-ca5e-4a4a-8646-3d97113e2c51
TQID: https://experienceleague.adobe.com/OibugZuWT-ZRo0gUnqtvvdSkBqJqTikIs0ARCTF91qs
product_v2: id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2: id: b1809bd0-a86b-4991-8083-2e3b517fc3b8
subfeature_v2: id: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 770
ht-degree: 0%

---

# 利用 PDF 服務 API 和 Node.js，幾分鐘內就能從 HTML 或 MS Office 製作 PDF。

![建立 PDF 英雄圖片](assets/createpdffromhtml_hero.jpg)

隨著全新的 Adobe PDF 服務 API 讓開發者自由選擇多種強大的 PDF 處理服務，以滿足複雜商業工作流程的需求，將文件工作流程數位化變得前所未有的簡單。 複雜的架構、實施策略與技術升級，都能透過這些現成的雲端網路服務來簡化。

在 PDF 服務 API 中，有多種可用服務可用於建立與操作 PDF，或將 PDF 匯出至 MS Office 及其他格式。

* 從靜態或動態 HTML、MS Word、PowerPoint、Excel 等工具製作 PDF 檔案
* 將 PDF 匯出至 MS Word、PowerPoint、Excel 等
* OCR 可辨識 PDF 檔案中的文字並啟用文件搜尋
* 開啟文件時請用密碼保護 PDF。
* 將 PDF 頁面或 PDF 文件合併成單一 PDF
* 壓縮 PDF 以縮小大小，方便透過電子郵件或線上分享
* 線性化以優化 PDF 以快速瀏覽網頁
* 組織包含插入、替換、重新排序、刪除及旋轉服務的 PDF 頁面

開發者只需幾分鐘即可開始使用，提供可執行的範例檔案，方便存取所有可用的網路服務。 以下是開始的方法。

## 取得憑證與下載範例檔案

第一步是取得憑證（API 金鑰）以解鎖使用權限。 [請在此](https://www.adobe.com/go/dcsdks_credentials) 註冊免費試用，並點擊「開始使用」以建立您的新憑證。

![API 金鑰](assets/apikey.png)

選擇「個人帳號」以註冊免費試用非常重要：

![個人帳戶](assets/personalaccount.png)

下一步你會選擇 PDF 服務 API 服務，然後為你的憑證加上名稱和描述。

有一個「建立個人化程式碼範例」的勾選框。 選擇此選項即可讓您的新憑證自動加入範例檔案，跳過手動步驟。

接著，選擇Node.js語言以接收Node.js特定範例，並點擊「建立憑證」按鈕。

![建立憑證](assets/createcredentials.png)

你會收到一個名為 PDFToolsSDK-Node.jsSamples.zip 的.zip檔案，可以儲存到你的本地檔案系統。

## 將你的憑證加入程式碼範例

如果您選擇「建立個人化程式碼範例」選項，則不必手動將客戶 ID 加入程式碼範例檔案，且可跳過下一步，直接前往下方執行程式碼範例區。

如果您未選擇「建立個人化程式碼範例」，則必須從 Adobe.io 控制台複製客戶端 ID（API 金鑰）：

![程式碼範例](assets/codesample.png)

把PDFToolsSDK-Node.jsSamples.zip裡的東西拉開。

請前往 adobe-dc-pdf-tools-sdk-node-samples 資料夾的根目錄。

用任何文字編輯器或 IDE 開啟pdftools-api-credentials.json。

將憑證貼上到程式碼中的客戶 ID 欄位：

```javascript
{
 "client_credentials": {
  "client_id": "abcdefghijklmnopqrstuvwxyz",
```

儲存檔案後，繼續執行程式碼範例。

## 執行你的第一個程式碼範例

使用命令提示字元，前往 adobe-dc-pdf-tools-sdk-node-samples 資料夾下的根目錄。

輸入 npm 安裝：

C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>npm install

現在你準備好執行樣本檔案了！

你的第一個範例，請建立一份PDF：

在仍處於命令提示字元時，請用以下指令執行建立 PDF 範例：

C：\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>node src/createpdf/create-pdf-from-docx.js

範例輸出：

![範例輸出](assets/exampleoutput.png)

你的 PDF 會建立在輸出中指定的位置，預設是 pdfServicesSdkResult 目錄。

## 資源與後續步驟

* 如需更多協助與支援，請造訪 Adobe [[!DNL Acrobat Services] API](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all) 社群論壇

PDF 服務 API [文件](https://www.adobe.com/go/pdftoolsapi_doc)

* [](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) PDF 服務 API 常見問題
* [如有關於授權與價格的問題，歡迎聯絡我們](https://www.adobe.com/go/pdftoolsapi_requestform)
* 相關文章：

   * [新的 PDF 服務 API 為文件工作流程提供更多功能](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)
   * [七月發布 [!DNL Adobe Acrobat Services]：PDF 嵌入與 PDF 服務](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
