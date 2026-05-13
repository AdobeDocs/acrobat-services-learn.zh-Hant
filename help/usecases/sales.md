---
title: 銷售提案與合約管理
description: 學習如何建立高效的工作流程，以自動化並簡化銷售提案
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8099
thumbnail: KT-8099.jpg
exl-id: 219c70de-fec1-4946-b10e-8ab5812562ef
TQID: https://experienceleague.adobe.com/Jj-xhGUcWVWOMooS2fOPcYmELcH70cG1eRRaPPy66Yk
product_v2: id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2: id: b1809bd0-a86b-4991-8083-2e3b517fc3b8id: c4d07275-6387-4756-8bf7-681e581ffd27
subfeature_v2: id: b4b3dc0f-b1be-46b4-b8ca-134a4629084aid: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 1442
ht-degree: 0%

---

# 管理銷售提案與合約

![使用案例英雄卡池](assets/UseCaseSalesHero.jpg)

銷售提案是企業邁向客戶獲取旅程的第一步。 就像所有事情一樣，第一印象會持久。 因此，你與顧客的第一次互動，會設定他們對你事業的期望。 您的提案必須簡潔、準確且方便。

合約與提案的文件結構中包含不同類型的資料。 它們包含動態資料（客戶名稱、報價金額等）與靜態資料（如公司能力、團隊簡介及標準工作工作說明詞等標準文字）。 製作範本文件，例如銷售提案，通常涉及單調的工作，例如手動替換模板中的專案細節。 在這個教學中，你會運用動態資料和工作流程，建立一個高效的銷售提案](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/sales-proposals-and-contracts)製作流程[。

## 你可以學到什麼

在這個實作教學中，學習如何使用多種工具實作動態資料與工作流程，其中最重要的是 [!DNL Adobe Acrobat Services] API。 這些 API 用來讓您和您的企業更方便地進行銷售提案與合約。 本教學示範如何自動建立、合併及顯示 PDF 文件的實作技巧。 手動執行這些任務既耗時又繁瑣。 善用 [!DNL Acrobat Services] API，你可以縮短這些任務所花費的時間。

## 相關 API 與資源

* [Microsoft Word](https://www.office.com/)

* [Node.js](https://nodejs.org/en/)

* [NPM](https://www.npmjs.com/get-npm)

* [[!DNL Acrobat Services] API](https://developer.adobe.com/document-services/homepage/)

* [Adobe 文件產生 API](https://developer.adobe.com/document-services/apis/doc-generation)

* [Adobe Sign API](https://developer.adobe.com/adobesign-api/)

* [Adobe 文件產生標註器](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

## 解決問題

現在你已經安裝好工具，就可以開始解決問題了。 提案包含靜態內容與每位客戶獨有的動態內容。 瓶頸之所以會出現，是因為每次提出提案時都需要這兩種資料。 輸入靜態文字很耗時，所以你會自動化，只手動處理每個客戶端的動態資料。

首先，在 [Microsoft Forms](https://www.office.com/launch/forms?auth=1) （或你偏好的表單建包器）中建立資料擷取表單。 此表單用於客戶動態資料，並加入銷售提案中。 填寫此表單以獲取客戶所需資訊——例如公司名稱、日期、地址、專案範圍、價格及額外意見。 要建立自己的表格，請使用此 [表單]（https://forms.office.com/Pages/ShareFormPage.aspx id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAAAN__rtiGj5UNElTR0pCQ09ZNkJRUlowSjVQWDNYUEg2RC4u&amp;sharetoken=1AJeMavBAzzxuISRKmUy）。 目標是讓潛在客戶填寫表單，然後將回應匯出成 JSON 檔案，再傳遞到你工作流程的下一階段。

有些表單建構器只能讓你匯出 CSV 檔案。 所以，你可能會覺得將產生的 CSV 檔案轉換](http://csvjson.com/csv2json)成 JSON 檔案很有用[。

靜態資料會在每個銷售提案中重複使用。 所以，你可以在 Microsoft Word 中使用銷售提案範本來提供靜態文字。 你可以使用這個 [範本](https://1drv.ms/w/s!AiqaN2pp7giKkmhVu2_2pId9MiPa?e=oeqoQ2)，但你也可以自己建立或使用 [Adobe 範本](https://developer.adobe.com/document-services/apis/doc-generation)。

現在，你需要一個能同時取得客戶動態資料（JSON）和 Microsoft Word 範本中靜態文字的系統，為客戶提出獨特的銷售提案。 API [!DNL Acrobat Services] 用來合併兩者並產生可簽署的 PDF。

要讓這一切運作，你就使用標籤。 標籤是易於使用的字串，可以代表數字、字詞、陣列，甚至複雜的物件。 標籤作為動態資料的佔位符，此處的動態資料是輸入於表單中的客戶端資料。 一旦你在範本中插入標籤，就可以將表單欄位從 JSON 檔案映射到 Word 範本。

## 使用標籤

打開你的銷售提案範本，選擇 **「插入** 」標籤。 在 **「附加** 元件」群組中，選擇 **「取得附加元件**」。 接著，選擇 **Adobe 文件產生外掛** 來新增。 新增後，你會在 Adobe **群組的首頁**&#x200B;標籤&#x200B;**看到文件產生標籤器**。

在 **Adobe** 群組的 **Home** 標籤中，選擇&#x200B;**文件產生**&#x200B;開始標記文件。右側窗戶的面板上有一段實用的示範影片。

![Word 內文件標註外掛的截圖](assets/sales_1.png)

選擇 **「開始使用**」。 接著，你必須提供範例資料。 請依下方所示貼上或上傳表單回應 JSON 檔案。

![貼上範例程式碼的截圖](assets/sales_2.png)

選擇 **產生標籤** ，即可從你貼上或上傳的 JSON 檔案中取得欄位清單。 標籤如下，右側邊欄。

![可用標籤截圖](assets/sales_3.png)

產生標籤後，你可以將它們插入文件中。 標籤會在游標所在的位置加入文件。 如上所示，你應該在專案範圍&#x200B;**副標題下方**&#x200B;加上&#x200B;**專案範圍**&#x200B;標籤。這樣當客戶在表單中輸入專案範圍時，他們的回應會顯示在專案範圍&#x200B;**副標題下方**，取代你剛新增的標籤。在你完成新增標籤後，文件的一部分應該會像下面截圖一樣。

![為 Word 文件新增標籤的截圖](assets/sales_4.png)

## 使用 API

前往 [!DNL Acrobat Services] API 首 [頁](https://developer.adobe.com/document-services/apis/doc-generation)。 要開始使用 [!DNL Acrobat Services] API，你需要應用程式的憑證。 往下滑到最下方，選擇 **「開始免費試用** 」來建立憑證。 你可以免費使用這些服務 [六個月，然後轉用](https://developer.adobe.com/document-services/pricing/main) 預付費，每筆文件交易只需 0.05 美元，這樣你只需支付所需的費用。

選擇 **PDF Services API** 作為您選擇的服務，並依下列方式填寫其他細節。

![取得憑證的截圖](assets/sales_5.png)

建立憑證後，你會得到一些程式碼範例。 選擇你偏好的語言（本教學使用Node.js）。 你的 API 憑證是壓縮檔。 將檔案解壓為 PDFToolsSDK-Node.jsSamples。

開始時，建立一個名為 auto-doc\*\*.\*\* 的空資料夾。在資料夾中，執行以下指令初始化一個Node.js專案： `npm init`。 你的專案名稱為「auto-doc」。**

在 ./PDFToolsSDK-Node.jsSamples/adobe-dc-pdf-tools-sdk-node-samples 這個資料夾中，有一個名為 pdftools-api-credentials.json 的檔案。 移動它並private.key到自動文件資料夾。 它包含你的 API 憑證。 另外，在 auto-doc 資料夾裡，建立一個叫做「resources」的子資料夾。 它保存了你產生銷售提案時從客戶那裡收到的 JSON 格式資料。 在同一資料夾中，儲存 Microsoft Word 的銷售提案範本。

現在你準備好創造奇蹟了！ 因為你在這個教學中使用的是Node.js，你必須安裝 Node.js [!DNL Acrobat Services] SDK。 要做到這點，可以在 auto-doc 資料夾裡執行 yarn add @adobe/documentservices-pdftools-node-sdk。

現在建立一個叫做 merge.js 的檔案，然後把以下程式碼貼上去。

```
javascript
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk'),
fs = require('fs');
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Setup input data for the document merge process
const jsonString = fs.readFileSync('resources/Proposal.json'),
jsonDataForMerge = JSON.parse(jsonString);
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile('resources/Proposal.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/Proposal.pdf'))
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation', err);
} else {
console.log('Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
```

這段程式碼是利用你用 S 建立的 [!DNL Acrobat Services]標籤，從 Microsoft 表單取得你的 JSON 檔案。 接著，它會將資料與你在 Microsoft Word 中建立的銷售提案範本合併，產生全新的 PDF。 PDF 會儲存在新建立的 ./output 資料夾中。

此外，程式碼使用 [Adobe Sign API](https://developer.adobe.com/adobesign-api/) ，讓兩家公司簽署產生的銷售提案。 請參考這篇部落格文章，裡面有詳細說明這個 API。

## 後續步驟

你一開始就是一個效率低、繁瑣、需要自動化的流程。 你從手動為每個客戶建立文件，轉變為建立簡化流程，自動化並簡化 [銷售提案流程](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/sales-proposals-and-contracts)。

使用 Microsoft Forms，你可以從客戶那裡獲得關鍵資料，這些資料會被納入他們獨特的提案中。 你在 Microsoft Word 裡建立了銷售提案範本，提供你不想每次都重複的靜態文字。 接著你使用 [!DNL Acrobat Services] API 將表單與範本的資料合併，更有效率地為客戶製作銷售提案 PDF。

這個實作教學只是這些 API 能做到的一瞥。 想了解更多解決方案，請造訪 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) API 頁面。 使用這些工具六個月免費。 接著，在隨用隨付](https://developer.adobe.com/document-services/pricing/main)計畫中，每筆文件交易[只需支付0.05美元，這樣你只需隨著團隊為銷售管道增加更多潛在客戶而付費。
