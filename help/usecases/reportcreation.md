---
title: 報告建立與編輯
description: 學習如何在您的網站上為客戶產生 PDF 報告
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8093
thumbnail: KT-8093.jpg
exl-id: 2f2bf1c2-1b33-4eee-9fd2-5d0b77e6b0a9
TQID: https://experienceleague.adobe.com/E6grXk4Sptkhetpgt-MoDndf-Ezm97qnutLbUqD-a20
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
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 1391
ht-degree: 0%

---

# 報告建立與編輯

![使用案例英雄卡池](assets/UseCaseReportHero.jpg)

金融、教育、行銷及其他產業使用 PDF 與客戶及利害關係人分享資料。 PDF 讓分享豐富的文件變得簡單，包含表格、圖像和互動內容，且格式是所有人都能瀏覽。 [!DNL Adobe Acrobat Services] API 協助這些公司從 Microsoft Word、Microsoft Excel、圖形及其他多元文件格式生成可分享的 PDF 報告。

假設你 [經營一家社群媒體追蹤公司](https://developer.adobe.com/document-services/use-cases/content-publishing/on-demand-report-creation)。 你的客戶登入網站中受密碼保護的部分，查看他們的活動分析結果。 他們通常希望將這些統計數據分享給高管、股東、捐助者或其他利害關係人。 可下載的 PDF 文件是客戶分享數字、圖表等資訊的絕佳方式。

透過將 PDF 服務 API[&#128279;](https://developer.adobe.com/document-services/apis/pdf-services) 整合到您的網站，您可以隨時隨地為每位客戶產生 PDF 報告。你可以製作 PDF，然後將它們合併成一份方便客戶下載並轉交給利害關係人的報告。

## 你可以學到什麼

在這個實作教學中，學習如何在Node.js與Express.js環境中（只需一些 JavaScript、HTML 和 CSS）使用 PDF Services SDK，快速且輕鬆地為現有網站新增 PDF 導向的功能。 本網站設有管理員上傳報告的頁面，客戶可查看可用報告清單並選擇文件轉成 PDF，並有方便下載系統產生 PDF 的端點。

## 相關 API 與資源

* [PDF 服務 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF 嵌入 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

## 客戶活動報告儀表板

>[!NOTE]
>
>本教學並非在討論Node.js最佳實務或如何保護你的網頁應用程式。 網站部分區域對外開放，文件命名可能不適合生產環境。 若要討論設計此類系統的最佳方法，請諮詢您的架構師與工程師。

這裡有一個基本的Express.js網頁應用程式，包含客戶報告區和管理員區塊。 此應用程式可展示社群媒體活動的報告。 例如，它可以顯示廣告被點擊的次數。

![如何取得個人化報告的截圖](assets/report_1.png)

你可以從 [GitHub 倉庫](https://github.com/afzaal-ahmad-zeeshan/express-adobe-pdf-tools)下載 這個專案。

現在，讓我們來探討如何發布這些報告。

## 上傳報告

為了簡單起見，這裡只使用基於檔案系統的上傳和處理。 在 Express.js 裡，你可以用 fs 模組列出目錄下所有可用的檔案。

在同一頁面，啟用管理員上傳報告檔案到伺服器，讓客戶查看。 這些檔案可以有許多不同格式，例如 Microsoft Word、Microsoft Excel、HTML [以及其他資料格式](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf) ，包括圖形檔案。 管理員頁面長這樣：

![管理員能力截圖](assets/report_2.png)

>[!NOTE]
>
>請對網址設密碼保護，或使用 npm 的護照套件，將應用程式保護在認證與授權層後面。

當管理員選擇並上傳檔案時，該檔案會被移至公開儲存庫，供其他人存取。 你用同一個倉庫從管理員頁面發佈文件，並列出客戶可用的行銷報告。 這段代碼如下：

```
router.get('/', (req, res) => {
try {
let files = fs.readdirSync('./public/documents/raw') // read the files
res.status(200).render("reports", { page: 'reports', files: files });
} catch (error) {
res.status(500).render("crash", { error: error });
}
});
```

這段程式碼列出所有檔案，並呈現檔案清單的視圖。

## 報告選擇

在使用者端，你有一個表單讓客戶選擇想納入社群媒體活動報告的文件。 為了簡化起見，在範例頁面上，只顯示文件名稱和一個選擇文件的勾選框。 客戶可以選擇單一報告或多個報告，合併成單一 PDF 文件。

為了更進階的使用者介面，你也可以在這裡展示報告預覽。

![客戶能力截圖](assets/report_3.png)

## 產生 PDF 報告

使用 PDF Services SDK 從你的資料輸入中建立 PDF 報告。 資料（如上方截圖所示）可能來自多種資料格式，如 Microsoft Word、Microsoft Excel、HTML、圖形等。 首先安裝 PDF Services SDK 的 npm 套件。

```
$ npm install --save @adobe/documentservices-pdftools-node-sdk
```

開始前，你必須擁有 Adobe[&#128279;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred) 免費提供的 API 憑證。免費使用帳戶[!DNL Acrobat Services][六個月，然後以每筆文件交易 \$0.05 的隨用](https://developer.adobe.com/document-services/pricing/main)付費方式使用。

下載歸檔檔案，並解壓 JSON 檔案作為憑證和私鑰。 在範例專案中，你會把檔案放到 src 目錄。

![src 目錄截圖](assets/report_4.png)

現在你已經設定好憑證，就可以開始寫 PDF 轉換任務了。 在此示範中，你必須在應用程式中執行兩個操作：

* 將原始文件轉換成 PDF 檔案

* 將多個 PDF 檔案合併在一份報告中

整體流程與執行任何行動相似。 唯一的差別是你使用的服務。 在以下程式碼中，你將原始文件轉換成 PDF 檔案：

```
async function createPdf(rawFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials),
operation = adobe.CreatePDF.Operation.createNew();
// Pass the content as input (stream)
const input = adobe.FileRef.createFromLocalFile(rawFile);
operation.setInput(input);
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

在上面的程式碼中，你讀取憑證並建立執行上下文。 PDF Services SDK 需要執行上下文來驗證你的請求。

接著，執行「建立 PDF」操作，將原始文件轉換成 PDF 格式。 最後，你用參數 `outputPdf` 複製 PDF 報告。 在範例程式碼中，你可以在 src/helpers/pdf.js 檔案中找到這段程式碼。 在這個教學後面，你會匯入 PDF 模組並呼叫這個方法。

如前一節所示，您的客戶可前往以下頁面選擇想轉換成 PDF 的報告：

![客戶能力截圖](assets/report_3.png)

當客戶選擇一個或多個這些報告時，你會建立 PDF 檔案。

首先，讓我們來看看一個 PDF 檔案的運作。 當使用者選擇單一報告時，只需將其轉為 PDF 並提供下載連結即可。

```
try {
console.log(`[INFO] generating the report...`);
await pdf.createPdf(`./public/documents/raw/${reports}`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

此程式碼會產生報告，並與客戶分享下載網址。 以下是輸出網頁：

![客戶下載畫面截圖](assets/report_5.png)

以下是輸出的 PDF：

![通用報告截圖](assets/report_6.png)

客戶可選擇多個檔案來產生合併報告。 當客戶選擇多份文件時，你會執行兩個操作：第一個為每個文件建立部分 PDF，第二個將它們合併成單一的 PDF 報告。

```
async function combinePdf(pdfs, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials),
operation = adobe.CombineFiles.Operation.createNew();
// Pass the PDF content as input (stream)
for (let pdf of pdfs) {
const source = adobe.FileRef.createFromLocalFile(pdf);
operation.addInput(source);
}
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

此方法可在 src/helpers/pdf.js 檔案中取得，並作為模組匯出的一部分公開。

```
try {
console.log(`[INFO] creating a batch report...`);
// Create a batch report and send it back
let partials = [];
for (let index in reports) {
const name = `partial-${index}-${reports[index]}`;
await pdf.createPdf(`./public/documents/raw/${reports[index]}`, `./public/documents/processed/${name}`);
partials.push(`./public/documents/processed/${name.replace('docx', 'pdf').replace('xlsx', 'pdf')}`);
}
await pdf.combinePdf(partials, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the combined report...`);
res.status(200).render("download", { page: 'reports', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

此程式碼會產生多個輸入文件的彙整報告。 唯一新增的功能是將 `combinePdf` PDF 檔案路徑名稱列表取回單一輸出 PDF。

現在，您的社群媒體儀表板客戶可以從帳號中選擇相關報告，並下載成一份方便的 PDF。 這個儀表板讓他們能以通用易開啟的格式，向管理層及其他利害關係人展示活動的成效，包含數據、表格和圖表。

## 後續步驟

這份實作教學示範如何使用 PDF 服務 API 幫助客戶下載相關報告，並以易於分享的 PDF 格式。 你創建了一個Node.js應用程式，展示 PDF 服務 API 在 PDF 報告與閱讀服務上的強大功能。 該應用程式示範了客戶如何下載單一報告文件，或將多個文件合併成一份 PDF 報告。

這款由 Adobe 驅動的應用程式幫助你的 [社群媒體儀表板](https://developer.adobe.com/document-services/use-cases/content-publishing/on-demand-report-creation) 客戶取得並分享他們所需的報告，不用擔心收件人是否都安裝了 Microsoft Office 或其他軟體。 你也可以在自己的應用程式中使用相同的技巧，幫助使用者查看、合併和下載文件。 或者，您也可以參考 Adobe 其他多種 API 來新增與追蹤簽名等更多功能。

要開始，請申請你的免費 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 帳號，然後為員工和客戶打造引人入勝的報告體驗。 免費使用帳號六個月，隨著 [行銷擴展，按需](https://developer.adobe.com/document-services/pricing/main) 付費，每筆文件交易只需 \$0.05。
