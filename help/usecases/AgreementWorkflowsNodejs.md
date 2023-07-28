---
title: Node.js 中的合約工作流程
description: 「 [!DNL Adobe Acrobat Services] API 可輕鬆將 PDF 功能整合到您的網頁應用程式中」
type: Tutorial
role: Developer
level: Beginner
feature: Use Cases
thumbnail: KT-7473.jpg
jira: KT-7473
keywords: 特色
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '2182'
ht-degree: 1%

---

# Node.js 中的合約工作流程

![使用案例主打橫幅](assets/UseCaseAgreementHero.jpg)

許多企業應用程式和流程都需要提案和合約等檔。 PDF 檔可確保檔案更安全且不可修改。 也提供數位簽章支援，讓您的客戶可以快速輕鬆地完成檔。 [!DNL Adobe Acrobat Services] API 可輕鬆將 PDF 功能整合到您的網頁應用程式中。

## 您可以學習哪些內容

在此實作教學課程中，瞭解如何將 PDF 服務新增至 Node.js 應用程式，將合約程式數位化。

## 相關 API 和資源

* [PDF 服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF 嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html)

* [專案代碼](https://github.com/adobe/pdftools-node-sdk-samples)

## 建立 [!DNL Adobe Acrobat Services]

若要開始，請設定要使用的 [!DNL Adobe Acrobat Services] 認證。 註冊帳戶並使用 [ Node.js Quickstart ](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js) 驗證憑證是否有效，然後再將功能整合至大型應用程式。

首先，取得Adobe開發人員帳戶。 然後，在 [ 「開始使用 ](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) 」頁面上，選取「 *建立新認證」下方的開始使用* 選項。 您可以註冊免費試用版，提供 1，000 筆檔交易，可在 6 個月內使用。

![建立新認證的影像](assets/AWNjs_1.png)

在以下「建立新認證」頁面中，系統會提示您決定「PDF 嵌入API」與「PDF 服務」API。

選取 *「PDF 服務」API* 。

輸入應用程式的名稱，然後勾選「建立個人化程式碼範例」標籤的 *方塊* 。 勾選此方塊，便會自動將您的認證嵌入程式碼範例中。 如果您未勾選此方塊，則必須手動將認證新增至應用程式。

選 *取應用程式類型的 Node.js* ，然後按一下「 *建立認證」* 。

不久後，.zip 檔案會開始下載包含您的認證的範例專案。 Node.js 套件 [!DNL Acrobat Services] 已包含在範例專案代碼中。

![選取 PDF 服務API認證的影像](assets/AWNjs_2.png)

## 手動設定範例專案

如果您選擇不從「建立新認證」頁面下載範例專案，也可以手動設定專案。

從 [ GitHub ](https://github.com/adobe/pdftools-node-sdk-samples) 下載代碼 （不內嵌您的憑證）。 如果您使用此版本的代碼，您必須先將認證新增至 pdftools-api-credentials.json 檔案，才能使用：

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

對於您自己的應用程式，您需要將私密金鑰檔案和認證檔案複製到您的應用程式來源。

您必須安裝 Node.js 套件，只要 [!DNL Acrobat Services] 。 若要安裝套件，請使用下列命令：

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## 設定記錄

此處的範例會使用 Express 來支援應用程式架構。 他們也會使用 log4js 進行應用程式記錄。 您可以使用 log4js，輕鬆地將記錄到主控台或登出檔案：

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

上述程式碼會將記錄的資料寫入檔案。/logs/applicationlog.txt。 如果您想要寫入主控台，可以注釋給 log4js.configure 的呼叫。

## 將 Word 檔案轉換為 PDF

合約和提案通常會寫入生產力應用程式中，例如 Microsoft Word。 若要接受此格式的檔並將檔轉換為 PDF，您可以新增應用程式的功能。 一起看看如何在 Express 應用程式中上傳及儲存檔，並將其儲存至檔案系統。

在應用程式的 HTML 中，新增檔案元素和開始上傳的按鈕：

```
<input type="file" name="source" id="source" />
<button onclick="upload()" >Upload</button>
```

在頁面的JavaScript中，使用擷取功能以非同步方式上傳檔案：

```
function upload()
{
  let formData = new FormData();
  var selectedFile = document.getElementById('source').files[0];
  formData.append("source", selectedFile);
  fetch('documentUpload', {method:"POST", body:formData});
}
```

選擇檔案夾以接受您上傳的檔案。 應用程式需要此檔案夾的路徑。 使用加入 \_\_dirname 的相對路徑來尋找絕對路徑：

```
const uploadFolder = path.join(__dirname, "../uploads");
```

由於檔案是透過貼文提交，您必須在伺服器端回復貼文訊息：

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

執行此函式後，檔案會儲存在應用程式上傳檔案夾中，以供進一步處理。

接下來，將檔案從原生格式轉換為 PDF。 您先前下載的範例程式碼包含用來將檔轉換為 PDF 的腳本 `create-pdf-from-docx.js` 。 以下功能： `convertDocumentToPDF` 處理已上傳的檔，並將其轉換為其他資料夾中的 PDF：

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

您可能會發現有程式碼的一般圖樣：

程式碼會建立認證物件和執行內容、初始化某些操作，然後根據執行內容執行操作。 您可以在整個範例程式碼中看到此模式。

在上傳功能中加入一些內容，將其稱為此功能，使用者上傳的 Word 檔現在會自動轉換為 PDF。

下列程式碼會建立轉換後 PDF 的目的地路徑，並起始轉換：

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

## 將其他檔案類型轉換為 PDF

檔工具組會將其他格式轉換為 PDF，例如靜態 HTML （另一種常見的檔案類型）。 工具組接受封裝為 .zip 檔的 HTML 檔案，並在同一個 .zip 檔案中包含檔參照的所有資源 （CSS 檔案、影像和其他檔案）。 HTML 檔案本身必須命名為 index.html，並放置在 .zip 檔案的根目錄中。

若要轉換包含 HTML 的 .zip 檔案，請使用下列程式碼：

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

此函式 `setCustomOptions` 會指定其他 PDF 設定，例如頁面大小。 您可以在此查看將頁面大小設為 11.5 乘以 11 英吋的功能：

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

開啟包含某些術語的 HTML 檔案時，您可以在瀏覽器中取得以下內容：

![電腦條款影像](assets/AWNjs_3.png)

本檔的來源是由 CSS 檔案和 HTML 檔案所組成：

![CSS 和 HTML 檔案的影像](assets/AWNjs_4.png)

處理 HTML 檔案後，PDF 格式的文字相同：

![電腦條款的 PDF 檔案](assets/AWNjs_5.png)

## 附加頁面

另一個常見的 PDF 檔案操作是將可能具有標準文字的頁面附加至末端，例如條款清單。 檔工具組可將數份 PDF 檔合併為單一檔。 如果您有檔路徑清單 （位於此 `sourceFileList` 處），您可以將每個檔案的檔案參考新增到物件以進行合併作業。

當執行合併作業時，它會提供包含來源內容的單一檔案。 您可以在物件上使用 `saveAsFile` ，將檔案儲存起來。

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

## 顯示 PDF 檔

您已在 PDF 檔案上執行數次作業，但最終，您的使用者必須檢視檔。 您可以使用 Adobe 的 PDF 嵌入API，將檔嵌入網頁中。

在顯示 PDF 的頁面上，新增元素 `<div />` 以按住檔並提供 ID。 您很快就會使用此 ID。 在網頁中，納入 `<script />` Adobe JavaScript資料庫的參考：

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

您需要的最後一個程式碼位是載入內嵌API JavaScript Adobe PDF後顯示檔的功能。 當您收到透過 adobe_dc_view\_sdk.ready 事件載入腳本的通知時，請建立新的 AdobeDC.View 物件。 此物件需要您的用戶端 ID 和先前建立之元素的 ID。 在 Adobe Developer ](https://console.adobe.io/tw) Console 中 [ 尋找您的用戶端 ID。當您檢視您先前建立認證時所建立的應用程式的設定時，會顯示用戶端 ID。

![API用戶端金鑰的影像](assets/AWNjs_6.png)

## 其他 PDF 選項

Adobe PDF [ 「嵌入API」示範 ](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) 可讓您預覽其他各種嵌入 PDF 檔的選項。

![嵌入 PDF 選項的影像 ](assets/AWNjs_7.png)

您可以開啟和關閉各種選項，並立即查看其演算方式。 當您找到喜歡的組合時，請按一下 *「產生代碼* 」按鈕，使用這些選項產生實際的 HTML 程式碼。

![程式碼預覽影像](assets/AWNjs_8.png)

## 新增數位簽章和安全性

檔準備就緒後，您可以新增數位簽章，使用Adobe Sign進行核准。 此功能的運作方式與您目前使用的功能有些不同。 對於數位簽章，必須將應用程式設定為使用 OAuth 進行使用者驗證。

設定應用程式的第一步是註冊 [ 應用程式 ](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md) ，以便使用 OAuth 進行Adobe Sign。 登入後，請按一下 *「帳戶* 」導覽至建立應用程式的畫面，然後開 *啟「Adobe Sign API* 」區段，然後按一下 *「API應用程式* 」，開啟已註冊的應用程式清單。

![註冊應用程式的第一步圖像](assets/AWNjs_9.png)

若要建立新的應用程式專案，請按一下右上角的加號圖示。

![畫面右上角加號圖示的影像](assets/AWNjs_10.png)

在開啟的視窗中，輸入應用程式名稱和顯示名稱。 選取 *網域的「客戶* 」，然後按一下「儲存 *」* 。

![輸入應用程式名稱和顯示名稱的位置影像](assets/AWNjs_11.png)

建立應用程式後，您可以從清單中選取該應用程式，然後按一下「 *為應用程式設定 OAuth」* 。 選取選項。 在「重新導向URL」中，輸入應用程式的URL。 您可以在這裡輸入多個 URL。 對於您正在測試的應用程式，其值為：

```
http://localhost:3000/signed-in 
```

使用 OAuth 取得權杖的程式是標準的。 您的應用程式會將使用者引導至URL進行登入。 使用者成功登入後，
這些檔案會重新導向至應用程式，並附上頁面查詢參數中的其他資訊。

對於登入URL，您的應用程式必須傳遞您的用戶端 ID、重新導向URL，以及所需的範圍清單。

URL圖樣如下所示：

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

系統會提示使用者登入其 ID，以索取Adobe Sign。 登入後，系統會詢問他們是否要提供應用程式的許可權。

![確認存取畫面的影像](assets/AWNjs_12.png)

如果使用者在重新導向URL上按一下 *「允許存取* 」，則已命名代碼的查詢參數會傳遞授權代碼：

HTTPs://YourServer.com/?code= **&lt;authorization_code\>\** &amp;api_access_point=HTTPs：//api.adobesign.com&amp;web_access_point=HTTPs：//secure.adobesign.com&lt;/authorization_code\>

將此代碼連同您的用戶端 ID 和用戶端密碼一起發佈到 Adobe Sign 伺服器，可提供存取服務的存取權杖。 儲存參數 `api_access_point` 中的值和 `web_access_point` 。 這些值用於進一步的要求。

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

當檔需要簽名時，必須先上傳檔。 您的應用程式可以將檔上傳至 `api_access_point` 要求 OAUTH 權杖時收到的值。 端點為 `/api/rest/v6/transientDocuments` 。 請求資料如下所示：

```
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.adobesign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"
<PDF CONTENT>
```

在您的應用程式中，使用下列程式碼建立要求：

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

要求會傳回 `transientID` 值。 檔已上傳，但尚未傳送。 若要傳送檔，請使用 `transientID` 請求來傳送檔。

首先，建立包含要簽署的檔資訊的 JSON 物件。 在下列步驟中，變數 `transientDocumentId` 包含來自上述程式碼的 ID，並 `agreementDescription` 包含說明需要簽署合約的文字。 簽署檔的人員會依其電子郵件地址和角色列出 `participantSetsInfo` 。

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

傳送此網頁請求會建立簽署要求，並傳回具有合約要求 ID 的 JSON 物件：

```
request(requestBody, function (error, response) {
  if (error) throw new Error(error);
  var JSONResponse = JSON.parse(response.body);
  var requestId = JSONResponse.id;
});
```

如果簽署者忘記簽署，且需要另一封通知電子郵件，請使用先前收到的 ID 再次傳送通知。 唯一的差別是您也必須新增當事人的參與者 ID。 您可以傳送GET要求以取得參與者 ID。 `/agreements/{agreementID}/members`

若要請求傳送提醒，請先建立描述要求的 JSON 物件。 最小物件需要參與者 ID 清單和提醒狀態 （「ACTIVE」、「完整」或「已取消」）。

此請求可選擇性地提供其他資訊，例如會對使用者顯示的「note」值。 或者，需要延遲 （數小時） 才能傳送提醒 （in `firstReminderDelay` ） 和提醒頻率 （在「頻率」欄位中），以接受值 （例如 DAILY_UNTIL_SIGNED、EVERY_THIRD_DAY_UNTIL_SIGNED 或 WEEKLY_UNTIL_SIGNED）。

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

這一切都需要傳送提醒要求。

![網頁表單影像](assets/AWNjs_13.png)

## 建立網頁表單

您也可以使用Adobe Sign API來建立網頁表單。 網頁表單可讓您將表格嵌入網頁中，或直接連結到該表單。 網頁表單建立後，也會顯示在您 Adobe Sign 主控台的「網頁表單」中。 您可以為增量建立建立具有「草稿」狀態、用於編輯網頁表單欄位的 AUTHORING 狀態，以及可立即代管表單的「作用中」狀態來建立網頁表單。

![「管理」畫面中Adobe Sign網頁表單的影像](assets/AWNjs_14.png)

若要建立網頁表單，請使用表格 `transientDocumentId` 。 決定表單的標題和初始化狀態。

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

您現在可以嵌入檔或連結。

## 後續步驟

從快速啟動和提供的程式碼中可以看出，使用 API 的節點 [!DNL Adobe Acrobat Services] 輕鬆實作 PDF 和數位檔核准程式。 Adobe的 API 會無縫整合至您現有的用戶端應用程式。

若要探索呼叫的必要範圍，或查看呼叫的建置方式，您可以從 [ Rest API 檔 ](https://secure.na4.adobesign.com/public/docs/restapi/v6) 建立範例呼叫。 「 [ 快速星號」 ](https://github.com/adobe/pdftools-node-sdk-samples) 也會示範 API 處理常式的其他功能和檔案格式 [!DNL Adobe Acrobat Services] 。

您可以在應用程式中新增多種 PDF 功能，讓使用者快速輕鬆地檢視和簽署其檔等。 若要開始，請立即查看 [[!DNL Adobe Acrobat Services] ](https://www.adobe.io/apis/documentcloud/dcsdk/) 。
