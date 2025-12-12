---
title: 員工入職現代化
description: 瞭解如何使用 [!DNL Adobe Acrobat Services] API使員工入職現代化
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-10203
thumbnail: KT-10203.jpg
exl-id: 0186b3ee-4915-4edd-8c05-1cbf65648239
source-git-commit: bd53d86abb0e5f9ee302c39e07c00101e5a1f8ed
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 0%

---

# 員工入職現代化

![使用案例英雄橫幅](assets/usecaseemployeeonboardinghero.jpg)

在大型組織中，員工入職過程可能是大型且緩慢的。 通常，定制文檔和模板材料是混合的，必須由新員工提供並簽名。 這種定制和模板材料的組合需要多個步驟 — 從參與此過程的人員那裡抽出寶貴的時間。 [!DNL Adobe Acrobat Services]和Acrobat Sign可以使此方法現代化並實現自動化，從而騰出您的人力資源部門來處理更重要的任務。 讓我們看看這是如何實現的。

## [!DNL Adobe Acrobat Services]是什麼？

[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage)是一組與使用文檔(而不只是PDF)相關的API。 總的來說，這套服務分為三大類：

* 首先是[PDF服務](https://developer.adobe.com/document-services/apis/pdf-services/)工具集。 這些是用於處理PDF和其他文檔的&quot;實用&quot;方法。 這些服務包括轉換到PDF和從PDF轉換、執行OCR和優化、合併和拆分等。 這是文檔處理功能的工具箱。
* [PDF提取API](https://developer.adobe.com/document-services/apis/pdf-extract/)使用功能強大的AI/ML技術來分析PDF並返回有關內容的驚人詳細資訊。 這包括文本、樣式和位置資訊，還可以返回CSV/XLS格式的表格資料以及檢索影像。
* 最後，[文檔生成API](https://developer.adobe.com/document-services/apis/doc-generation/)允許開發人員將MicrosoftWord用作「模板」，與其資料（來自任何來源）混合，並生成動態個性化文檔(PDF和Word)。

開發人員可以[註冊](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html)，並試用所有這些服務，免費試用。 [!DNL Acrobat Services]平台使用基於REST的API，但也支援用於Node、Java、.NET和Python的SDK（此時僅提取）。

雖然不是API，但開發人員還可以使用免費的[PDF嵌入API](https://developer.adobe.com/document-services/apis/pdf-embed/)，它為您的網頁提供了一致且靈活的文檔查看體驗。

## 什麼是Acrobat Sign?

[Acrobat Sign](https://www.adobe.com/acrobat/business/sign.html)是電子簽名服務的世界領先者。 您可以使用各種不同的工作流（包括多個簽名）發送文檔進行簽名。 Acrobat Sign還支援需要簽名和其他資訊的工作流。 所有這些功能都由功能強大的控制板支援，並具有靈活的創作系統。

與[!DNL Acrobat Services]一樣，Acrobat Sign有[免費試用](https://www.adobe.com/acrobat/business/sign.html#sign_free_trial)，開發人員可通過儀表板和易於使用的基於REST的API測試簽名過程。

## 單機場景

讓我們考慮一個現實場景，演示Adobe的服務如何能起到作用。 當新員工加入公司時，他們需要根據自己的角色定制資訊。 此外，他們還需要全公司的材料。 最後，他們必須通過簽署檔案來證明對公司政策的接受。 讓我們將其分解為具體步驟：

* 首先，需要按姓名來歡迎新員工的定製封面信。 信中應包含有關員工姓名、角色、薪金和地點的資訊。
* 自定義信函必須與包含公司範圍基本資訊（考慮各種人力資源政策、好處等）的PDF組合在一起。
* 必須包括要求員工簽名和日期的最終文檔。
* 以上所有內容都應作為一份文檔呈現，併發送給員工進行簽名。

我們來詳細說說如何做。

## 生成動態文檔

Adobe的[文檔生成](https://developer.adobe.com/document-services/apis/doc-generation/) API允許開發人員使用MicrosoftWord和簡單模板語言建立動態文檔，作為生成PDF和Word文檔的基礎。 這是一個如何運作的例子。

讓我們從具有硬編碼值的Word文檔開始。 文檔可以按您的任何方式設定樣式，包括圖形、表格等。 這是初始文檔。

![初始文檔的螢幕快照](assets/onboarding_1.png)

文檔生成通過向Word文檔添加「標籤」來工作，該文檔將替換為您的資料。 雖然可以手動輸入這些令牌，但有[MicrosoftWord載入項](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin/)，使此操作更容易執行。 開啟它為作者提供了一個工具，可以定義可在文檔中使用的標籤或資料集。

![文檔標誌器的螢幕快照](assets/onboarding_2.png)

您可以從本地檔案上載JSON資訊，以JSON文本複製，或選擇繼續處理初始資料。 這樣，您就可以根據您的特定需求以臨時方式定義標籤。 在此示例中，只需要名稱、角色、薪金和位置的標籤。 這是使用&#x200B;**建立標籤**&#x200B;按鈕完成的：

![定義標籤的螢幕快照](assets/onboarding_3.png)

定義第一個標籤後，您可以繼續根據需要定義任意數量的標籤：

![已定義標籤的螢幕快照](assets/onboarding_4.png)

在定義標籤後，選擇文檔中的文本，並在適當時用標籤替換它。 在本示例中，為名稱、角色和薪金添加標籤。

![標籤螢幕快照](assets/onboarding_5.png)

文檔生成不僅支援簡單標籤，還支援邏輯表達式。 檔案第二段的案文只適用於路易斯安那州的人。 通過進入「文檔標誌器」的「高級」頁籤並定義條件，可以添加條件表達式。 下面是如何定義簡單等式條件，但請注意，數值比較和其他比較類型也受支援。

![條件螢幕快照](assets/onboarding_6.png)

然後，可以在段落中插入並包括以下內容：

![文檔中條件的螢幕快照](assets/onboarding_7.png)

若要測試此操作的方式，請選擇&#x200B;**生成文檔**。 你第一次這樣做時，必須用Adobe ID登錄。 登錄後，將顯示可手動編輯的預設JSON。

![資料截圖](assets/onboarding_8.png)

生成PDF，然後可以查看或下載。

![生成的PDF的螢幕快照](assets/onboarding_9.png)

儘管文檔標誌器允許您快速設計和測試，但您可以使用其中一個SDK來自動化此過程。 雖然實際代碼因特定需要而不同，但以下是此代碼在Node.js中的顯示方式示例：

```js
 const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');

const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Data would be dynamic...
let data = {
    "name":"Raymond Camden",
    "role":"Lead Developer",
    "salary":9000,
    "location":"Louisiana"
}

// Create an ExecutionContext using credentials.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance.
const documentMerge = PDFServicesSdk.DocumentMerge,
    documentMergeOptions = documentMerge.options,
    options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance.
const documentMergeOperation = documentMerge.Operation.createNew(options);

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile('documentMergeTemplate.docx');
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
    .then(result => result.saveAsFile('documentOutput.pdf'))
    .catch(err => {
        if(err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

簡而言之，代碼設定憑據、建立操作對象和設定輸入和選項，然後調用該操作。 最後，將結果作為PDF保存。 （結果也可以作為Word輸出。）

「文檔生成」支援更複雜的使用情形，包括具有完全動態的表和影像的能力。 有關詳細資訊，請參閱[文檔](https://developer.adobe.com/document-services/docs/overview/document-generation-api/)。

## 執行PDF操作

[PDF服務API](https://developer.adobe.com/document-services/apis/pdf-services/)提供了一組大量「實用程式」操作，用於處理PDF。 這些操作包括：

* 從Office文檔建立PDF
* 將PDF導出到Office文檔
* 組合和拆分PDF
* 將OCR應用於PDF
* 設定、刪除和修改對PDF的保護
* 刪除、插入、重新排序和旋轉頁面
* 通過壓縮或線性化優化PDF
* 獲取PDF屬性

對於此方案，文檔生成調用的結果必須與標準PDF合併。 對於SDK，此操作相當簡單。 以下是Node.js中的示例：

```js
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
 
// Initial setup, create credentials instance.
const credentials = PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();
 
// Create an ExecutionContext using credentials and create a new operation instance.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials),
    combineFilesOperation = PDFServicesSdk.CombineFiles.Operation.createNew();
 
// Set operation input from a source file.
const combineSource1 = PDFServicesSdk.FileRef.createFromLocalFile('documentOutput.pdf'),
      combineSource2 = PDFServicesSdk.FileRef.createFromLocalFile('standardCorporate.pdf');

combineFilesOperation.addInput(combineSource1);
combineFilesOperation.addInput(combineSource2);
 
// Execute the operation and Save the result to the specified location.
combineFilesOperation.execute(executionContext)
    .then(result => result.saveAsFile('combineFilesOutput.pdf'))
    .catch(err => {
        if (err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

此代碼將獲取兩個PDF，合併它們，並將結果保存到新PDF中。 簡單易行！ 請參閱[docs](https://developer.adobe.com/document-services/docs/overview/pdf-services-api/)以瞭解可以執行的操作示例。

## 簽名過程

在入職流程的最後一站，員工必須簽署協定，聲明他們已閱讀並同意中定義的所有策略。 [Acrobat Sign](https://www.adobe.com/acrobat/business/sign.html)支援許多不同的工作流和整合，包括通過[API](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html)實現的自動工作流。 總的來說，方案的最後部分可以完成如下：

首先，設計包含需要簽名的表單的文檔。 有多種方法可以做到這一點，包括在Adobe Sign用戶儀表板中設計的可視化。 另一個選項是使用Document Generation Word載入項為您插入標籤。 此示例請求籤名和日期。

![帶有標籤的文檔螢幕快照](assets/onboarding_10.png)

此文檔可以另存為PDF，並使用上述相同方法，與所有文檔聯合在一起。 此過程將建立一個內聚包，其中包含個性化問候語、標準公司文檔和適合簽名的最終頁面。

該模板可以上載到Acrobat Sign儀表板，然後用於新協定。 使用REST API，可以將此文檔發送給潛在員工以請求其簽名。

![已簽名文檔的螢幕截圖](assets/onboarding_11.png)

## 親身體驗

本文所描述的一切都可以立即進行測試。 [!DNL Adobe Acrobat Services] API [免費試用](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html)當前在六個月期間為您提供了1,000個免費請求。 Acrobat Sign的[免費試用](https://www.adobe.com/acrobat/business/sign.html#sign_free_trial)允許您發送加水印的協定以用於測試目的。

有問題嗎？[支援論壇](https://community.adobe.com/t5/acrobat-services-api/ct-p/ct-Document-Cloud-SDK)每天都由Adobe開發人員和支援人員監視。 最後，為了獲得更多靈感，請務必收看下一集[回形針](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF)。 定期與新聞、演示和客戶談話進行現場會議。

