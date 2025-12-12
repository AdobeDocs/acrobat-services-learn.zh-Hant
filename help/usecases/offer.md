---
title: 管理員工聘用函
description: 瞭解如何生成可交付給新員工以供其簽名的聘用函
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8096
thumbnail: KT-8096.jpg
exl-id: 92f955f0-add5-4570-aa3a-ea63055dadb2
source-git-commit: bd53d86abb0e5f9ee302c39e07c00101e5a1f8ed
workflow-type: tm+mt
source-wordcount: '1714'
ht-degree: 0%

---

# 管理員工聘用函

![使用案例英雄橫幅](assets/UseCaseOfferHero.jpg)

員工聘函是員工與您的組織最早的體驗之一。 因此，您希望確保您的優惠信是品牌上的，但您不希望每次都從頭開始在文字處理器中構建字母。 [!DNL Adobe Acrobat Services]個API提供了一種快速、簡單和有效的方法，可處理[的關鍵部分，以生成並向新員工提供聘用信](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/employee-offer-letters)。

## 你能學到的

本實踐教程將介紹如何設定一個Node Express項目，該項目顯示一個Web表單，供用戶用僱員詳細資訊填充。 這些詳細資訊通過Web使用[!DNL Acrobat Services]生成優惠信作為PDF，可以通過Adobe SignAPI將其發送給客戶以供其簽名。

## 相關API和資源

