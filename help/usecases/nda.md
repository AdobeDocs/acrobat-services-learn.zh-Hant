---
title: 建立NDA
description: 瞭解如何建立動態NDAPDF以進行協作
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8098
thumbnail: KT-8098.jpg
exl-id: f4ec0182-a46e-43aa-aea3-bf1d19f1a4ec
source-git-commit: ba73105ecf0bd27b7445ec4388fc4009eec273b8
workflow-type: tm+mt
source-wordcount: '1072'
ht-degree: 0%

---

# 建立NDA

![使用案例英雄橫幅](assets/UseCaseNDAHero.jpg)

組織與外部貢獻者協作構建其服務和產品。 保密協定是這些合作中的重要內容。 它要求所有各方不得發佈任何可能損害任何實體的機密資訊。

使用最廣的NDA格式是PDF文檔。 各組織準備一份NDA並將其發送給所有各方。 然後，一旦大家簽了字，就開始簽合同。 在高速團隊中，手動PDF建立會減慢進度。

## 你能學到的

本實踐教程介紹如何為您的公司建立專門的MicrosoftWord NDA模板。 Adobe的MicrosoftWord免費載入項，[Adobe文檔生成標籤](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)，插入「標籤」以輸入動態值。 瞭解如何將JSON資料傳遞到模板並建立動態PDF。 生成的PDF可以通過電子郵件發送或在瀏覽器中顯示給您的合作者，具體取決於您的業務要求和目標。 接下來，您只需在Node.js、JavaScript、Express.js、HTML和CSS上體驗一下。

## 相關API和資源

使用[!DNL Adobe Acrobat Services]，可以使用動態資料即時生成PDF文檔。 [!DNL Acrobat Services]提供了一套PDF工具，包括Adobe文檔生成API以自動建立[NDA](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation)。

