---
title: 管理銷售提案和合約
description: 瞭解如何建立高效的工作流程，以自動化和簡化銷售提案
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8099.jpg
jira: KT-8099
exl-id: 219c70de-fec1-4946-b10e-8ab5812562ef
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1395'
ht-degree: 2%

---

# 管理銷售提案和合約

![使用案例主打橫幅](assets/UseCaseSalesHero.jpg)

銷售提案是企業贏取客戶旅程的第一步。 就像一切一樣，第一印象是最後。 因此，您與客戶的第一次互動會設定他們對您的業務的期望。 您的提案必須簡潔、準確且方便。

合約和提案在其檔結構中包含不同類型的資料。 它們包含動態資料 （用戶端名稱、報價量等等） 和靜態資料 （如公司功能、團隊基本資料和標準 SOW 條款）。 建立範本檔 （例如銷售提案） 通常涉及單調的工作，例如手動取代樣板範本中的專案細節。 在此教學課程中，您可以使用動態資料和工作流程，建立有效的銷售提案 ](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html) 建立程式 [ 。

## 您可以學習哪些內容

在此實作教學課程中，瞭解如何使用幾個工具來實作動態資料和工作流程，其中最重要的一個工具是 [!DNL Adobe Acrobat Services] API。 這些 API 可用來讓您和您的業務更方便銷售提案和合約。 本教學課程示範了實作技巧，以顯示如何自動建立、合併和顯示 PDF 檔。 手動執行這些工作既耗時又繁瑣。 [!DNL Acrobat Services]利用 API，您可以縮短在這些工作上花費的時間。

## 相關 API 和資源

* [Microsoft Word](https://www.office.com/)

* [Node.js](https://nodejs.org/en/)

* [Npm](https://www.npmjs.com/get-npm)

* [[!DNL Acrobat Services] API](https://www.adobe.io/apis/documentcloud/dcsdk/)

* [Adobe檔產生API](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Adobe檔產生記錄器](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

## 解決問題

現在您已安裝工具，就可以開始解決問題。 這些提案具有每位客戶特有的靜態內容和動態內容。 瓶頸會發生，因為每次提交建議時，兩種資料類型都是必要的。 輸入靜態文字非常耗時，因此您將自動執行該文字，並且只能手動處理來自每個用戶端的動態資料。

首先，在 Microsoft Forms ](https://www.office.com/launch/forms?auth=1) （或您偏好的表格建立器） 中 [ 建立資料擷取表單。此表單適用于新增至銷售方案的用戶端動態資料。 填寫此表單時會提出問題，向客戶取得所需的詳細資料，例如：公司名稱、日期、位址、專案範圍、定價和其他注釋。 若要建立自己的表單，請使用此 [ 表單 ] （HTTPs://forms.office.com/Pages/ShareFormPage.aspx id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAAAN__rtiGj5UNElTR0pCQ09ZNkJRUlowSjVQWDNYUEg2RC4u&amp;sharetoken=1AJeMavBAzzxuISRKmUy）。 目標是讓潛在客戶端填寫表單，然後將其回應匯出為 JSON 檔案，這些檔案會傳遞至工作流程的下一部分。

有些表單指定只能讓您將資料匯出為CSV檔案。 因此，您可能會發現將產生的CSV檔案轉換 ](http://csvjson.com/csv2json) 為 JSON 檔案很有用 [ 。

靜態資料會在每一個銷售提案中重複使用。 因此，您可以使用 Microsoft Word 中的銷售提案範本來提供靜態文字。 您可以使用此 [ 範本，但您可以建立自己的範本 ](https://1drv.ms/w/s!AiqaN2pp7giKkmhVu2_2pId9MiPa?e=oeqoQ2) 或使用 [ Adobe範本 ](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html) 。

現在，您需要一些能同時使用 JSON 格式的用戶端動態資料和 Microsoft Word 範本中的靜態文字來為客戶制定獨特的銷售建議。 API [!DNL Acrobat Services] 可用來合併兩者並產生可簽署的 PDF。

若要製作此作品，請使用標籤。 標籤是簡單易用的字串，可代表數位、字詞、陣列，甚至是複雜的物件。 標籤是動態資料的預留位置，在這種情況下，就是在表單中輸入的用戶端資料。 將標籤插入範本後，您可以將表單欄位從 JSON 檔案對應到 Word 範本。

## 使用標記

開啟您的銷售提案範本，然後選取「 **插入」索** 引標籤。 在「 **增益集」** 群組中，選取「 **取得增益集」** 。 然後，選 **Adobe檔產生增益集** 」加以新增。 新增後，您會在「首頁 **」索引標籤的** 「Adobe」群組中 **看到「** 檔世代記錄」。

在「 **Adobe群組的「首頁** 」索 **引標籤上** ，選取 **「檔產生** 」以開始標記檔。實用的示範影片會出現在視窗右側的面板中。

![Word 內 Document Tagger 增益集的螢幕擷圖](assets/sales_1.png)

選 **取開始使用** 。 接著，系統會要求您提供範例資料。 如以下所示，貼上或上傳表格回應 JSON 檔案。

![貼上範例程式碼的螢幕擷圖](assets/sales_2.png)

選 **取「產生標籤** 」，從您貼上或上傳的 JSON 檔案取得欄位清單。 標籤會顯示在右側邊欄的下方。

![可用標籤的螢幕擷圖](assets/sales_3.png)

產生標籤後，您可以將其插入檔中。 標籤會新增至檔游標位置。 如上所示，您應該將「專案範圍 **」標籤直接新增** 到 **「專案範圍** 」字幕下方。如此一來，當客戶在表單中輸入專案範圍時，其回應會顯示在「專案範圍 **」字幕下** 方，取代您剛新增的標籤。新增標籤後，檔的一部分看起來應該像下面的螢幕擷取。

![將標籤新增至 Word 檔的螢幕擷圖](assets/sales_4.png)

## 使用 API

前往 [!DNL Acrobat Services] API [ 首頁 ](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html) 。 若要開始使用 [!DNL Acrobat Services] API，您需要應用程式的認證。 往下捲動，然後選取 **「開始免費試** 用」以建立認證。 您可以免費使用這些服務 [ 6 個月，每次 ](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 檔交易只需支付 $0.05，因此只需支付所需的費用。

選取 **「PDF 服務」API** 做您選擇的服務，並填入如下所示的其他詳細資料。

![取得認證的螢幕擷圖](assets/sales_5.png)

建立認證後，您會獲得一些程式碼範例。 選取您偏好的語言 （此教學課程使用 Node.js）。 您的API認證位於 zip 檔案中。 將檔案解壓縮為 PDFToolsSDK-Node.jsSamples。

首先，請建立一個名為 auto-doc\**的空白資料夾。\*\* 在檔案夾中，執行下列命令以初始化 Node.js 專案： `npm init` 。 為您的專案命名「自動檔」 *。*

在檔案夾中 。/PDFToolsDK-Node.jsSamples/adobe-dc-pdf-tools-sdk-node-samples，有一個名為 pdftools-api-credentials.json 的檔案。 將它和 private.key 移至自動檔資料夾。 其中包含您的API認證。 此外，在自動資料夾中，建立名為「resources」的子檔案夾。 它包含每次產生銷售方案時從用戶端收到的 JSON 格式資料。 在同一個檔案夾中，儲存 Microsoft Word 中的銷售提案範本。

現在您可以施展魔法了！ 由於您在本教學課程中使用 Node.js，因此必須安裝 Node.js [!DNL Acrobat Services] SDK。 若要這麼做，請在「自動檔」資料夾中，執行「@adobe/documentservices-pdftools-node-sdk」

現在建立一個名為 merge.js 的檔案，然後將下列程式碼貼到檔案中。

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

此程式碼會在您使用 [!DNL Acrobat Services] 的標記建立的協助下，從 Microsoft 表格取得 JSON 檔案。 然後，將資料與您在 Microsoft Word 中建立的銷售提案範本合併，產生全新的 PDF。 PDF 會儲存在新建立的檔案中。/輸出檔案夾。

此外，程式碼會使用 [ Adobe Sign API ](https://www.adobe.io/apis/documentcloud/sign.html) 讓兩家公司簽署產生的銷售方案。 請參閱此部落格文章，深入瞭解此API。

## 後續步驟

您一開始是一個效率低下、冗長乏味的過程，需要自動化。 您從手動為每個客戶建立檔，到建立簡化的工作流程來自動化和簡化 [ 銷售提案流程 ](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html) 。

使用 Microsoft Forms，您從客戶得到的重要資料，這些資料會包含在其唯一提案中。 您在 Microsoft Word 中建立了銷售提案範本，以提供每次都不想重新建立的靜態文字。 然後，您使用 [!DNL Acrobat Services] API 合併來自表單和範本的資料，以更有效率的方式為客戶建立銷售提案 PDF。

此實作教學課程僅能一窺這些 API 是否可行。 若要探索更多解決方案，請造 [[!DNL Adobe Acrobat Services] ](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 訪 API 頁面。所有工具均可免費使用 6 個月。 然後，根據「現成付費」計畫，每份檔只需 [ ](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) 支付 $0.05 美元，因此您只需隨著團隊增加更多潛在客戶而付費至銷售管線。