* [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe文檔生成API](https://developer.adobe.com/document-services/apis/doc-generation)

* [Adobe SignAPI](https://developer.adobe.com/adobesign-api/)

* [文檔生成標籤Word載入項](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin)

* [項目示例](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/employee-offer-letters)

## 快速入門

[Node.js](https://nodejs.org/)是寫程式平台。 它附帶了大量庫，如Express Web伺服器。 [下載Node.js](https://nodejs.org/en/download/)，然後按照步驟安裝此良好的開源開發環境。

要在Node.js中使用Adobe文檔生成API，請轉到[文檔生成API](https://developer.adobe.com/document-services/apis/doc-generation)站點以訪問您的帳戶或註冊新帳戶。 您的帳戶[在6個月內免費，然後按單價支付](https://developer.adobe.com/document-services/pricing/main)，每個文檔交易僅需0.05美元，因此您可以嘗試無風險，然後僅在公司增長時支付。

登錄到[Adobe Developer Console](https://developer.adobe.com/console/)後，按一下&#x200B;**[!UICONTROL 新建項目]**。 預設情況下，項目名為「項目1」。 按一下&#x200B;**[!UICONTROL 編輯項目]**&#x200B;按鈕，將名稱更改為「提供信件生成器」。 螢幕中心是&#x200B;**[!UICONTROL 開始使用新項目]**&#x200B;節。 要啟用項目安全性，請執行以下步驟：

按一下&#x200B;**添加API**。 您可以看到許多API可供選擇。 在&#x200B;**[!UICONTROL 按產品篩選]**&#x200B;部分，選擇&#x200B;**[!UICONTROL Document Cloud]**，然後按一下&#x200B;**[!UICONTROL 下一步]**。

現在，生成訪問API的憑據。 憑據的形式為JSON Web令牌([JWT](https://jwt.io/))：安全通信的開放標準。 如果您熟悉JWT並且已生成密鑰，則可以在此處上載公鑰。 或者，選擇&#x200B;**選項1**&#x200B;以讓Adobe為您生成密鑰，繼續。

![生成憑據的螢幕快照](assets/offer_1.png)

按一下&#x200B;**[!UICONTROL 生成密鑰對]**&#x200B;按鈕。 您可以下載config.zip檔案。 解壓縮存檔檔案。 它包含兩個檔案：certificate_pub.crt和private.key。 確保後者是安全的，因為後者包含您的私有憑據，並且可能用於在您無法控制的情況下生成虛假文檔。

按&#x200B;**[!UICONTROL 「下一步」]**。否，啟用對PDF生成API的訪問。 在&#x200B;**[!UICONTROL 選擇產品配置檔案]**&#x200B;螢幕上，選中&#x200B;**[!UICONTROL 企業PDF服務開發人員]**，然後按一下&#x200B;**[!UICONTROL 保存配置的API]**&#x200B;按鈕。 現在，您已準備好開始使用API。

## 設定項目

設定節點項目以運行代碼。 此示例使用[Visual Studio代碼](https://code.visualstudio.com/)（VS代碼）作為編輯器。 建立名為「letter-generator」的資料夾，並在VS代碼中將其開啟。 從&#x200B;**[!UICONTROL 檔案]**&#x200B;菜單中，選擇&#x200B;**[!UICONTROL 終端]** \> **[!UICONTROL 新終端]**&#x200B;以在此資料夾中開啟Shell。 輸入以下命令，檢查節點是否已安裝並位於路徑上：

```
node -v
```

您應該看到已安裝的節點版本。

現在您已安裝了開發環境，您可以繼續建立項目。

首先，使用節點包管理器(npm)初始化項目。 鍵入以下內容：

```
npm init
```

您會被問到一些有關節點項目的問題。 您可以跳過這些問題中的大多數，但請確保項目名稱為「letter-generator」，入口點為&#x200B;**index.js**。 選擇&#x200B;**是**&#x200B;以完成項目初始化。

您現在擁有package.json檔案。 節點使用此檔案來組織項目。 在建立index.js之前，必須添加具有以下內容的Adobe庫
命令：

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

應將名為node_modules的新資料夾添加到項目中。 此資料夾是下載所有庫（在「節點」中稱為依賴項）的位置。 還將使用對Adobe PDF服務的引用更新package.json檔案。

現在，您希望將Express作為輕量級Web框架進行安裝。 輸入下列命令：

```
npm install express –save
```

與以前一樣，將相應更新package.json的dependencies部分。

## 建立聘用函模板

現在，在項目根目錄中，建立一個名為&quot;app.js&quot;的檔案。 讓我們在其中放入以下起始代碼：

```
const express = require('express');
const bodyParser = require('body-parser');
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk')
const path = require('path');
const app = express();
const port = 8000;
app.use(bodyParser.urlencoded({ extended: true }));
app.get('/', (req, res) => {
res.sendFile(path.join(__dirname + '/index.html'));
});
app.post('/', (req, res) => {
console.log('Got body:', req.body);
res.sendStatus(200);
});
app.listen(port, () => {
console.log(`Candidate offer letter app listening on port ${port}!`)
});
```

請注意，獲取路由返回&#x200B;**index.html**&#x200B;檔案。 讓我們使用該名稱和以下簡單格式建立HTML檔案。 您以後可以根據需要添加CSS樣式和其他設計元素。 此表格包含候選人生成歡迎信的基本詳細資訊：

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Offer Letter Generator</title>
</head>
<body>
<h1>Enter Candidate Details</h1>
<form action="" method="post">
<div>
<label for="firstname">First name: </label>
<input type="text" name="firstname" id="firstname" required>
</div>
<div>
<label for="lastname">Last name: </label>
<input type="text" name="lastname" id="lastname" required>
</div>
<div>
<label for="salary">Salary ($): </label>
<input type="number" name="salary" id="salary" required>
</div>
<div>
<label for="startdate">Start Date: </label>
<input type="date" name="startdate" id="startdate" required>
</div>
<div>
<input type="submit" value="Generate Letter">
</div>
</form>
</body>
</html>
```

使用以下命令運行Web伺服器：

```
node app.js
```

您應該看到「Candiate offer letter app listening on port 8000（在埠8000上偵聽候選項提供信函應用）」的消息。 如果將瀏覽器開啟到<http://localhost:8000/>，表單應如下所示：

![Web表單螢幕截圖](assets/offer_2.png)

請注意，表單將自行發佈。 如果填寫資料並按一下&#x200B;**生成字母，**&#x200B;應在控制台上看到以下資訊：

```
Got body: { firstname: 'John',
lastname: 'Doe',
salary: '887888',
startdate: '2021-04-01' }
```

您將此控制台日誌記錄替換為對[!DNL Acrobat Services]的Web服務調用。 首先，必須建立基於JSON的資訊模型。 此模型的格式如下：

```
{
    "offer_letter": {
    "firstname": "John",
    "lastname": "Doe",
    "salary": "887888",
    "startdate": "2021-04-01"
    }
}
```

如果您願意，可以使此模型更加詳細，但在本教程中，請繼續使用此簡單示例。 此表單沒有驗證，因為此表單不在本文的範圍內。 要將表單主體轉換為上述資料模型，請將app.post處理程式方法更改為具有以下代碼：

```
app.post('/', (req, res) => {
const docModel = {'offer_letter': req.body};
generateLetter(docModel);
res.sendStatus(200);
});
```

第一行將JSON資料置於所需格式。 現在，您將此資料傳遞給generateLetter函式。 停止伺服器並在app.js的末尾貼上以下代碼。 此代碼將Word文檔作為模板，並用JSON文檔中的資訊填充佔位符。

```
// Letter generation function
function generateLetter(jsonDataForMerge) {
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge,
documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(
'resources/OfferLetter-Template.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/OfferLetter.pdf'))
.catch(err => {
if(err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log(
'Exception encountered while executing operation', err);
} else {
console.log(
'Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

要解包那兒的代碼很多。 我們先看主要部分： `documentMergeOperation`。 此部分是您獲取JSON資料並將其與Word文檔模板合併的位置。 您可以將Adobe站點[上的](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade)示例用作引用，但讓我們製作您自己的簡單示例。 開啟Word並建立新的空白文檔。 您可以隨心所欲地定制它，但至少有這樣的功能：

親愛的X:

我們很高興能以每年X美元的價格為您提供一個職位。 你的起始日期是X。

歡迎

將文檔保存為「OfferLetter-Template.docx」，位於項目根目錄中名為「resources」的資料夾中。 注意文檔中的三個X。 這些X是JSON資訊的臨時佔位符。 雖然可以使用特殊語法來替換這些佔位符，但Adobe提供了可簡化此任務的Word載入項。 要安裝載入項，請轉到Adobe[文檔生成標籤Word載入項](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin)站點。

在您的OfferLetter-Template中，按一下新的&#x200B;**文檔生成**&#x200B;按鈕。 側面板開啟。 按一下&#x200B;**開始**。 您有一個要貼上到示例JSON資料中的文本區域。 將JSON的「offer-data」代碼段從上複製到文本區域。 它應如下所示：

![字母和代碼螢幕快照](assets/offer_3.png)

按一下&#x200B;**生成標籤**&#x200B;按鈕。 您將獲得一個標籤下拉菜單，以插入文檔中的適當點中。 突出顯示文檔中的第一個X，然後選擇&#x200B;**[!UICONTROL 名字]**。 按一下&#x200B;**[!UICONTROL 插入文本]**，「親愛的X，」更改為「親愛的```{{`offer_letter`.firstname}}```，」。 此標籤是`documentMergeOperation`的正確格式。 繼續，在相應的X上添加其餘三個標籤。 不要忘記保存OfferLetter-template.docx。 它應該是這樣的：

親愛的 ```{{`offer_letter`.firstname}} {{`offer_letter`.lastname}}```，

我們很高興為您提供一年$ ```{{`offer_letter`.salary}}```的職位。 您的開始日期為```{{`offer_letter`.startdate}}```。

歡迎

現在，Word模板具有與JSON格式匹配的標籤。 例如，Word文檔開頭的```{{`offer_letter`.`firstname`}}```被JSON資料的「firstname」部分中的值替換。

返回到`generateLetter`函式。 要保護REST調用的安全，請在項目根目錄中建立一個名為pdftools-api-credentials.json的新檔案。 貼上以下JSON資料，並使用[開發人員控制台](https://developer.adobe.com/console/)的「服務帳戶(JWT)」部分的詳細資訊調整它。

```
{
"client_credentials": {
"client_id": "<YOUR_CLIENT_ID>",
"client_secret": "<YOUR_CLIENT_SECRET>"
},
"service_account_credentials": {
"organization_id": "<YOUR_ORGANIZATION_ID>",
"account_id": "<YOUR_TECHNICAL_ACCOUNT_ID>",
"private_key_file": "<PRIVATE_KEY_FILE_PATH>"
}
}
```

* 可以從控制台的&#x200B;**[!UICONTROL 憑據詳細資訊]**&#x200B;部分直接複製客戶端ID、客戶端密碼和組織ID。

* 帳戶ID是&#x200B;**技術帳戶ID**。

* 將先前生成的private.key檔案複製到項目中，並在項目的private_key_file部分中輸入其名稱
pdftools-api-credentials.json檔案。 如果您願意，可以在此處放置私鑰檔案的路徑。 請記住要保持安全，因為一旦失控，它就會被誤用。

要生成已填入JSON資料的PDF，請返回&#x200B;**[!UICONTROL 輸入候選詳細資訊]** Web表單並發佈一些資料。 由於文檔必須從Adobe下載，因此需要一些時間，但您應該在標題為「輸出」的新資料夾中有一個名為OfferLetter.pdf的檔案。

## 後續步驟

就這樣！ 這只是個開始。 如果您研究Word載入項的「文檔生成」頁籤的「高級」部分，您會注意到並非所有佔位符標籤都來自關聯的JSON資料。 您還可以添加簽名標籤。 這些標籤允許您將生成的文檔上載到[Adobe Sign](https://www.adobe.com/ca/sign.html)，以便交付並簽名到新員工。 閱讀Adobe SignAPI入門以瞭解如何操作。 此過程類似，因為您使用的是使用JWT令牌保護的REST調用。

上面提供的單個文檔示例可用作應用程式的基礎，因為組織必須[跨多個地點增加季節性雇用](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/employee-offer-letters)名員工。 如圖所示，主要流程是通過線上應用程式從候選者處獲取資料。 該資料用於填充聘用函的欄位，並將其發出以進行電子簽名。

[!DNL Adobe Acrobat Services]可免費使用6個月，然後[按單次付費](https://developer.adobe.com/document-services/pricing/main)，每個文檔交易只需0.05美元，因此您可以嘗試它，並隨著業務增長擴展您的優惠信工作流。 到[開始](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
正在構建您自己的模板，[註冊您的開發人員帳戶](https://developer.adobe.com/)。

