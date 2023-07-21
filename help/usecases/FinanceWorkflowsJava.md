---
title: 在 JAVA 中管理金融檔工作流程
description: 「 [!DNL Adobe Acrobat Services] 提供處理和擷取 PDF 財務檔資料的所有必要工具、服務和功能」。
type: Tutorial
role: Developer
level: Intermediate
thumbnail: KT-7482.jpg
jria: KT-7482
exl-id: 3bdc2610-d497-4a54-afc0-8b8baa234960
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# 在 JAVA 中管理財務檔工作流程

![使用案例主打橫幅](assets/UseCaseFinancialHero.jpg)

金融業會大量使用 PDF 檔案來交換資料，因為這有助於維持檔案格式、設計和結構。 這種強大格式可讓財務分析師和顧問協助客戶制定明智的決策。

不過，PDF 格式對於處理和自動化可能困難重重，尤其是合併多個資料來源時──在金融業是常見的使用案例。 建立處理 PDF 檔的自訂解決方案是一個選擇，但是無需在軟體和基礎架構上投入太多時間和金錢。 [!DNL Adobe Acrobat Services] 提供處理和擷取 PDF 檔資料的所有必要工具、服務和功能。

## 您可以學習哪些內容

在此實作教學課程中，瞭解如何將 [!DNL Adobe Acrobat Services] API 用於 [!DNL Java Spring Boot] 應用程式。 您可以建立模型檢視控制器 （MVC） 應用程式，從 PDF 檔擷取內容、將其轉換為 Excel 等其他資料格式、合併多個 PDF，以及使用密碼保護資源。 本教學課程說明如何使用 [ Adobe PDF 內嵌 ](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) API，在您的網站上處理和顯示 PDF 檔。

## 相關 API 和資源

