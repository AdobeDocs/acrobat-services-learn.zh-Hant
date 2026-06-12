---
title: Node.js 中的協議工作流程
description: '[!DNL Adobe Acrobat Services] API 能輕鬆將 PDF 功能整合到你的網頁應用程式中'
feature: Use Cases
role: Developer
level: Beginner
type: Tutorial
jira: KT-7473
thumbnail: KT-7473.jpg
keywords: 特色
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
TQID: https://experienceleague.adobe.com/8SQivYwIRQxLqcHWCvrZ7b4t2BOqGyWKZvvNSpomrlo
product_v2:
  - id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2:
  - id: b1809bd0-a86b-4991-8083-2e3b517fc3b8
  - id: c4d07275-6387-4756-8bf7-681e581ffd27
subfeature_v2:
  - id: c4b1e8f2-d9a8-4792-b5e4-be52bd870028
  - id: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 2187
ht-degree: 0%

---

# Node.js 中的協議工作流程

![使用案例英雄卡池](assets/UseCaseAgreementHero.jpg)

許多商業應用與流程需要文件，如提案與協議。 PDF 文件確保檔案更安全且更難被修改。 他們也提供數位簽章支援，讓您的客戶能快速且輕鬆地完成文件。 [!DNL Adobe Acrobat Services] API 可以輕鬆將 PDF 功能整合到你的網頁應用程式中。

## 你可以學到什麼

在這個實作教學中，學習如何在Node.js應用程式中加入 PDF 服務，以數位化協議流程。

## 相關 API 與資源

