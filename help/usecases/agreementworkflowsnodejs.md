---
title: Node.js中的協定工作流
description: '[!DNL Adobe Acrobat Services]個API可輕鬆將PDF功能納入Web應用程式'
feature: Use Cases
role: Developer
level: Beginner
type: Tutorial
jira: KT-7473
thumbnail: KT-7473.jpg
keywords: 特色
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: ba73105ecf0bd27b7445ec4388fc4009eec273b8
workflow-type: tm+mt
source-wordcount: '2094'
ht-degree: 0%

---

# Node.js中的協定工作流

![使用案例英雄橫幅](assets/UseCaseAgreementHero.jpg)

許多業務應用程式和流程都需要諸如建議書和協定之類的文檔。 PDF文檔確保檔案更安全，修改更少。 它們還提供數字簽名支援，以便您的客戶能夠快速、輕鬆地完成其文檔。 [!DNL Adobe Acrobat Services]個API可輕鬆將PDF功能納入Web應用程式。

## 你能學到的

在本操作教程中，瞭解如何將PDF服務添加到Node.js應用程式以數字化協定進程。

## 相關API和資源

* [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe SignAPI](https://developer.adobe.com/adobesign-api/)

* [項目代碼](https://github.com/adobe/pdftools-node-sdk-samples)

## 正在設定[!DNL Adobe Acrobat Services]

若要開始，請設定憑據以使用[!DNL Adobe Acrobat Services]。 註冊帳戶並使用[Node.js Quickstart](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js)驗證憑據是否有效，然後再將該功能整合到較大的應用程式中。

首先，獲取Adobe開發商帳戶。 然後，在[開始](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK)頁面上，在「建立新憑據」下選擇「*開始*」選項。 您可以註冊免費試用版，該版本提供1,000個可在6個月內使用的文檔交易記錄。

![建立新憑據的映像](assets/AWNjs_1.png)

在以下「建立新身份證明」頁中，系統將提示您在PDF嵌入API和PDF服務API之間做出選擇。

選擇&#x200B;*PDF服務API*。

輸入應用程式的名稱並選中標有&#x200B;*建立個性化代碼示例*&#x200B;的框。 選中此框會在代碼示例中自動嵌入您的憑據。 如果未選中此框，則必須手動將憑據添加到應用程式。

為應用程式類型選擇&#x200B;*Node.js*，然後按一下&#x200B;*建立憑據*。

幾分鐘後，.zip檔案開始下載，其中包含您的憑據的示例項目。 [!DNL Acrobat Services]的Node.js包已作為示例項目代碼的一部分包含。

![選擇PDF服務API憑據的映像](assets/AWNjs_2.png)

## 手動配置示例項目

如果選擇不從「建立新身份證明」頁下載示例項目，則還可以手動設定項目。

從[GitHub](https://github.com/adobe/pdftools-node-sdk-samples)下載代碼（未嵌入憑據）。 如果使用此版本的代碼，則必須先將憑據添加到pdftools-api-credentials.json檔案中，然後才能使用：

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

對於您自己的應用程式，您需要將私鑰檔案和憑據檔案複製到您的應用程式源。

必須為[!DNL Acrobat Services]安裝Node.js包。 要安裝軟體包，請使用以下命令：

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## 設定日誌記錄

此處的示例使用Express作為應用程式框架。 它們還使用log4js進行應用程式日誌記錄。 使用log4js ，您可以輕鬆將日誌記錄直接到控制台或輸出到檔案：

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

上述代碼將記錄的資料寫入檔案。/logs/applicationlog.txt。 如果希望它寫入控制台，則可以注釋掉對log4js.configure的調用。

## 將Word檔案轉換為PDF

協定和建議通常都寫在生產力應用程式中，比如Microsoft·Word。 要接受此格式的文檔並將文檔轉換為PDF，您可以為應用程式添加功能。 讓我們看一下如何在Express應用程式中上載和保存文檔並將其保存到檔案系統。

在應用程式的HTML中，添加一個檔案元素和一個用於開始上載的按鈕：

```
<input type="file" name="source" id="source" />
<button onclick="upload()" >Upload</button>
```

在頁的JavaScript中，使用fetch函式非同步上載檔案：

```
function upload()
{
  let formData = new FormData();
  var selectedFile = document.getElementById('source').files[0];
  formData.append("source", selectedFile);
  fetch('documentUpload', {method:"POST", body:formData});
}
```

選擇一個資料夾以接受上載的檔案。 應用程式需要指向此資料夾的路徑。 使用與\_\_dirname聯接的相對路徑查找絕對路徑：

```
const uploadFolder = path.join(__dirname, "../uploads");
```

由於檔案是通過帖子提交的，因此您必須對伺服器端的帖子消息做出響應：

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

執行此函式後，檔案將保存在應用程式上載資料夾中，並可供進一步處理。

接下來，將檔案從其本機格式轉換為PDF。 您以前下載的示例代碼包含名為`create-pdf-from-docx.js`的指令碼，用於將文檔轉換為PDF。 以下函式`convertDocumentToPDF`將上載的文檔轉換為其他資料夾中的PDF:

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

您可以注意到帶有代碼的一般模式：

該代碼生成證書對象和執行上下文，初始化一些操作，然後使用執行上下文執行該操作。 在整個示例代碼中，您可以看到這種模式。

通過對上載函式進行一些添加，以便調用此函式，用戶上載的Word文檔現在將自動轉換為PDF。

以下代碼為轉換的PDF生成目標路徑並啟動轉換：

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

## 將其他檔案類型轉換為PDF

文檔工具包將其它格式轉換為PDF，如靜態HTML，另一種常用文檔類型。 該工具包接受打包為.zip檔案的HTML文檔，該文檔引用的所有資源（CSS檔案、影像和其他檔案）都位於同一.zip檔案中。 HTML文檔本身必須命名index.html，並放置在.zip檔案的根中。

要轉換包含HTML的.zip檔案，請使用以下代碼：

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

函式`setCustomOptions`指定其他PDF設定，如頁面大小。 在此，您可以看到將頁面大小設定為11.5 x 11英吋的函式：

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

開啟包含某些術語的HTML文檔時，在瀏覽器中可獲得以下內容：

![電腦術語的映像](assets/AWNjs_3.png)

此文檔的源由CSS檔案和HTML檔案組成：

![CSS和HTML檔案的影像](assets/AWNjs_4.png)

處理HTML檔案後，您的PDF格式相同：

![電腦條款的PDF檔案](assets/AWNjs_5.png)

## 添加頁面

使用PDF檔案的另一個常見操作是將可能具有標準文本（如術語清單）的頁面附加到末尾。 文檔工具包可以將多個PDF文檔合併為單個文檔。 如果您有文檔路徑清單（在`sourceFileList`中），則可以將每個檔案的檔案引用添加到組合操作的對象。

當執行組合操作時，它提供一個包含源內容的單個檔案。 您可以在對象上使用`saveAsFile`將檔案保留到儲存。

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

## 顯示PDF文檔

您已對PDF檔案執行了多項操作，但最終，您的用戶必須查看文檔。 可以使用Adobe的PDF嵌入API將文檔嵌入到網頁中。

在顯示PDF的頁面上，添加一個`<div />`元素以保存文檔，並為其提供ID。 您很快就使用此ID。 在網頁中，包括對AdobeJavaScript庫的`<script />`引用：

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

您最後需要的代碼位是一個函式，在載入Adobe PDF嵌入API JavaScript後，該函式將顯示文檔。 當您收到通過adobe_dc_view\_sdk.ready事件載入指令碼的通知時，請建立新的AdobeDC.View對象。 此對象需要您的客戶端ID和先前建立的元素的ID。 在[Adobe Developer Console](https://developer.adobe.com/console/)中查找您的客戶端ID。 當您查看先前生成憑據時建立的應用程式的設定時，將顯示客戶端ID。

![API客戶端密鑰的映像](assets/AWNjs_6.png)

## 其他PDF選項

[Adobe PDF嵌入API演示](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf)使您能夠預覽嵌入PDF文檔的各種其他選項。

![嵌入PDF選項的影像](assets/AWNjs_7.png)

您可以開啟和關閉各種選項，並立即查看它們的呈現方式。 當您找到喜歡的組合時，按一下&#x200B;*\&lt;/\>生成代碼*&#x200B;按鈕，使用這些選項生成實際的HTML代碼。

![代碼預覽影像](assets/AWNjs_8.png)

## 添加數字簽名和安全性

文檔準備好後，您可以使用Adobe Sign添加數字簽名以供批准。 此功能與您到目前為止使用的功能有一點不同。 對於數字簽名，必須將應用程式配置為使用OAuth進行用戶身份驗證。

設定應用程式的第一步是[註冊您的應用程式](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/gstarted/create_app.md)以將OAuth用於Adobe Sign。 登錄後，通過按一下&#x200B;*帳戶*&#x200B;導航到用於建立應用程式的螢幕，然後開啟&#x200B;*Adobe SignAPI*&#x200B;部分，然後按一下&#x200B;*API應用程式*&#x200B;以開啟已註冊的應用程式清單。

![註冊應用程式的第一步的映像](assets/AWNjs_9.png)

要建立新應用程式條目，請按一下右上角的加號表徵圖。

![螢幕右上角的加號表徵圖的影像](assets/AWNjs_10.png)

在開啟的窗口中，輸入應用程式名稱和顯示名稱。 為域選擇&#x200B;*客戶*，然後按一下&#x200B;*保存*。

![輸入應用程式名稱和顯示名稱的影像](assets/AWNjs_11.png)

建立應用程式後，可以從清單中選擇它，然後按一下&#x200B;*為應用程式*&#x200B;配置OAuth。 選擇選項。 在重定向URL中，輸入應用程式的URL。 可以在此處輸入多個URL。 對於您正在測試的應用程式，其值為：

```
http://localhost:3000/signed-in 
```

使用OAuth獲取令牌的過程是標準的。 您的應用程式將用戶引導到要登錄的URL。 用戶成功登錄後，
在頁面的查詢參數中包含附加資訊時，會將它們重定向回應用程式。

對於登錄URL，您的應用程式必須傳遞您的客戶端ID、重定向URL以及所需作用域的清單。

URL的模式如下所示：

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

系統提示用戶登錄其Adobe Sign的ID。 登錄後，系統會詢問他們是否向應用程式提供權限。

![確認訪問螢幕的影像](assets/AWNjs_12.png)

如果用戶按一下重定向URL上的&#x200B;*允許訪問*，則名為代碼的查詢參數將傳遞授權代碼：

https://YourServer.com/?code=**\<授權代碼\>**\&api_access_point=https://api.adobesign.com&web_access_point=https://secure.adobesign.com

將此代碼連同您的客戶端ID和客戶端密鑰一起發佈到Adobe Sign伺服器，提供了訪問該服務的訪問令牌。 保存參數`api_access_point`和`web_access_point`中的值。 這些值用於進一步的請求。

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

當文檔需要簽名時，必須首先上載文檔。 您的應用程式可以將文檔上載到請求OAUTH令牌時收到的`api_access_point`值。 終結點為`/api/rest/v6/transientDocuments`。 請求資料如下所示：

```
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.adobesign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"
<PDF CONTENT>
```

在您的應用程式中，使用以下代碼生成請求：

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

請求返回`transientID`值。 文檔已上載，但尚未發送。 若要發送文檔，請使用`transientID`請求發送文檔。

首先構建包含要簽名的文檔資訊的JSON對象。 在下面的代碼中，變數`transientDocumentId`包含來自上述代碼的ID，而`agreementDescription`包含描述需要簽名的協定的文本。 要簽署文檔的人員在`participantSetsInfo`中按其電子郵件地址和角色列出。

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

發送此Web請求將生成簽名請求並返回JSON對象，其ID為協定請求：

```
request(requestBody, function (error, response) {
  if (error) throw new Error(error);
  var JSONResponse = JSON.parse(response.body);
  var requestId = JSONResponse.id;
});
```

如果簽名者忘記簽名並需要另一封通知電子郵件，請使用先前收到的ID再次發送通知。 唯一的區別是您還必須添加參與方的參與方ID。 您可以通過向`/agreements/{agreementID}/members`發送GET請求來獲取參與者ID。

要請求發送提醒，請首先生成描述該請求的JSON對象。 最小對象需要參與者ID的清單和提醒狀態（「ACTIVE」、「COMPLETE」或「CANCELLED」）。

請求可以有附加資訊，例如將顯示給用戶的「注釋」值。 或者，等待發送提醒的延遲（以小時為單位）（在`firstReminderDelay`中），以及提醒頻率（在欄位&quot;frequency&quot;中），該提醒頻率接受DAILY_UNTIL_SIGNED、EVERY_THIRD_DAY_UNTIL_SIGNED或WEEKLY_UNTIL_UNTIL_SIGNED_SINTIN_SIGNED_S_S。

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

這就是發出提醒請求的全部。

![Web窗體的映像](assets/AWNjs_13.png)

## 建立Web表單

您還可以使用Adobe SignAPI建立Web表單。 Web表單使您能夠將表單嵌入到網頁中或直接連結到該網頁。 建立Web表單後，它還會顯示在Adobe Sign控制台的Web表單中。 可以建立具有DRAFT狀態的Web表單以用於增量構建、用於編輯Web表單域的AUTHORING狀態以及立即托管表單的ACTIVE狀態。

![「Adobe Sign管理」螢幕中的Web表單影像](assets/AWNjs_14.png)

要建立Web表單，請使用表單`transientDocumentId`。 確定窗體的標題和初始化窗體的狀態。

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

您現在可以嵌入或連結到文檔。

## 後續步驟

如您從快速啟動和提供的代碼中所看到的，使用帶有[!DNL Adobe Acrobat Services]個API的Node可輕鬆實現PDF和數字文檔批准過程。 Adobe的API無縫整合到您現有的客戶端應用程式中。

要發現調用所需的作用域，或查看如何生成調用，可以從[Rest API文檔](https://secure.na4.adobesign.com/public/docs/restapi/v6)生成示例調用。 [Quickstarts](https://github.com/adobe/pdftools-node-sdk-samples)還演示了[!DNL Adobe Acrobat Services] API進程的其他功能和檔案格式。

您可以向應用程式添加多種PDF功能，使用戶能夠快速、輕鬆地查看和簽署其文檔等。 若要開始，請立即簽出[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage/)。
