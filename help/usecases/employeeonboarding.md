---
title: 現代化員工入職流程
description: 瞭解如何使用 API 現代化員工到職  [!DNL Adobe Acrobat Services]  流程
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-10203.jpg
jira: KT-10203
exl-id: 0186b3ee-4915-4edd-8c05-1cbf65648239
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1514'
ht-degree: 1%

---

# 將員工到職流程現代化

![使用案例主打橫幅](assets/usecaseemployeeonboardinghero.jpg)

在大型組織中，員工到職程式龐大且緩慢。 一般以來，系統會混合自訂檔與樣板材料，由新員工展示及簽署。 這種自訂材質與樣板材質的混合需要多個步驟，讓參與流程的人員有寶貴的時間。 [!DNL Adobe Acrobat Services] Acrobat Sign 可以現代化並自動化此方法，讓您的人力資源個人化處理更重要的工作。 一同探討如何實現這一目標。

## 什麼是 [!DNL Adobe Acrobat Services] ？

[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage) 是一組與使用檔 （不只是 PDF） 相關的 API。 廣義而言，此服務套件分為三個主要類別：

* 首先是 [ 「PDF 服務 ](https://developer.adobe.com/document-services/apis/pdf-services/) 」工具集。 這些是處理 PDF 和其他檔的「公用程式」方法。 這些服務包括轉換 PDF、執行 OCR 和優化、合併和分割 PDF 等等。 它是檔處理功能的工具箱。
* [PDF Extract API ](https://developer.adobe.com/document-services/apis/pdf-extract/) 使用強大的 AI/ML 技術來分析 PDF，並傳回關於內容的驚人細節。 這包括文字、樣式和位置資訊，還可以傳回 CSV/XLS 格式的表格資料以及擷取影像。
* 最後， [ 「檔產生」API ](https://developer.adobe.com/document-services/apis/doc-generation/) 允許開發人員將 Microsoft Word 當作「範本」，與其資料 （從任何來源） 混合，並產生動態的個人化檔 （PDF 和 Word）。

開發人員可以 [ 註冊 ](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) 並免費試用所有這些服務。 該 [!DNL Acrobat Services] 平臺使用基於 REST 的API，但也支援節點、JAVA、.NET 和 Python 的 SDK （目前僅支援 Extract）。

開發人員雖然不是API，但也可以使用免費 [ 的 PDF 內嵌API ](https://developer.adobe.com/document-services/apis/pdf-embed/) ，透過網頁檢視檔時能提供一致且靈活的體驗。

##  Acrobat Sign 是什麼？

[Acrobat Sign ](https://www.adobe.com/tw/sign.html) 是電子簽名服務的世界領導者。 您可以使用各種不同的工作流程 （包括多個簽名） 傳送檔以索取簽名。 Acrobat Sign 也支援需要簽名和其他資訊的工作流程。 具備靈活撰寫系統的強大儀表板支援所有這些功能。

與此 [!DNL Acrobat Services] 功能一 [ 樣，Acrobat Sign 提供免費試 ](https://www.adobe.com/sign.html#sign_free_trial) 用版，可讓開發人員透過儀表板和便於使用的 REST 架構API來測試簽署程式。

## 入門案例

讓我們來考慮一個真實情況，展示Adobe服務可以如何提供協助。 當新進員工加入公司時，他們需要根據自己的角色量身定制資訊。 此外，他們還需要公司範圍的材料。 最後，他們必須簽署檔，證明他們接受企業政策。 讓我們將此細分為具體步驟：

* 首先，需要一封以名字迎接新員工的自訂求職信。 信中應包含有關員工姓名、職位、薪資和地點的相關資訊。
* 自訂字母必須與包含整個公司基本資訊的 PDF 合併 （考慮各種人力資源政策、優點等等）
* 必須包含最終檔，以索取員工的簽名和日期。
* 上述所有內容應以一份檔形式呈現給員工以供簽署。

一同深入瞭解如何做到這一點。

## 產生動態檔

Adobe的檔 [ 產生 ](https://developer.adobe.com/document-services/apis/doc-generation/) API可讓開發人員使用 Microsoft Word 和簡單的範本語言來建立動態檔，作為產生 PDF 和 Word 檔的基礎。 以下是運作方式的範例。

讓我們先從具有硬式編碼值的 Word 檔開始。 檔可以隨您想要的方式設定樣式，包括圖形、表格等等。 以下是初始檔。

![初始檔的螢幕擷圖](assets/onboarding_1.png)

檔產生的工作原理是將「權杖」新增至以資料取代的 Word 檔中。 雖然可以手動輸入這些字元，但有 [ Microsoft Word 增益集 ](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin/) 可讓您更輕鬆地進行此操作。 開啟檔案可讓作者定義可在檔中使用的標籤或資料集。

![檔記錄器螢幕擷圖](assets/onboarding_2.png)

您可以從本機檔案上傳 JSON 資訊、複製 JSON 文字，或是選取以繼續使用初始資料。 這麼做可讓您根據特定需求臨時定義標籤。 在這個範例中，只需要名字、職位、薪資和地點的標籤。 使用「建立標籤 **」** 按鈕即可完成此作業：

![定義標籤的螢幕擷圖](assets/onboarding_3.png)

定義第一個標籤後，您可以根據需要繼續定義：

![已定義標籤的螢幕擷圖](assets/onboarding_4.png)

在定義標籤後，選取檔中的文字，並視需要以標籤取代。 此範例中新增了姓名、職位和薪資等標籤。

![標籤螢幕擷圖](assets/onboarding_5.png)

檔產生不僅支援簡單的標籤，也支援邏輯運算式。 檔的第二段文字僅適用于「代表小號」中的人物。 您可以透過進入 Document Tagger 的「進階」索引標籤並定義條件來新增條件式運算式。 以下是您定義簡單等於條件的方式，但請注意，也支援數位比較和其他比較類型。

![條件的螢幕擷圖](assets/onboarding_6.png)

接著，即可在段落中插入及繞排此動作：

![檔中條件的螢幕擷圖](assets/onboarding_7.png)

若要測試其運作方式，請選取「 **產生檔」** 。 第一次這麼做時，您必須使用Adobe ID登入。 登入後，系統會顯示可手動編輯的預設 JSON。

![資料螢幕擷圖](assets/onboarding_8.png)

隨即產生可檢視或下載的 PDF。

![產生的 PDF 螢幕擷圖](assets/onboarding_9.png)

雖然 Document Tagger 可讓您快速設計和測試，但一旦完成並開始生產，您就可以使用其中一個 SDK 來自動化此程式。 實際程式碼因特定需求而有所不同，但 Node.js 中此程式碼的外觀範例如下：

```js
 const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');

const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Data would be dynamic...
let data = {
    "name":"Raymond Camden",
    "role":"Lead Developer",
    "salary":9000,
    "location":"Louisiana"
}

// Create an ExecutionContext using credentials.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance.
const documentMerge = PDFServicesSdk.DocumentMerge,
    documentMergeOptions = documentMerge.options,
    options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance.
const documentMergeOperation = documentMerge.Operation.createNew(options);

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile('documentMergeTemplate.docx');
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
    .then(result => result.saveAsFile('documentOutput.pdf'))
    .catch(err => {
        if(err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

簡言之，程式碼會設定認證、建立操作物件並設定輸入和選項，然後呼叫操作。 最後，它會將結果儲存為 PDF。 （結果也可以輸出為 Word。）

Document Generation 支援更複雜的使用案例，包括具有完全動態表格和影像的功能。 如需詳細資訊， [ 請參閱檔 ](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) 。

## 執行 PDF 作業

PDF [ Services API ](https://developer.adobe.com/document-services/apis/pdf-services/) 提供大量處理 PDF 的「公用程式」操作。 這些作業包括：

* 從 Office 檔建立 PDF
* 將 PDF 轉存為 Office 檔
* 合併和分割 PDF
* 將 OCR 套用至 PDF
* 設定、移除及修改 PDF 保護
* 刪除、插入、重新排序和旋轉頁面
* 透過壓縮或線性化優化 PDF
* 取得 PDF 屬性

若是這種情況，「檔產生」呼叫的結果必須與標準 PDF 合併。 這個操作對於 SDK 相當簡單。 Node.js 的範例如下：

```js
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
 
// Initial setup, create credentials instance.
const credentials = PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();
 
// Create an ExecutionContext using credentials and create a new operation instance.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials),
    combineFilesOperation = PDFServicesSdk.CombineFiles.Operation.createNew();
 
// Set operation input from a source file.
const combineSource1 = PDFServicesSdk.FileRef.createFromLocalFile('documentOutput.pdf'),
      combineSource2 = PDFServicesSdk.FileRef.createFromLocalFile('standardCorporate.pdf');

combineFilesOperation.addInput(combineSource1);
combineFilesOperation.addInput(combineSource2);
 
// Execute the operation and Save the result to the specified location.
combineFilesOperation.execute(executionContext)
    .then(result => result.saveAsFile('combineFilesOutput.pdf'))
    .catch(err => {
        if (err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

此程式碼會處理兩個 PDF、合併它們，並將結果儲存為新的 PDF。 簡單又簡單！ [如需可執行操作的範例，請參閱檔 ](https://developer.adobe.com/document-services/docs/overview/pdf-services-api/) 。

## 簽署程式

在入職程式的最後一站，員工必須簽署合約，告知他們已閱讀並同意其中定義的所有政策。 [Acrobat Sign ](https://www.adobe.com/tw/sign.html) 支援許多不同的工作流程和整合，包括透過 [ API自動執行的工作 ](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html) 流程和整合。 從廣義而言，案例的最終部分可以如下所示完成：

首先，設計包含需要簽署的表單的檔。 有多種執行方法，包括在使用者控制台中Adobe Sign設計的視覺效果。 另一個選項是使用「檔產生 Word」增益集為您插入標籤。 此範例要求籤署和日期。

![使用 Sign 標籤的檔螢幕擷圖](assets/onboarding_10.png)

本檔可以儲存為 PDF，並使用上述相同方法，將所有檔合併在一起。 此程式建立一個連貫一致的套件，其中包含個人化的賀詞、標準的公司檔，以及可供簽署的最終頁面。

範本可以上傳至 Acrobat Sign 儀表板，然後用於新合約。 使用 REST API 即可將這份檔傳送給准的員工，以索取他們的簽名。

![已簽署檔的螢幕擷圖](assets/onboarding_11.png)

## 自己體驗

本文所述的一切專案現在都可以進行測試。 API [!DNL Adobe Acrobat Services] [ 免費試 ](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) 用版目前為您提供六個月內 1，000 次免費請求。 Acrobat Sign 的 [ 免費試 ](https://www.adobe.com/sign.html#sign_free_trial) 用版可讓您傳送加上浮水印的合約做為測試用途。

有問題嗎?支援 [ 論壇 ](https://community.adobe.com/t5/document-services-apis/ct-p/ct-Document-Cloud-SDK) 每天由Adobe開發人員及支援人員監控。 為了獲得更多靈感，請務必觀看下一 [ 集「剪紙片段 ](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) 」。 定期舉行現場會議，與客戶發表新聞、示範和討論。