* [PDF 服務 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF 嵌入 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe Sign API](https://developer.adobe.com/adobesign-api/)

* [專案程式碼](https://github.com/adobe/pdftools-node-sdk-samples)

## 準備工作 [!DNL Adobe Acrobat Services]

要開始，請設定使用 [!DNL Adobe Acrobat Services]. 註冊帳號並使用 [Node.js 快速入門](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js) 功能驗證你的憑證是否有效，再將此功能整合到更大的應用程式中。

首先，申請一個 Adobe 開發者帳號。 接著，在 [「開始使用](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) 」頁面，選擇 *「建立新憑證」下的「開始使用* 」選項。 你可以註冊他們的免費試用，提供1,000筆文件交易，可於六個月內使用。

![創建新憑證的形象](assets/AWNjs_1.png)

在以下「建立新憑證」頁面，系統提示您在 PDF 嵌入 API 與 PDF 服務 API 之間做出選擇。

選擇 *PDF 服務 API*。

輸入應用程式名稱，並勾選標示為「 *建立個人化程式碼範例*」的方框。 勾選此框會自動將您的憑證嵌入程式碼範例中。 如果你沒有勾選這個選項，你必須手動將你的憑證加入應用程式。

選擇 *Node.js* 以選擇應用程式類型，然後點選 *建立憑證*。

幾分鐘後，一個.zip檔案開始下載，裡面有包含你的憑證的範例專案。 Node.js [!DNL Acrobat Services] 套件已經包含在範例專案程式碼中。

![選擇 PDF 服務 API 憑證的圖片](assets/AWNjs_2.png)

## 手動設定範例專案

如果你選擇不從建立新憑證頁面下載範例專案，也可以手動設定專案。

從 [GitHub](https://github.com/adobe/pdftools-node-sdk-samples) 下載程式碼（不要嵌入你的帳號）。 如果您使用此版本的程式碼，您必須在使用前將您的憑證加入pdftools-api-credentials.json檔案：

```
{
  "client_credentials": {
    "client_id": "<client_id>",
    "client_secret": "<client_secret>"
  },
  "service_account_credentials": {
    "organization_id": "<organization_id>",
    "account_id": "<technical_account_id>",
    "private_key_file": "<private_key_file_path>"
  }
}
```

針對你自己的應用程式，你需要將私鑰檔案和憑證檔案複製到你的應用程式來源。

你必須安裝 Node.js 套件。[!DNL Acrobat Services]安裝套件時，請使用以下指令：

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## 設定記錄

這裡的範例是用 Express 作為應用程式框架。 他們也使用 log4js 來記錄應用程式。 使用 log4js，你可以輕鬆將日誌直接指向主控台或檔案：

```
const log4js = require('log4js');
const logger = log4js.getLogger();
log4js.configure( {
    appenders: { fileAppender: { type:'file', filename: './logs/applicationlog.txt'}},
    categories: { default: {appenders: ['fileAppender'], level:'info'}}
});
 
logger.level = 'info';
logger.info('Application started')
```

上述程式碼會將已記錄的資料寫入 ./logs/applicationlog.txt 的檔案。 如果你想讓它寫入主控台，可以註解 log4js.conconfigure 的呼叫。

## 將 Word 檔案轉換成 PDF

協議和提案通常在生產力應用程式中撰寫，例如Microsoft Word。 若要接受此格式文件並將文件轉換成 PDF，您可以為您的應用程式新增功能。 讓我們來看看如何在 Express 應用程式中上傳並儲存文件，並儲存到檔案系統。

在應用程式的 HTML 中，新增一個檔案元素和一個啟動上傳的按鈕：

```
<input type="file" name="source" id="source" />
<button onclick="upload()" >Upload</button>
```

在頁面的 JavaScript 中，使用擷取函數非同步上傳檔案：

```
function upload()
{
  let formData = new FormData();
  var selectedFile = document.getElementById('source').files[0];
  formData.append("source", selectedFile);
  fetch('documentUpload', {method:"POST", body:formData});
}
```

選擇一個資料夾來接受你上傳的檔案。 應用程式需要一個通往這個資料夾的路徑。 透過與 \_\ 連接的相對路徑求得絕對路徑_dirname：

```
const uploadFolder = path.join(__dirname, "../uploads");
```

由於檔案是透過郵寄方式提交，您必須回覆伺服器端的發帖訊息：

```
router.post('/', (req, res, next) => {
  console.log('uploading')
  if(!req.files || Object.keys(req.files).length === 0) {
  return res.status(400).send('No files were uploaded');
  }
    
  const uploadPath = path.join(uploadFolder, req.files.source.name);
  var buffer = req.files.source.data;
  var result = {"success":true};
  fs.writeFile(uploadPath, buffer, 'binary', (err)=> {
    if(err) {
      result.success = false;
    }
    res.json(result);
  });       
});
```

此函式執行後，檔案會儲存在應用程式的上傳資料夾中，並可供進一步處理。

接著，將檔案從原生格式轉換成 PDF。 你之前下載的範例程式碼包含一個名為 `create-pdf-from-docx.js` 「將文件轉成 PDF」的腳本。 以下函式 `convertDocumentToPDF`，將上傳的文件轉成 PDF 格式，存放在另一個資料夾中：

```
function convertDocumentToPDF(sourcePath, destinationPath)
{    
  try {   
    const credentials = PDFToolsSDK.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdftools-api-credentials.json")
    .build();
 
    const executionContext = 
      PDFToolsSDK.ExecutionContext.create(credentials),
    createPdfOperation = PDFToolsSDK.CreatePDF.Operation.createNew();
 
    const docxReadableStream = fs.createReadStream(sourcePath);
    const input = PDFToolsSDK.FileRef.createFromStream(
      docxReadableStream, 
      PDFToolsSDK.CreatePDF.SupportedSourceFormat.docx);
    createPdfOperation.setInput(input);
 
    createPdfOperation.execute(executionContext)
    .then(result => result.saveAsFile(destinationPath))
    .catch(err => {        
      logger.erorr('Exception encountered while executing operation');        
    })
  }
  catch(err) {        
    logger.error(err);
  }
}
```

你可能會注意到這個代碼有一個大致的模式：

程式碼會建立一個憑證物件和執行上下文，初始化某個操作，然後用該執行上下文執行該操作。 你可以在整個範例程式碼中看到這個模式。

透過對上傳功能做一些新增功能，讓它能呼叫這個功能，使用者上傳的 Word 文件現在會自動轉成 PDF。

以下程式碼建立已轉換 PDF 的目標路徑並啟動轉換：

```
const documentFolder = path.join(__dirname, "../docs");
var extPosition = req.files.source.name.lastIndexOf('.') - 1;
if(extPosition < 0 ) {
  extPosition = req.files.source.name.length
}
const destinationName = path.join(documentFolder,  
  req.files.source.name.substring(0, extPosition) + '.pdf');
console.log(destinationName);
 
logger.info('converting to ${destinationName}')
  convertDocumentToPDF(uploadPath, destinationName);
```

## 將其他檔案格式轉換為 PDF

文件工具包可將其他格式轉換為 PDF，例如靜態 HTML，這是另一種常見的文件類型。 該工具包接受以 .zip 檔案形式包裝的 HTML 文件，文件中所有資源（CSS 檔案、圖片及其他檔案）都在同一 .zip 檔案中。 HTML 文件本身必須命名為 index.html，並放在 .zip 檔案的根目錄中。

若要轉換包含 HTML 的 .zip 檔案，請使用以下程式碼：

```
//Create an HTML to PDF operation and provide the source file to it
htmlToPDFOperation = PDFToolsSdk.CreatePDF.Operation.createNew();     
const input = PDFToolsSdk.FileRef.createFromLocalFile(sourceZipFile);
htmlToPDFOperation.setInput(input);
 
// custom function for setting options
setCustomOptions(htmlToPDFOperation);
 
// Execute the operation and Save the result to the specified location.
htmlToPDFOperation.execute(executionContext)
  .then(result => result.saveAsFile(destinationPdfFile))
  .catch(err => {
    logger.error('Exception encountered while executing operation');
});
```

此函式 `setCustomOptions` 會指定其他 PDF 設定，例如頁面大小。 這裡，你可以看到函式將頁面大小設定為 11.5 x 11 英吋：

```
const setCustomOptions = (htmlToPDFOperation) => {    
  const pageLayout = new PDFToolsSdk.CreatePDF.options.PageLayout();
  pageLayout.setPageSize(11.5, 8);

  const htmlToPdfOptions = 
    new PDFToolsSdk.CreatePDF.options.html.CreatePDFFromHtmlOptions.Builder()
    .includesHeaderFooter(true)
    .withPageLayout(pageLayout)
    .build();
  htmlToPDFOperation.setOptions(htmlToPdfOptions);
};
```

打開包含一些術語的 HTML 文件，瀏覽器中會出現以下內容：

![電腦術語的影像](assets/AWNjs_3.png)

本文件的原始碼由一個 CSS 檔案和一個 HTML 檔案組成：

![CSS 與 HTML 檔案的圖片](assets/AWNjs_4.png)

處理完 HTML 檔案後，你會得到相同的 PDF 格式文字：

![電腦術語的PDF檔案](assets/AWNjs_5.png)

## 附錄頁面

PDF 檔案的另一個常見操作是將可能包含標準文字的頁面（如術語列表）附加在末尾。 文件工具包可將多個 PDF 文件合併成一份文件。 如果你有一個文件路徑清單（這裡有 `sourceFileList`），你可以將每個檔案的檔案參考加入一個物件，進行合併操作。

當合併操作執行時，會提供一個包含原始內容的單一檔案。 你可以在物件上使用 `saveAsFile` 來持久化檔案到儲存裝置。

```
const executionContext = PDFToolsSDK.ExecutionContext.create(credentials);
var combineOperation = PDFToolsSDK.CombineFiles.Operation.createNew();
 
sourceFileList.forEach(f => {
  var combinedSource = PDFToolsSDK.FileRef.createFromLocalFile(f);
  console.log(f);
  combineOperation.addInput(combinedSource);
});
    
 
combineOperation.execute(executionContext)
  .then(result=>result.saveAsFile(destinationFile))
  .catch(err => {
    logger.error(err.message);
});    
```

## 顯示 PDF 文件

你已經對 PDF 檔案做了多次操作，但最終還是要讓使用者查看這些文件。 你可以使用 Adobe 的 PDF 嵌入 API 將文件嵌入網頁。

在顯示 PDF 的頁面上，加入 `<div />` 一個元素來存放文件並設定 ID。 你很快就會用這個ID。 在網頁中，請附 `<script />` 上對 Adobe JavaScript 函式庫的參考：

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

你需要的最後一段程式碼是，當 Adobe PDF 嵌入 API JavaScript 載入後，會顯示文件的函式。 當你收到腳本透過 adobe_dc_view\_sdk.ready 事件載入的通知時，請建立一個新的 AdobeDC.View 物件。 這個物件需要你的客戶端 ID 以及先前建立的元素的 ID。 在 [Adobe 開發者主控台](https://developer.adobe.com/console/)中找到你的客戶 ID。 當你查看先前產生憑證時所建立應用程式的設定時，會顯示客戶端 ID。

![API 用戶端金鑰的映像檔](assets/AWNjs_6.png)

## 其他 PDF 選項

[Adobe PDF 嵌入 API 示範](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf)讓你預覽其他嵌入 PDF 文件的選項。

![嵌入 PDF 選項的圖片 &#x200B;](assets/AWNjs_7.png)

你可以開啟或關閉各種選項，立刻看到它們的渲染方式。 當你找到喜歡的組合時，點擊 *\ 生成程式碼*&#x200B;按鈕，利用這些選項產生實際的 HTML 程式碼。

![程式碼預覽圖片](assets/AWNjs_8.png)

## 新增數位簽章與安全性

文件準備好後，你可以透過 Adobe Sign 新增數位簽章以供審核。 這個功能和你之前用過的功能有點不同。 對於數位簽章，應用程式必須設定為使用 OAuth 進行使用者認證。

設定應用程式的第一步是註冊 [你的應用程式](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/gstarted/create_app.md) ，讓它使用 Adobe Sign 的 OAuth。 登入後，點擊 *「帳戶*」進入建立應用程式的畫面，然後開啟 *Adobe Sign API* 區塊，再點選 *「API 應用程式* 」以開啟已註冊應用程式清單。

![申請註冊第一步的圖片](assets/AWNjs_9.png)

要建立新的申請項目，請點擊右上角的加號圖示。

![畫面右上角的加號圖示圖片](assets/AWNjs_10.png)

在開啟的視窗中，輸入應用程式名稱和顯示名稱。 選擇 *網域中的客戶* ，然後點選 *儲存*。

![應用程式名稱與顯示名稱輸入處的圖片](assets/AWNjs_11.png)

應用程式建立後，您可以從列表中選擇它，並點選 *「為應用程式*&#x200B;配置 OAuth」。 選擇選項。 在重定向網址中，輸入你申請的網址。 你可以在這裡輸入多個網址。 你測試的應用程式值為：

```
http://localhost:3000/signed-in 
```

使用 OAuth 取得代幣的流程是標準的。 你的應用程式會引導使用者到一個登入的網址。 使用者成功登入後，這些資料會被導回應用程式，並在頁面的查詢參數中提供額外資訊。

登入 URL 必須傳送你的客戶端 ID、重定向 URL 以及所需範圍清單。

網址的模式如下：

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

使用者會被提示登入 Adobe Sign 的 ID。 登入後，他們會被詢問是否要提供應用程式權限。

![確認進入畫面的圖片](assets/AWNjs_12.png)

如果使用者點擊 *重定向網址的「允許存取* 」，一個名為代碼的查詢參數會傳遞授權碼：

https://YourServer.com/?code=**\<authorization_code\>**\&api_access_point=https：//api.adobesign.com&web_access_point=https：//secure.adobesign.com</authorization_code\>

將此程式碼連同你的客戶端 ID 和客戶端秘密一起上傳到 Adobe Sign 伺服器，就能獲得存取服務的權杖。 將參數 `api_access_point` 和 `web_access_point`中的值儲存。 這些數值用於後續請求。

```
var requestURL = ' ${api_access_point}oauth/token?code=${code}'
  +'&client_id=${client_id}'
  +'&client_secret=${client_secret}&'
  +'redirect_uri=${redirect_url}&'
  +'grant_type=authorization_code';
request.post(requestURL, {form: { }
}, (err,response,body)=>{                
    var token_response = JSON.parse(body)
    var access_token = token_response.access_token;
    console.log(access_token);
});
```

當文件需要簽名時，必須先上傳該文件。 你的應用程式可以將文件上傳到 `api_access_point` 申請 OAUTH 代幣時收到的值。 端點為 `/api/rest/v6/transientDocuments`。 請求資料如下：

```
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.adobesign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"
<PDF CONTENT>
```

在你的應用程式中，請用以下程式碼建置請求：

```
var uploadRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/transientDocuments',
  'headers': {
    'Authorization': 'Bearer  ${auth_token}'
  },
  formData: {
    'File': {
      'value': fs.createReadStream(documentPath),
      'options': {
        'filename': fileName,
        'contentType': null
      }
    }
  }
};
 
request(uploadRequest, (error, response) => {
  if (error) throw new Error(error);
  var jsonResponse = JSON.parse(response.body);
  var transientDocumentId = jsonResponse.transientDocumentId;
  logger.info('transientDocumentId:', transientDocumentId)
});
```

請求會回傳一個 `transientID` 值。 文件已經上傳，但還沒寄出。 要傳送文件，請使用 `transientID` to 請求來傳送文件。

首先建立一個包含文件要簽署資訊的 JSON 物件。 以下變數 `transientDocumentId` 包含上述程式碼的 ID，並 `agreementDescription` 包含描述需要簽署的協議文字。 簽署文件的人會依照電子郵件地址和職務列出 `participantSetsInfo` 。

```
var requestBody = {
  "fileInfos":[
    {"transientDocumentId":transientDocumentId}],
    "name":agreementDescription,
    "participantSetsInfo":[
      {"memberInfos":[{"email":"user@domain.com"}],
       "order":1,"role":"SIGNER"}
    ],
    "signatureType":"ESIGN","state":"IN_PROCESS"
};
```

發送此網頁請求會建立簽署請求，並回傳一個帶有協議請求 ID 的 JSON 物件：

```
request(requestBody, function (error, response) {
  if (error) throw new Error(error);
  var JSONResponse = JSON.parse(response.body);
  var requestId = JSONResponse.id;
});
```

如果簽署人忘記簽名且需要另一封通知郵件，請使用先前收到的識別碼再次發送通知。 唯一的差別是你還必須加入雙方的參與者身份證。 你可以透過發送 GET 請求到 `/agreements/{agreementID}/members`。

要請求發送提醒，首先建立一個描述請求的 JSON 物件。 最小物件需要參與者 ID 清單及提醒狀態（「ACTIVE」、「COMPLETE」或「CANCELLED」）。

請求可選擇性地附加額外資訊，例如「備註」的值，顯示給使用者。 或者，等待發送提醒（在 中）的延遲（以 `firstReminderDelay`小時計），以及一個提醒頻率（在「頻率」欄位中），可接受如DAILY_UNTIL_SIGNED、EVERY_THIRD_DAY_UNTIL_SIGNED或WEEKLY_UNTIL_SIGNED等數值。

```
var requestBody = {
  //participantList is an array of participant ID strings
  "recipientParticipantIds":participantList
  ,"status":"ACTIVE",
  "note":"This is a reminder to sign out important agreement."
}
 
var reminderRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/agreements/${agreementID}/reminders',
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
 
};

request(reminderRequest, function (error, response) {
});
```

這就是發送提醒請求所需的全部訊息。

![網頁表單圖片](assets/AWNjs_13.png)

## 建立網頁表單

你也可以使用 Adobe Sign API 來建立網頁表單。 網頁表單允許您將表單嵌入網頁或直接連結到網頁。 一旦建立 Web 表單，也會在 Adobe Sign 主控台的網頁表單中顯示。 你可以建立帶有草稿狀態的網頁表單以進行增量建置，撰寫狀態用於編輯網頁表單欄位，並以 ACTIVE 狀態立即託管表單。

![Adobe Sign 管理畫面中的網頁表單圖片](assets/AWNjs_14.png)

要建立網頁表單，請使用表單 `transientDocumentId`。 決定表單的標題和初始化的狀態。

```
var requestBody = {
  "fileInfos": [
    {
      "transientDocumentId": transientDocumentId
    }
  ],
  "name": webFormTitle,
  "state": status,
  "widgetParticipantSetInfo": {
    "memberInfos": [ { "email": "" } ],
    "role": "SIGNER"
  }
}
```

```
var createWebFormRequest = {
  'method': 'POST',
  'url': `${oauthParameters.signin_domain}/api/rest/v6/widgets`,
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
}
```

```
request(createWebFormRequest, function (error, response) {
  var jsonResp = JSON.parse(response.body);
  var webFormID = jsonResp.id;
});
```

你現在可以嵌入或連結到你的文件。

## 後續步驟

從快速啟動和提供的程式碼可以看出，使用 Node 搭配 [!DNL Adobe Acrobat Services] API 實作 PDF 和數位文件審核流程非常簡單。 Adobe 的 API 能無縫整合到你現有的客戶應用程式中。

要發現呼叫所需的範圍，或了解呼叫的建置方式，你可以從 [Rest API 文件](https://secure.na4.adobesign.com/public/docs/restapi/v6)中建立範例呼叫。 [快速入門](https://github.com/adobe/pdftools-node-sdk-samples)程式也展示了 API 處理的其他功能與檔案格式[!DNL Adobe Acrobat Services]。

你可以在應用程式中加入多種 PDF 功能，讓使用者能快速且輕鬆地查看與簽署文件，還有更多功能。 首先，請立即查看 [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage/) 。
