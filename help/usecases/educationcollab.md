---
title: 學生 — 教師協作
description: 瞭解如何建立線上學習平台，使教師和學生能夠輕鬆地在PDF中共用資源
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8091
thumbnail: KT-8091.jpg
exl-id: 570a635c-e539-4afc-a475-ecf576415217
source-git-commit: ba73105ecf0bd27b7445ec4388fc4009eec273b8
workflow-type: tm+mt
source-wordcount: '1385'
ht-degree: 0%

---

# 學生 — 教師協作

![使用案例英雄橫幅](assets/UseCaseStudentHero.jpg)

教育機構使用PDF檔案與學生共用學習材料。 PDF為教師提供可互換的文檔格式。

將[Adobe PDF服務API](https://developer.adobe.com/document-services/apis/pdf-services)和[Adobe PDF嵌入API](https://developer.adobe.com/document-services/apis/pdf-embed)整合到應用中，為教師和學生提供了可在其上進行教學和學習的單一平台。 例如，您的應用可以讓學生就他們的作業和報告卡提出問題，並在組作業上進行協作。

Node.js應用程式有正式的SDK可訪問PDF服務API。 這使您能夠將MicrosoftWord或MicrosoftExcel等文檔轉換為
PDF。 此外，您還可以執行更高級的操作，如組合多個報告、重新排列頁面和保護PDF。 有關詳細資訊，請查看[產品文檔](https://developer.adobe.com/document-services/homepage/)。

## 你能學到的

在本實踐教程中，學習如何建立一個線上學習平台，該平台[使教師和學生能夠輕鬆地在PDF中共用資源](https://developer.adobe.com/document-services/use-cases/collaboration/student-teacher-collaboration)。 本教程使用使用Node.js JavaScript運行時(Node.js)和PDF服務建立的[學習門戶](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers)。

學習門戶具有以下功能：

* 使教師能夠上載資源

* 使學生能夠選擇多個文檔以轉換為PDF

* 啟用文檔到PDF的轉換

* 在Web瀏覽器中為學生提供PDF預覽，並允許他們在不使用其他軟體的情況下對文檔進行批注

* 使學生能夠留下注釋並將其下載到其電腦

瞭解[!DNL Adobe Acrobat Services]如何為學生提供豐富的PDF體驗。 [!DNL Acrobat Services]個API可無縫整合到您的現有應用程式中，因此學生可以上傳、轉換和查看檔案，然後生成和保存注釋 — 所有這些都在您當前的設定中。

## 相關API和資源

* [PDF嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [項目代碼](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers)

## 正在將資源上載到學習門戶

在學習門戶的教師部分，教師可以上傳作業和測試等文檔。 文檔可以採用任何格式，如MicrosoftWord、MicrosoftExcel、HTML、各種影像格式等。

![學習門戶的教師節截圖](assets/edu_1.png)

上載的文檔被儲存並在學生開啟其網頁時呈現給他們。

要瞭解應用程式如何上載檔案，請參閱[項目代碼](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers)。

## 將文檔轉換為PDF

學生可以將任何類型的單個或多個文檔轉換為PDF，如MicrosoftWord、Excel和PowerPoint，以及其他常用文本和影像檔案類型。 學習門戶使用PDF服務執行檔案到PDF的轉換。

要建立自己的學習門戶，必須首先建立自己的憑據。 [註冊](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)至
使用PDF服務API，可免費處理6個月和最多1,000個文檔事務。 之後，當類增加其分配時，[按單價付款](https://developer.adobe.com/document-services/pricing/main)的每個文檔交易僅為\$0.05。

當學生從儀表板中選擇文檔時，他們會看到以下內容：

![學習門戶的學生部分螢幕快照](assets/edu_2.png)

學生只需選擇要轉換的文檔，然後按一下&#x200B;**獲取報告**。

學習門戶將文檔轉換為PDF，並顯示報告頁面以及PDF檔案的預覽。

下面是此步驟的示例代碼：

```
async function createPdf(rawFile, outputPdf) {
    try {
            // configurations
            const credentials =  adobe.Credentials
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

示例代碼調用Express路由處理程式內的`createPdf`方法以生成PDF。

要瞭解如何調用此方法，請參閱[項目代碼](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers/blob/master/src/helpers/pdf.js)。

## 預覽學習資源

用戶介面使用PDF嵌入API在Web瀏覽器中呈現PDF。 此API可免費使用。

PDF嵌入API使用的憑據與PDF服務API不同，因此您必須[建立憑據](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
才能使用它。 然後，您可以使用「嵌入PDF」完全自由。

確保在令牌中輸入正確的網站URL。 否則，您可能無法使用令牌呈現PDF。

用戶介面使用[Handlebars](https://handlebarsjs.com/)模板語言。 它在Web瀏覽器中顯示PDF。

下面是此步驟的代碼：

```
<div id="adobe-dc-view" style="height: 750px; width: 700px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function () {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-credentials-here>", divId: "adobe-dc-view" });
        adobeDCView.previewFile(
            {
                content: {
                    location: { url: "<file-url>" }
                },
                    metaData: { fileName: "<file-name>" }
            },
           );
    });
</script>
 
<p>Material has been generated, <a href="/students/download/{{filename}}" target="_blank">click here</a> to download it.
</p>
```

此代碼顯示PDF輸出和下載PDF報告的連結，如下面的螢幕捕獲所示：

![學生PDF預覽螢幕截圖](assets/edu_3.png)

學生應該能夠下載報告或在此處處理材料。

## 注釋PDF文檔

學習平台應支援基本的PDF注釋、注釋和討論。 PDF嵌入API提供了所有這些功能。 它使用`showAnnotationTools`激活批注支援，使教師和學生能夠對文檔發表評論，並將評語作為PDF的一部分存檔。

若要在PDF文檔中啟用批注，您只需將參數`showAnnotationTools` :true傳遞給`previewFile`方法。 這將在PDF預覽器中顯示注釋工具。 從預覽右上角的三點菜單訪問此工具。

![PDF中注釋工具的螢幕快照](assets/edu_4.png)

在教師上傳的文檔中，學生可以突出顯示文本、添加註釋等。

![在PDF中添加註釋的螢幕快照](assets/edu_5.png)

在上面的螢幕捕獲中，用戶被標籤為「來賓」，但您可以為用戶（如學生和教師）配置配置檔案。

當學生應用批注時，PDF嵌入API將在頂部橫幅上顯示&#x200B;**保存**&#x200B;按鈕。 保存會將注釋添加到檔案。 嘗試按一下&#x200B;**保存**，查看檔案如何使用報表中嵌入的注釋進行保存。

學生可以使用注釋來提出問題或分享他們對學習材料的評論。

## 跟蹤文檔使用

教師和學校瞭解學生如何使用線上平台非常重要。 這有助於教師為學生提供資源，幫助他們更好地完成作業。 PDF嵌入API與分析整合，您可以使用分析來衡量發生的所有事件，如用戶開啟、閱讀和關閉文檔時。 使用PDF服務API，教師還可以禁用打印、下載和檔案修改，以幫助維護學術完整性。

如果您有[Adobe Analytics](https://developer.adobe.com/analytics-apis/docs/2.0/)許可證，則可以使用其[現成整合](https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfembed/controlpdfexperience#adobe-analytics)。 否則，請使用回叫將您的PDF服務與其他分析提供程式整合，如[Google](https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfembed/controlpdfexperience#google-analytics)。

要啟用文檔事件的度量，請使用帶有AdobeDC視圖實例的`registerCallback`方法附加事件處理程式。 您可以在控制台上顯示基本度量，如開啟文檔或讀取頁面。 您還可以將度量保存到日誌中，或將其發佈到其他分析儲存中。

以下是用於附加事件處理程式的示例代碼：

```
adobeDCView.registerCallback(
    AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
    function(event) {
           console.log(event);
    },
    {
           enablePDFAnalytics: true
    }
);
```

老師可以看到多少學生看過這個作業，有多少學生翻閱了他們所有的筆記，還有其他有價值的細節。

下面是Web瀏覽器控制台的螢幕捕獲：

![Web瀏覽器控制台螢幕快照](assets/edu_6.png)

此螢幕捕獲顯示學生開啟了分配檔案，他們閱讀了第一頁 — 他們要麼沒有滾動到其他頁面，要麼文檔只有一頁 — 然後他們下載了該檔案。 您可以收集這些指標來執行分析和研究學生的行為。

此外，[Adobe Analytics](https://business.adobe.com/products/adobe-analytics.html)與PDF嵌入API整合，因此，如果您有對Adobe Analytics套件的訂閱，則可以在訂閱中發佈您的度量。 要在Adobe Analytics發佈度量，您只需將套件ID傳遞給PDFEmbed API建構子。 (請注意，您必須使用PDF嵌入API憑據，而不是PDF服務API憑據)。

下面是示例代碼，說明如何將套件ID傳遞給PDFEmbed API建構子：

```
var adobeDCView = new AdobeDC.View({
    clientId: "<your-adobe-dc-credential>",
    divId: "<#element>"
    reportSuiteId: <your-id-here>,
}); 
```

## 後續步驟

本實踐教程介紹了如何使用PDF服務API和PDF嵌入API建立學習門戶，從而幫助學生和教師之間進行有效的[協作](https://developer.adobe.com/document-services/use-cases/collaboration/student-teacher-collaboration)。 使用此門戶，教師可以以任何格式上傳學習資料，並使用PDF服務API將其轉換為PDF。 然後，學生可以使用PDF嵌入API預覽這些PDF。

現在，您知道如何對PDF報告進行批注、歸檔批注並跟蹤PDF報告的使用，因此可以開始在自己的項目中實施這些解決方案。

您可以使用[!DNL Adobe Acrobat Services]個API在您的網站上建立用戶友好的互動式PDF體驗。 使用Adobe PDF服務API免費6個月，然後只需[按需付費](https://developer.adobe.com/document-services/pricing/main)&#x200B;(通過AWS或直接協定)，每個文檔交易費用僅為\$0.05。 使用無時間限制的「Adobe PDF嵌入自由」。 建立免費帳戶以立即開始[&#128279;](https://www.adobe.com/go/dcsdks_credentials)。