* [PDF 服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF 嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [專案範例](https://github.com/adobe/pdftools-java-sdk-samples)

## 設定

[!DNL Adobe Acrobat Services] 使用驗證系統控制資源存取。 若要存取服務，您必須向組織或應用程式Adobe索取API金鑰。 如果您有API鍵，請繼續執行下一節。 若要建立新的API鍵，請造訪 [ 網站中的快速入 ](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 門 [!DNL Acrobat Services] 。 您可以使用免費試用版建立金鑰，其中提供最多 6 個月的 1，000 筆檔交易額度。

若要依照本教學課程操作，您需要兩組API鍵：

* Adobe PDF服務 — 用於處理 PDF 檔

* Adobe PDF嵌入API

建立憑證後，複製 PDF Services API認證，並將私密金鑰複製到資源區段內的 [!DNL Spring Boot] 應用程式。 請在網站上進一步瞭解 [ Maven 和 Gradle 資料庫的 [!DNL Adobe Acrobat Services] 相關性與相依性 ](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) 。請務必先設定所有必要的套件和資料庫，再繼續進行。

![PDF Services 目錄位置的螢幕擷圖API認證](assets/FAWJ_1.png)

若要設定記錄服務，請造訪 [ Adobe檔 ](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=services) 並捲動至「記錄」區段。

>[!NOTE]
>
> 在生產環境中，請勿將私密金鑰儲存在版本控制中。 務必使用密碼保存庫或金鑰插入服務，以防止未經授權使用認證。

在設定應用程式 [!DNL Spring Boot] 後，您就可以繼續處理 PDF 並產生客戶報告。

## 提交報告資料

若要使用「Adobe PDF服務」API，請先設定 `ExecutionContext` 耗用您提供憑證的憑證。 由於您的應用程式內已有認證，因此您可以從檔案中讀取憑證並建立上下文，如下所示：

```
Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
    .fromFile(AUTH_FILE_PATH)
    .build();

ExecutionContext executionContext = ExecutionContext.create(credentials);
```

接下來，取得處理 PDF 檔的內容。 您可以執行以下動作：

* 轉換 PDF 檔 （為 Excel、Word 或圖形類型）

* 建立 PDF 檔 （來自 HTML、Excel、Word 等）

* 合併多個 PDF 檔

* 保護和取消保護 PDF 檔 （您必須有密碼）

* 優化在網路上傳送的 PDF 檔

這些範例都可在 [ GitHub 範例 ](https://github.com/adobe/pdfservices-java-sdk-samples/tree/master/src/main/java/com/adobe/pdfservices/operation/samples) 存放庫中找到。

接下來， [!DNL Spring Boot] 您可以使用字串路徑或上傳檔案的串流取得檔案。 您執行的每個操作都必須初始化，且必須設定輸入檔案路徑。 若是此教學課程，請使用 Blackrock ](https://www.blackrock.com/us/individual/products/investment-funds) 公開提供的 PDF 報告 [ 。您可以使用任何其他來源，包括您自己的報告。

首先，從檔案擷取 [ FileRef ](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/io/FileRef.html) 物件。 若要簡化，請依字串路徑專注于檔案。 在下方，您可以建立一項操作，將路徑中的檔案從 PDF 轉換為 Excel：

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
ExportPDFOperation exportOperation = ExportPDFOperation.createNew(ExportPDFTargetFormat.XLSX);

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_PDF);
exportOperation.setInput(inputPdf);
```

在此步驟之後，您的程式就可以在 PDF 上執行第一個操作。 接下來，您將執行操作，並在 Excel 工作表中獲得結果：

```
try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_EXCEL);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

此案例僅處理一個 PDF 檔案。 您也可以從多個 PDF 檔案開始，並將其合併為單一檔案。 在財務資料包告中，使用多個檔案很常見，因為您必須處理多個來源的基金，才能提供全面的報告。

## 產生報告

[!DNL Adobe Acrobat Services] 不支援開箱即用處理 Excel 檔，但您仍然可以使用社群架構和資料庫來處理內容。

例如，您可以使用 [ Apache POI ](https://poi.apache.org/) 在應用程式中 [!DNL Java Spring Boot] 處理 Excel （或其他 Microsoft 檔），或者您也可以在 Excel 檔案上執行其他手動或自動化工作。

在本範例中，從 PDF 檔開始，您可以擷取這三隻基金的淨資產價值，並顯示在表格中。 您也可以根據您的需求和可用的資料提取其他資訊，例如圖表和表格。 您甚至可以匯入其他來源的資料。

以此範例以 Excel 格式產生報告後，您可以使用「Adobe PDF服務」操作，將報告轉換回 PDF 檔並加以保護。

若要將報告從 Excel 格式轉換為 PDF 檔，請使用下列操作：

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
CreatePDFOperation exportOperation = CreatePDFOperation.createNew();

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_EXCEL);
exportOperation.setInput(inputPdf);

try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_PDF);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

>[!TIP]
>
> 若要避免每次要求來時必須重新建立物件，請使用 Spring 的相依性插入來插入 `ExecutionContext` 物件。

此程式碼會以 Excel 格式從報告產生 PDF 檔。

在將此 PDF 傳送給客戶之前，您可以先使用密碼保護檔案。 建立另一個為您 [ 處理此保護的操作， ProtectPDFOperation ](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/ProtectPDFOperation.html) ， 然後使用 [ ProtectPDFOptions ](https://opensource.adobe.com/pdfservices-java-sdk-samples/apidocs/latest/com/adobe/pdfservices/operation/pdfops/options/protectpdf/package-summary.html) 將密碼新增至檔。

```
ProtectPDFOptions options = ProtectPDFOptions.passwordProtectOptionsBuilder()
                    .setUserPassword("p@55w0rd")
                    .setEncryptionAlgorithm(EncryptionAlgorithm.AES_256)
                    .build();
ProtectPDFOperation operation = ProtectPDFOperation.createNew(options);
```

接著，指定輸入並執行操作。 產生的檔案上應具有密碼，以防止未經授權的存取。

## 顯示報告

在產生 PDF 報告後，您可以使用「內嵌API」Adobe PDF在網站上顯示報告。 此JavaScript API可讓網頁開發人員在網頁瀏覽器內以原生方式載入和演算 PDF 檔。

>[!NOTE]
>
> 此時您需要第二個認證權杖，即用戶端 ID。

在應用程式 [!DNL Spring Boot] 中，在您要轉譯 PDF 報告的位置新增下列 HTML 片段：

```
<div id="pdf-viewer"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function()
    {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-client-id-here>", divId: "pdf-viewer" });
        adobeDCView.previewFile(
        {
            content: {
                location: {
                    url: "<your-document.pdf>"
                }
            },
            metaData: {
                fileName: "<document-name.pdf>"
            }
        });
    });
</script>
```

此腳本會載入 PDF 檔，並讓檢視者為檔加上批註和加上注釋。 以下是此「嵌入」API的視圖，如 Firefox 所示：

![Firefox 中 PDF 檔的螢幕擷圖](assets/FAWJ_2.png)

PDF 內嵌API提供了預覽 PDF 以及為報告加上批註所需的所有工具。

## 後續步驟

此實作教學課程探討了 API， [[!DNL Adobe Acrobat Services] ](https://www.adobe.io/apis/documentcloud/dcsdk/) 並討論如何使用這些服務來處理 PDF 資料並產生財務決策報告。它示範了您如何將 API 整合到您的系統中，並以 [!DNL Java Spring Boot] 範例框架為例，以展示快速處理 PDF 檔有多麼簡單。

探索 [[!DNL Adobe Acrobat Services] ](https://www.adobe.io/apis/documentcloud/dcsdk/) 並瞭解 Adobe PDF Services 能為您的業務做些什麼。若要瞭解 SDK 中可用的更多功能，請查閱 [ GitHub 儲存庫 ](https://github.com/adobe/pdftools-java-sdk-samples) 尋找範例，並探索 PDF 內嵌API ](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 如何 [ 協助您在應用程式中快速顯示 PDF。

若要輕鬆合併和操作檔，請先為您的金融客戶建立實用的 PDF 報告，首先註冊您的免費 [ Adobe開發人員帳戶 ](https://www.adobe.io/apis/documentcloud/dcsdk/) 。
