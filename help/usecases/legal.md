---
title: 管理法律合同
description: 瞭解如何通過自定義資料輸入自動生成和保護法律文檔
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8097
thumbnail: KT-8097.jpg
exl-id: e0c32082-4f8f-4d8b-ab12-55d95b5974c5
source-git-commit: ba73105ecf0bd27b7445ec4388fc4009eec273b8
workflow-type: tm+mt
source-wordcount: '1890'
ht-degree: 0%

---

# 管理法律合同

![使用案例英雄橫幅](assets/UseCaseLegalHero.jpg)

數字化帶來了挑戰。 目前，大多陣列織都有許多類型的[法律合同](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/legal-contracts)，它們必須建立、編輯、批准，並已由不同方簽署。 這些法律合同通常需要獨特的定製和品牌。 組織還可能需要在簽名後以受保護的格式保存它們，以保護它們的安全。 要完成所有這些任務，他們需要一個強健的文檔生成和管理解決方案。

許多解決方案提供了一些文檔生成功能，但無法自定義資料輸入和條件邏輯，如僅適用於特定方案的子句。 手動更新公司的法律模板是一項艱巨任務，而且隨著這些文檔越來越廣泛，容易出錯。 自動化這些過程的需要是相當大的。

## 你能學到的

在本操作教程中，探討在文檔中生成自定義輸入欄位時[[!DNL Adobe Acrobat Services] API](https://developer.adobe.com/document-services/apis/doc-generation)的功能。 此外，還探討如何輕鬆地將這些生成的文檔轉換為受保護的攜帶型文檔格式(PDF)，以防止資料操作。

本教程涉及在探討將合同轉換為PDF時進行一些寫程式。 要有效跟進，應在您的PC上安裝[MicrosoftWord](https://www.microsoft.com/en-us/download/office.aspx)和[Node.js](https://nodejs.org/)。 還建議您基本瞭解Node.js和[ES6語法](https://www.w3schools.com/js/js_es6.asp)。

## 相關API和資源

* [Adobe文檔生成API](https://developer.adobe.com/document-services/apis/doc-generation)

* [PDF嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe SignAPI](https://developer.adobe.com/adobesign-api/)

* [項目代碼](https://github.com/agavitalis/adobe_legal_contracts.git)

## 建立模板文檔

您可以使用MicrosoftWord應用程式或下載Adobe的[Word示例模板](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade)來建立法律文檔。 但是，如果不使用某些幫助工具(如[Adobe文檔生成Tagger載入項](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin))來為MicrosoftWord自定義輸入和數字簽名並非易事。

文檔生成標籤是MicrosoftWord的一個載入項，它用於使文檔定制在使用標籤時能夠無縫進行。 它允許在使用JSON資料動態填充的文檔模板中建立動態欄位。

![如何在Word中添加Adoe文檔生成標誌的螢幕快照](assets/legal_1.png)

要說明文檔生成標籤的使用，請安裝此載入項，然後建立JSON資料模型，該模型用於標籤簡單的法律合同文檔。

通過按一下&#x200B;**插入**&#x200B;頁籤在Word中安裝文檔生成標誌符，然後在載入項組中按一下&#x200B;**我的載入項**。 在「Office載入項」菜單上，搜索「Adobe文檔生成」，然後按一下&#x200B;**添加**&#x200B;並遵循該過程。 您可以在上面的螢幕捕獲中看到這些步驟。

在為Word載入項安裝文檔生成標籤後，建立一個簡單的JSON資料模型以標籤法律文檔。

若要繼續，請開啟所選的任何編輯器，建立一個名為Agreement.json的檔案，然後將下面的代碼段貼上到您建立的JSON檔案中。

```
{
"Agreement": {
"Date": "1/24/2021",
"Prime Contractor Name": "Ogbonna Vitalis Corp",
"Prime State": "Lagos",
"Address": "Maryland Ave, Lagos State, Ng",
"Sub Contractor Name": "Vivvaa Soln",
"Sub Contractor State": "California",
"Sub Contractor Address": "Molusi Avenue, Dallas Texas, CA",
"Agreement Date": "1/24/2021",
"Length": 5
}
}
```

保存此JSON文檔後，將其導入到「文檔生成標籤」載入項。 通過按一下Word螢幕右上角Adobe組中的&#x200B;**文檔生成**&#x200B;導入文檔，如下面的螢幕捕獲所示。

![Word中Adobe文檔生成標籤器載入項的螢幕快照](assets/legal_2.png)

這將顯示一個視頻來指導您。 按一下&#x200B;**開始**&#x200B;可以觀看或直接進入標籤欄位。 按一下&#x200B;**開始**&#x200B;後，將顯示上載表單。 按一下&#x200B;**上載JSON檔案**&#x200B;並選擇剛建立的JSON檔案。 導入完成後，按一下&#x200B;**生成標籤**&#x200B;以生成標籤。

導入和生成標籤後，可將這些標籤添加到文檔中。 要添加它們，請將游標放在希望標籤顯示的準確位置。 然後從文檔生成API中選擇一個標籤，然後按一下&#x200B;**插入文本**。 下面的螢幕捕獲概述了此過程。

![向文檔添加標籤的螢幕快照](assets/legal_3.png)

除了使用導入的JSON資料模型建立的基本標籤外，您還可以為更多選項使用高級功能，如影像、條件邏輯、計算、重複元素和條件短語。 通過按一下「文檔生成標籤」面板中的&#x200B;**高級**，您可以訪問這些功能。 您可以在下面的螢幕捕獲中看到這一點。

![Adobe文檔生成標誌器的「高級」頁籤的螢幕快照](assets/legal_4.png)

這些高級功能與基本標籤沒有區別。 要包括條件邏輯，請選擇要填充的文檔部分。 然後，配置確定標籤插入的規則。

為了進一步說明，例如在協定中，您只希望有條件地包括一個部分。 在「選擇內容類型」欄位中，選擇「**節」。**&#x200B;在「選擇記錄」欄位中，選擇確定條件節是否顯示的選項。 選擇所需的條件運算子，並在「值」欄位中設定要測試的值。 然後按一下&#x200B;**插入條件。**&#x200B;下面的螢幕捕獲說明了此過程。

![插入條件內容的螢幕快照](assets/legal_5.png)

對於計算，請選擇算術或聚合，然後根據可用的模板標籤包括相關的第一條記錄、運算子和第二條記錄。 然後按一下&#x200B;**插入計算**。

此外，法律合同通常需要當事人簽字。 可以使用「數值計算」部分正下方的「Adobe Sign文本」標籤插入電子簽名。 若要包括電子簽名，必須指定收件人數，從下拉清單中選擇&#x200B;**簽名者**&#x200B;和欄位類型。 完成後，按一下&#x200B;**插入Adobe Sign文本標籤**&#x200B;以完成該過程。

要確保資料完整性，請以受保護的格式保存法律文檔。 使用[!DNL Acrobat Services]個API，您可以快速將文檔轉換為PDF格式。 您可以構建簡單的快速Node.js應用程式，將文檔生成API整合到該應用程式中，並使用此簡單應用程式將標籤文檔從Word轉換為PDF格式。

## 項目設定

首先，為Node.js應用程式設定資料夾結構。 在本示例中，調用此簡單應用程式AdobeLegalContractAPI。 您可以在此處檢索原始碼[](https://github.com/agavitalis/adobe_legal_contracts.git)。

### 目錄結構

建立名為AdobeLegalContractAPI的資料夾，然後在您選擇的編輯器中將其開啟。 使用以下資料夾結構使用```npm init```命令建立基本Node.js應用程式：

```
###Directory Structure
AdobeLegalContractAPI
-----config
----------default.json
-----controllers
----------createPDFController.js
----------previewController.js
-----models
----------document.js
-----routes
----------web.js
-----services
-----------upload.js
-----uploads
-----views
-----index.js
```

上面是應用程式的簡單Node.js應用程式結構。 現在繼續安裝必要的npm包。

### 軟體包安裝

使用npm install命令安裝所需的軟體包，如下面的代碼段所示：

```
npm install express body-parser morgan multer hbs path config mongoose
```

安裝包後，請確保包.json檔案的內容與下面的代碼段類似：

```
###package.json
{
"name": "adobelegalcontractapi",
"version": "1.0.0",
"description": "",
"main": "index.js",
"directories": {
"test": "test"
},
"dependencies": {
"body-parser": "^1.19.0",
"config": "^3.3.6",
"express": "^4.17.1",
"hbs": "^4.1.1",
"mongoose": "^5.12.1",
"morgan": "^1.10.0",
"multer": "^1.4.2",
"path": "^0.12.7"
},
"devDependencies": {},
"scripts": {
"start": "node index.js"
},
"repository": {
"type": "git",
"url": "https://github.com/agavitalis/adobe_legal_contracts.git"
},
"author": "Ogbonna Vitalis",
"license": "ISC",
"bugs": {
"url": "https://github.com/agavitalis/adobe_legal_contracts/issues"
},
"homepage": "https://github.com/agavitalis/adobe_legal_contracts#readme"
}
```

在這些代碼片段中，您安裝了應用程式依賴項，包括視圖的Handlebar模板引擎。

本教程的主要重點是使用[[!DNL Acrobat Services] API](https://developer.adobe.com/document-services/homepage/)將文檔轉換為PDF。 因此，沒有一個逐步的過程來說明如何構建此Node.js應用程式。 但是，您可以在[GitHub](https://github.com/agavitalis/adobe_legal_contracts.git)上檢索完整的正在工作的Node.js應用程式碼。

## 將[!DNL Adobe Acrobat Services]個API整合到Node.js應用程式

[!DNL Adobe Acrobat Services]個API是基於雲的可靠服務，旨在無縫地處理文檔。 它提供三個API:

* Adobe PDF服務API

* Adobe PDF嵌入式API

* Adobe文檔生成API

您需要憑據才能使用[!DNL Acrobat Services]個API(與PDF嵌入API憑據不同)。 如果您沒有有效的憑據，請[註冊](https://www.adobe.com/go/dcsdks_credentials?ref=getStartedWithServicesSDK)並完成以下螢幕捕獲中所示的工作流。 享受[免費6個月試用，然後按時付費](https://developer.adobe.com/document-services/pricing/main)，每個文檔交易僅0.05美元。

![建立新憑據的螢幕快照](assets/legal_6.png)

完成註冊過程後，代碼樣例會自動下載到您的PC，以幫助您啟動。 您可以提取此代碼示例，然後繼續操作。 不要忘記將pdftools-api-credentials.json和private.key檔案從提取的代碼樣本複製到Node.js項目的根目錄。 在可以訪問[!DNL Acrobat Services] API終結點之前，需要憑據。 您還可以下載具有個性化憑據的SDK示例，以便不必更新示例代碼中的密鑰。

現在，使用應用程式根目錄中的終端運行```npm install \--save @adobe/documentservices-pdftools-node-sdk```命令來安裝Adobe PDF服務節點SDK。 成功安裝後，您可以使用[!DNL Acrobat Services]個API來處理應用程式中的文檔。

## 建立PDF文檔

[!DNL Acrobat Services]個API支援從MicrosoftOffice文檔（Word、Excel和PowerPoint）和其他[支援的檔案格式](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf)&#x200B;(如.txt、.rtf、.bmp、.jpeg、gif、.tiff和.png)建立PDF。 使用Acrobat服務API，您可以輕鬆地將法律合同從任何其他檔案格式轉換為PDF。

Adobe文檔生成API允許轉換到Word檔案或PDF。 例如，您可以使用Word模板生成合同，包括用紅線標籤已編輯的文本。 然後，將其轉換為PDF，並使用PDF服務API使用密碼保護文檔，將其發送以進行簽名等。

要從可用支援的檔案格式實現PDF文檔的建立，有一個表單可用於使用[!DNL Acrobat Services]上載要轉換的文檔。

設計的上載表單顯示在下面的螢幕捕獲中，您可以訪問[GitHub](https://github.com/agavitalis/adobe_legal_contracts.git)上的HTML和CSS檔案。

![表單上載的螢幕快照](assets/legal_7.png)

現在，將以下代碼片段添加到控制器/createPDFController.js檔案中。 此代碼將檢索上載的文檔並將其轉換為PDF。 [!DNL Acrobat Services]將原始上載的檔案和轉換後的檔案保存在不同的資料夾中。

```
###controllers/createPDFController.js
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
const Document = require('../models/document');
/*
* GET / route to show the createPDF form.
*/
function createPDF(req, res) {
//catch any response on the url
let response = req.query.response
res.render('index', { response })
}
/*
* POST /createPDF to create a new PDF File.
*/
function createPDFPost(req, res) {
let filePath = req.file.path;
let fileName = req.file.filename;
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials and create a new operation instance.
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials),
createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
// Set operation input from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(filePath);
createPdfOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
createPdfOperation.execute(executionContext)
.then(async(result) => {
let newFileName = `createPDFFromDOCX-${Math.random() * 171}.pdf`
let newFilePath = require('path').resolve('./') + `\\output\\${newFileName}`
await result.saveAsFile(`views/output/${newFileName}`)
//Creates a new document
let newDocument = new Document({
documentName: newFileName,
url: newFilePath
});
//Save it into the DB.
newDocument.save((err, docs) => {
if (err) {
res.send(err);
}
else {
res.redirect('/?response=PDF Successfully created')
}
});
})
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
}
//export all the functions
module.exports = { createPDF, createPDFPost };
```

上面的代碼段需要您以前安裝的文檔模型和[!DNL Acrobat Services]節點SDK。 有兩個功能：

* createPDF顯示上載文檔表單。

* createPDFPost將上載的文檔轉換為PDF。

這些函式將轉換後的PDF文檔保存在視圖/輸出目錄中，您可以在此將它們下載到PC。

您還可以使用自由PDF嵌入API預覽已轉換的PDF檔案。 使用PDF嵌入API，您可以在此處[生成Adobe憑據](https://www.adobe.com/go/dcsdks_credentials)（與[!DNL Acrobat Services]憑據不同），並註冊允許的域以訪問該API。 請遵循該過程並為應用程式生成PDF嵌入API憑據。 您還可以查看演示[此處](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf)，您可以從中輕鬆生成代碼，以快速啟動。

返回到應用程式，在應用程式的視圖資料夾中建立list.hbs和preview.hbs檔案，並將下面的代碼段分別貼上到list.hbs和preview.hbs檔案中。

```
###views/list.hbs
<!DOCTYPE html>
<html lang="en">
<head>
<title>Adobe Legal Contract</title>
<!-- Meta tags -->
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,
initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<!-- //Meta tags -->
<link
href=".min.css" rel="stylesheet" integrity="sha384-eOJMYsd53ii+scO/
bJGFsiCZc+5NDVN2yr8+0RDqr0Ql0h+rP48ckxlpbzKgwra6" crossorigin="anonymous">
<link rel="stylesheet" href="css/style.css" type="text/css"
media="all" /><!-- Style-CSS -->
<link href="css/font-awesome.css" rel="stylesheet" /><!--
font-awesome-icons -->
</head>
<body>
<section>
<div class="form-36-mian section-gap">
<div class="wrapper">
<div class="container">
<div class="row">
{{#each documents}}
<div class="col-md-4 mb-2">
<div class="card" style="width:
18rem;">
<img class="card-img-top"
src="./images/pdf.png"
alt="Card image cap">
<div class="card-body">
<h5
class="card-title">{{documentName}}</h5>
<a
href="/downloadPDF/{{_id}}" class="btn btn-primary"><i class="fa
fa-download" aria-hidden="true"></i> Download</a>
<a
href="/previewPDF/{{_id}}" class="btn btn-info"><i class="fa fa-eye"
aria-hidden="true"></i> Preview</a>
</div>
</div>
</div>
{{/each}}
</div>
</div>
<!-- copyright -->
<div class="copy-right">
<p>(c) 2021 Vitalis</p>
</div>
<!-- //copyright -->
</div>
</div>
</section>
</body>
</html>
###views/preview.hbs
<!DOCTYPE html>
<html lang="en">
<head>
<title>[!DNL Adobe Acrobat Services] PDF Embed API</title>
<meta charset="utf-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<meta id="viewport" name="viewport" content="width=device-width,
initial-scale=1" />
</head>
<body style="margin: 0px">
<input type="hidden" id="pdfDocumentName"
value={{document.documentName}} />
<input type="hidden" id="pdfDocumentUrl" value={{document.url}} />
<div id="adobe-dc-view"></div>
<script
src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
let pdfDocumentName =
document.getElementById("pdfDocumentName").value;
let pdfDocumentUrl =
document.getElementById("pdfDocumentUrl").value;
document.addEventListener("adobe_dc_view_sdk.ready", function
() {
var adobeDCView = new AdobeDC.View({ clientId:
"XXXXXXXXXXXXXXXX", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url:
`http://localhost:5000/output/${pdfDocumentName}` } },
metaData: { fileName: pdfDocumentName }
}, {});
});
</script>
</body>
</html>
```

此外，建立controller/previewController.js檔案並將下面的代碼段貼上到該檔案中。

```
const Document = require('../models/document');
/*
* GET /listFiles route to show PDF file lists.
*/
async function listFiles(req, res) {
let documents = await Document.find({});
res.render('lists', { documents })
}
/*
* GET /previewPDF route to show PDF file in AdobeEmbedAPI.
*/
async function previewPDF(req, res) {
//catch any response on the url
let documentId = req.params.documentId
let document = await Document.findOne({_id:documentId});
res.render('preview', { document })
}
/*
* GET /downloadPDF To Download PDF Documents.
*/
async function downloadPDF(req, res) {
let documentId = req.params.documentId
let document = await Document.findOne({_id:documentId});
res.download(document.url);
}
//export all the functions
module.exports = {listFiles, previewPDF, downloadPDF };
```

在上述控制器檔案中，有三個功能： listFiles、previewPDF和downloadPDF。 listFiles函式列出了迄今為止使用[!DNL Acrobat Services]個API生成的所有PDF檔案。 previewPDF函式允許您使用PDF嵌入API預覽PDF檔案，而downloadPDF函式則允許您將生成的PDF檔案下載到您的電腦。 下面的螢幕捕獲顯示使用PDF嵌入API的PDF預覽示例。

![PDF預覽螢幕截圖](assets/legal_8.png)

## 摘要

在本操作教程中，您使用文檔生成標籤器MicrosoftWord載入項標籤了文檔。 然後，將[!DNL Acrobat Services]個API整合到Node.js應用程式和
將帶標籤的文檔轉換為可下載的PDF格式，儘管您也可以直接將法律合同建立為PDF。 最後，您使用Adobe PDF嵌入API預覽生成的驗證和簽名PDF。

已完成的應用程式使用動態欄位標籤[合法合同模板](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/legal-contracts)、將其轉換為PDF、預覽它們，以及使用[!DNL Acrobat Services]個API對它們進行簽名變得更容易。 您的團隊可以自動將正確的合同發送給每個客戶，而不是花時間建立一個唯一的合同，然後花更多時間發展您的業務。

組織使用[!DNL Adobe Acrobat Services]個API以實現其完整性和易用性。 最棒的是，您可以享受[6個月免費試用，然後按時付費](https://developer.adobe.com/document-services/pricing/main)。 你只付你用的錢。 此外，PDF嵌入API始終是免費的。

準備好通過改進文檔流來提高生產效率嗎？ [立即開始](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)。