* [Adobe文檔生成API](https://developer.adobe.com/document-services/apis/doc-generation)

* [Adobe SignAPI](https://developer.adobe.com/adobesign-api/)

* [Adobe文檔生成標誌](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

* [項目代碼](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)

* [[!DNL Acrobat Services] 個鍵](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred)

## 建立JSON模型

MicrosoftWord模板取決於JSON模型，因此您首先建立該模型。 在本教程中，您使用包含公司詳細資訊（如聯繫資訊）的基本JSON結構。

```
{
"vendor": {
"companyName": "GlobalCorp",
"street": "123 Any Street",
"street2": "",
"city":"Anywhere",
"state":"CA",
"primaryContact": {
"firstName":"John",
"lastName":"Doe",
"email":"john-doe@example.com",
"phone":"123-456-7890"
}
},
"authorizedSigner": {
"firstName": "Sarah",
"lastName": "Rose",
"email": "sarah@example.com",
"phone":"555-555-1234"
}
}
```

您可以在MicrosoftWord中使用此結構來生成模板。 此資料可以來自任何資料源，只要它是JSON格式。 為簡單起見，您在Node.js應用程式內建立了多個檔案，但您的使用情形可能需要資料庫連接來獲取供應商資訊。

## 建立MicrosoftWord模板

在MicrosoftWord文檔中建立NDA模板。 Adobe PDF服務API要求MicrosoftWord文檔包含標籤，服務可以在標籤中從JSON文檔中插入值。 儘管所有Adobe請求的模板都相同，但JSON中的動態資料會更改。 這些標籤有助於為此情況下的每個供應商建立PDF文檔，使用單個MicrosoftWord模板，並通過自動生成NDA文檔來加快流程。

您可以將[免費文檔生成標誌器載入項](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)安裝到MicrosoftWord。 如果您是組織的一部分，則可以請求您的Microsoft辦公室管理員為每個人安裝免費載入項。

安裝了外接程式後，可以在「首頁」(Home)頁籤的「Adobe」(Category)類別下找到該外接程式。 要開啟該頁籤，請選擇&#x200B;**文檔生成**:

![Word](assets/nda_1.png)中文檔生成載入項的螢幕快照

在頁籤內，您可以上載示例JSON文檔。 此文檔可以是示例，因為您僅使用它建立MicrosoftWord模板。

![文檔生成載入項中示例資料的螢幕快照](assets/nda_2.png)

選擇&#x200B;**生成標籤**&#x200B;以查看可在模板內使用的項目。 以下是從JSON結構中提取的屬性，可供在模板中使用：

![文檔生成載入項中文本標籤的螢幕快照](assets/nda_3.png)

這些是`authorizedSigner`欄位中的功能。 其它欄位已開啟，您可以在MicrosoftWord中展開視圖。 外接程式還提供高級資料選項，如表、清單、計算值等。

## 建立標籤

您可以隨意建立模板或將[現有模板](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade)導入MicrosoftWord。 設定文檔後，通過按一下載入項中的相應標籤將標籤添加到每個欄位。

MicrosoftWord檔案中的以下模板：

![示例模板的螢幕快照](assets/nda_4.png)

此檔案包含多個標籤。 運行程式時，這些欄位將填入供應商資訊。

文檔生成Tagger與Adobe SignAPI整合。 由於此整合，您可以自動建立簽名文本標籤，以便生成的文檔隨後可以發送到Adobe Sign進行簽名。

## 為供應商生成NDA

在示例應用程式中，為輸入和輸出準備了資料夾。 如前所述，您使用JSON檔案，以便有兩個檔案顯示系統中的可用供應商。 檔案顯示在瀏覽器上打印的表單中：

```
<h1><b>NDA</b>: Generate for vendor.</h1>
<hr />
<p>Following ({{files.length}}) vendors are ready, select to generate NDA and deliver for signature:</p>
<form method="POST">
<ul>
{{#each files }}
<li><input type="checkbox" name="vendor" value="{{this}}" id="file-{{@index}}" /> <label for="file-{{@index}}">{{this}}</label></li>
{{/each}}
</ul>
<input type="submit" value="Create NDA" />
</form>
```

此代碼在瀏覽器中生成以下用戶介面(UI):

![建立NDA用戶介面的螢幕快照](assets/nda_5.png)

當管理員選擇人員時，應用會使用Adobe PDF服務在途中生成NDA。

```
async function compileDocFile(json, inputFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials);
// create the operation
const documentMerge = adobe.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(json, documentMergeOptions.OutputFormat.PDF);
const operation = documentMerge.Operation.createNew(options);
// Pass the content as input (stream)
const input = adobe.FileRef.createFromLocalFile(inputFile);
operation.setInput(input);
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

在Express路由器內使用此代碼：

```
// Create one report and send it back
try {
console.log(`[INFO] generating the report...`);
const fileContent = fs.readFileSync(`./public/documents/raw/${vendor}`, 'utf-8');
const parsedObject = JSON.parse(fileContent);
await pdf.compileDocFile(parsedObject, `./public/documents/template/Adobe-NDA-Sample.docx`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("preview", { page: 'nda', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

您可以在GitHub上查看[完整的示例代碼](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)。

此代碼在對[!DNL Adobe Acrobat Services] SDK的API調用中使用JSON文檔和MicrosoftWord模板。 在響應中，您將收到輸出並將其保存到應用程式的檔案系統。 您可以通過電子郵件將生成的文檔轉發給客戶端，或使用免費[Adobe PDF嵌入API](https://developer.adobe.com/document-services/apis/pdf-embed)在瀏覽器內顯示預覽。

此調用將建立以下NDA文檔：

![NDA文檔預覽的螢幕截圖](assets/nda_6.png)

[!DNL Adobe Acrobat Services]個API插入內容以建立PDF文檔。 如果沒有這些工具，您可能必須編寫代碼以處理Office文檔並使用原始PDF檔案格式。 在Adobe PDF服務的幫助下，您可以通過單個API調用完成所有這些步驟。

現在，使用[Adobe SignAPI](https://developer.adobe.com/adobesign-api/)請求NDA上的簽名，並將最終簽名的文檔交付給所有各方。 Adobe Sign通知您[使用Webhook](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/webhooks.md)。 收聽此網頁掛接，可獲取NDA的狀態。

有關Adobe Sign進程的更詳細說明，請[查閱文檔](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html)或閱讀此深入的部落格。

## 後續步驟

在本操作教程中，Adobe文檔生成標誌器用於使用MicrosoftWord模板和JSON資料檔案動態生成PDF文檔。 該載入項幫助[自動為每個參與方建立自定義的NDA](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation)，然後使用簽名API收集簽名。

您可以使用這些技術動態建立您自己的NDA或其他文檔，從而騰出您團隊的時間專注於高效工作。 瀏覽[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/apis/pdf-services)以查找所選語言和運行時的API和SDK，以便您可以直接向應用程式添加PDF函式以快速建立PDF文檔。 [開始](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)，試用期為6個月
[按使用付費](https://developer.adobe.com/document-services/pricing/main)，每個文檔交易僅0.05 USD。
