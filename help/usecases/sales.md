---
title: 管理銷售提案和合約
description: 瞭解如何建立高效的工作流程，以自動化和簡化銷售提案
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8099
thumbnail: KT-8099.jpg
exl-id: 219c70de-fec1-4946-b10e-8ab5812562ef
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 0%

---

# 管理銷售提案和合約

![使用案例主打橫幅](assets/UseCaseSalesHero.jpg)

銷售提案是企業贏取客戶旅程的第一步。 就像一切一樣，第一印象是最後。 因此，您與客戶的第一次互動會設定他們對您的業務的期望。 您的提案必須簡潔、準確且方便。

合約和提案在其文件結構中包含不同類型的數據。 它們包含動態數據 （用戶端名稱、報價量等等） 和靜態數據 （如公司功能、團隊基本數據和標準 SOW 條款）。 建立範本檔 （例如銷售提案） 通常涉及單調的工作，例如手動取代樣板範本中的項目細節。 在此教學課程中，您可以使用動態數據和工作流程，建立有效的銷售提案[&#128279;](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/sales-proposals-and-contracts)建立程式。

## 您可以學習哪些內容

在此實作教學課程中，瞭解如何使用幾個工具來實作動態數據和工作流程，其中最重要的一個工具是 [!DNL Adobe Acrobat Services] API。 這些 API 可用來讓您和您的業務更方便銷售提案和合約。 本教學課程示範了實作技巧，以顯示如何自動建立、合併和顯示 PDF 檔。 手動執行這些工作既耗時又繁瑣。 [!DNL Acrobat Services]利用 API，您可以縮短在這些工作上花費的時間。

## 相關 API 和資源

* [Microsoft Word](https://www.office.com/)

* [Node.js](https://nodejs.org/en/)

* [npm](https://www.npmjs.com/get-npm)

* [[!DNL Acrobat Services] 蜜蜂屬](https://developer.adobe.com/document-services/homepage/)

* [Adobe檔產生API](https://developer.adobe.com/document-services/apis/doc-generation)

* [Adobe Sign API](https://developer.adobe.com/adobesign-api/)

* [Adobe檔產生記錄器](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

## 解決問題

現在您已安裝工具，就可以開始解決問題。 這些提案具有每位客戶特有的靜態內容和動態內容。 瓶頸會發生，因為每次提交建議時，兩種數據類型都是必要的。 輸入靜態文字非常耗時，因此您將自動執行該文字，並且只能手動處理來自每個客戶端的動態數據。

首先，在 Microsoft Forms[&#128279;](https://www.office.com/launch/forms?auth=1) （或您偏好的表格建立器） 中建立數據擷取表單。此表單適用於新增至銷售方案的用戶端動態數據。 填寫此表單時會提出問題，向客戶取得所需的詳細數據，例如：公司名稱、日期、位址、專案範圍、定價和其他註釋。 若要建立自己的表單，請使用此 [窗體] （https://forms.office.com/Pages/ShareFormPage.aspx id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAAAN__rtiGj5UNElTR0pCQ09ZNkJRUlowSjVQWDNYUEg2RC4u&amp;sharetoken=1AJeMavBAzzxuISRKmUy）。 目標是讓潛在用戶端填寫窗體，然後將其回應匯出為 JSON 檔案，這些檔案會傳遞至工作流程的下一部分。

有些表單指定只能讓您將數據匯出為CSV檔案。 因此，您可能會發現將產生的CSV檔案轉換[&#128279;](http://csvjson.com/csv2json)為 JSON 檔案很有用。

靜態數據會在每一個銷售提案中重複使用。 因此，您可以使用 Microsoft Word 中的銷售提案範本來提供靜態文字。 您可以使用此 [範本，但您可以建立自己的範本](https://1drv.ms/w/s!AiqaN2pp7giKkmhVu2_2pId9MiPa?e=oeqoQ2)或使用 [Adobe範本](https://developer.adobe.com/document-services/apis/doc-generation)。

現在，您需要一些能同時使用 JSON 格式的用戶端動態數據和 Microsoft Word 範本中的靜態文字來為客戶提出獨特的銷售建議。 API [!DNL Acrobat Services] 可用來合併兩者併產生可簽署的 PDF。

若要製作此作品，請使用標籤。 卷標是簡單易用的字串，可代表數位、字詞、陣列，甚至是複雜的物件。 卷標是動態數據的佔位元，在這種情況下，就是在窗體中輸入的客戶端數據。 將標籤插入範本後，您可以將表單域從 JSON 檔案對應到 Word 範本。

## 使用標籤

開啟您的銷售提案範本，然後選取「 **插入」索** 引標籤。 在「 **囑載入巨集」** 群組中，選取「 **取得載入巨集」**。 然後，選 **Adobe文件產生載入巨集** 」加以新增。 新增後，您會在「首頁&#x200B;**」索引標籤的**「Adobe」群組中&#x200B;**看到「**&#x200B;檔世代記錄」。

在「**Adobe群組的「首頁**」索&#x200B;**引標籤上**，選取&#x200B;**「檔產生**」以開始標記檔。實用的示範影片會出現在視窗右側的面板中。

![Word 內 Document Tagger 載入巨集的螢幕擷圖](assets/sales_1.png)

選 **取開始使用**。 接著，系統會要求您提供範例數據。 如以下所示，貼上或上傳表格回應 JSON 檔案。

![貼上範例程式代碼的螢幕擷圖](assets/sales_2.png)

選 **取「產生標籤** 」，從您貼上或上傳的 JSON 檔案取得欄位清單。 標籤會顯示在右側邊欄的下方。

![可用標籤的螢幕擷圖](assets/sales_3.png)

產生標籤后，您可以將其插入檔中。 標籤會新增至文件游標位置。 如上所示，您應該將「專案範圍&#x200B;**」標籤直接新增**&#x200B;到&#x200B;**「專案範圍**」字幕下方。如此一來，當客戶在窗體中輸入專案範圍時，其回應會顯示在「專案範圍&#x200B;**」字幕下**&#x200B;方，取代您剛新增的標籤。新增標籤后，檔的一部分看起來應該像下面的螢幕擷取。

![將標籤新增至 Word 檔案的螢幕擷圖](assets/sales_4.png)

## 使用 API

前往 [!DNL Acrobat Services] API [首頁](https://developer.adobe.com/document-services/apis/doc-generation)。 若要開始使用 [!DNL Acrobat Services] API，您需要應用程式的認證。 往下卷動，然後選取 **「開始免費試** 用」以建立認證。 您可以免費使用這些服務 [6 個月，每次](https://developer.adobe.com/document-services/pricing/main) 檔交易只需支付 $0.05，因此只需支付所需的費用。

選取 **「PDF 服務」API** 做您選擇的服務，並填入如下所示的其他詳細數據。

![取得認證的螢幕擷圖](assets/sales_5.png)

建立認證后，您會獲得一些程式碼範例。 選取您偏好的語言 （此教學課程使用Node.js）。 您的API認證位於 zip 檔案中。 將檔案解壓縮為 PDFToolsSDK-Node.jsSamples。

首先，請建立一個名為 auto-doc\**的空白資料夾。\*\* 在檔案夾中，執行下列命令以初始化Node.js專案： `npm init`。 為您的專案命名「自動檔」*。*

在檔案夾中 。/PDFToolsSDK-Node.jsSamples/adobe-dc-pdf-tools-sdk-node-samples，有一個名為 pdftools-api-credentials.json 的檔案。 將其移動並private.key至自動資料夾。 其中包含您的API認證。 此外，在自動資料夾中，建立名為「resources」的子檔案夾。 它包含每次產生銷售方案時從用戶端收到的 JSON 格式數據。 在同一個資料夾中，從 Microsoft Word 儲存銷售提案範本。

現在您可以施展魔法了！ 由於您在此教學課程中使用 Node.js，因此必須安裝 Node.js [!DNL Acrobat Services] SDK。 若要這麼做，請在「自動檔」資料夾中，執行「@adobe/documentservices-pdftools-node-sdk」

現在建立一個名為「merge.js」的檔案，然後將下列程式代碼貼到檔案中。

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

此程序代碼會在您使用 [!DNL Acrobat Services]的標記建立的協助下，從Microsoft窗體取得 JSON 檔案。 然後，將數據與您在 Microsoft Word 中建立的銷售提案範本合併，產生全新的 PDF。 PDF 會儲存在新建立的檔案中。/輸出資料夾。

此外，程式代碼會使用 [Adobe Sign API](https://developer.adobe.com/adobesign-api/) 讓兩家公司簽署產生的銷售方案。 請參閱此部落格文章，深入瞭解此API。

## 後續步驟

您一開始是一個效率低下、冗長乏味的過程，需要自動化。 您從手動為每個客戶建立檔，到建立簡化的工作流程來自動化和簡化 [銷售提案流程](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/sales-proposals-and-contracts)。

使用「Microsoft窗體」時，您的客戶會收到重要數據，這些數據會包含在其獨特的提案中。 您在 Microsoft Word 中建立了銷售提案範本，以提供您每次都不想重新建立的靜態文字。 然後，您使用 [!DNL Acrobat Services] API 合併來自表單和範本的數據，以更有效率的方式為客戶建立銷售提案 PDF。

此實作教學課程僅能一窺這些 API 是否可行。 若要探索更多解決方案，請造 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 訪 API 頁面。 所有工具均可免費使用 6 個月。 然後，根據「現成付費」計劃，每份檔只需[&#128279;](https://developer.adobe.com/document-services/pricing/main)支付 $0.05 美元，因此您只需隨著團隊增加更多潛在客戶而付費至銷售管線。
