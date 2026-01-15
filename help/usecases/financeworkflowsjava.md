---
title: 在Java中管理財務文檔工作流
description: '[!DNL Adobe Acrobat Services]提供了處理和提取PDF財務文檔資料所需的所有工具、服務和功能'
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-7482
thumbnail: KT-7482.jpg
exl-id: 3bdc2610-d497-4a54-afc0-8b8baa234960
source-git-commit: ba73105ecf0bd27b7445ec4388fc4009eec273b8
workflow-type: tm+mt
source-wordcount: '1204'
ht-degree: 0%

---

# 在Java中管理財務文檔工作流

![使用案例英雄橫幅](assets/UseCaseFinancialHero.jpg)

金融業廣泛使用PDF檔案來交換資料，因為它有助於維護文檔格式、設計和結構。 這種穩健的形式使金融分析師和顧問能夠幫助他們的客戶做出明智的決策。

但是，PDF格式對處理和自動化可能具有挑戰性，尤其是當組合多個資料源時 — 這是金融業中的常見使用情形。 構建一個處理PDF文檔的定製解決方案是一個選項，但不需要在軟體和基礎架構方面投入太多的時間和資金。 [!DNL Adobe Acrobat Services]提供了處理和提取PDF文檔資料所需的所有工具、服務和功能。

## 你能學到的

在本操作教程中，瞭解如何為[!DNL Adobe Acrobat Services]應用程式使用[!DNL Java Spring Boot]個API。 您可以構建一個模型視圖控制器(MVC)應用程式，該應用程式從PDF文檔中提取內容，將內容轉換為其他資料格式（如Excel），合併多個PDF，並且密碼保護資源。 本教程介紹如何使用PDF[Adobe嵌入API](https://developer.adobe.com/document-services/apis/pdf-embed)處理PDF文檔並在您的網站上顯示它們。

## 相關API和資源

* [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [項目示例](https://github.com/adobe/pdftools-java-sdk-samples)

## 設定

[!DNL Adobe Acrobat Services]使用身份驗證系統控制資源訪問。 要訪問服務，您必須從Adobe中為組織或應用程式請求API密鑰。 如果您有API密鑰，請繼續下一節。 若要建立新API密鑰，請訪問[站點中的](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)入門[!DNL Acrobat Services]。 您可以使用免費試用程式建立密鑰，該試用程式提供1,000個文檔事務處理，最多可使用6個月。

要繼續學習本教程，您需要兩組API密鑰：

* Adobe PDF服務 — 用於處理PDF文檔

* Adobe PDF嵌入式API

建立憑據後，將PDF服務API憑據和私鑰複製到資源部分內的[!DNL Spring Boot]應用程式。 瞭解有關[網站上](https://developer.adobe.com/document-services/docs/overview/pdf-services-api)Maven和Gradle庫及依賴項[!DNL Adobe Acrobat Services]的詳細資訊。 確保在繼續之前設定所有必需的包和庫。

![PDF服務API憑據的目錄位置螢幕快照](assets/FAWJ_1.png)

要配置日誌記錄服務，請訪問[Adobe文檔](https://developer.adobe.com/document-services/docs/overview/pdf-services-api)並滾動到「日誌記錄」部分。

>[!NOTE]
>
> 在生產環境中，不要在版本控制中保存私鑰。 始終使用密鑰保管庫或密鑰注入服務來防止未經授權使用憑據。

現在已配置[!DNL Spring Boot]應用程式，您可以繼續處理PDF並為客戶生成報告。

## 提交報表資料

要使用Adobe PDF服務API，請首先設定一個`ExecutionContext`，該佔用您提供的憑據。 由於您在應用程式中具有憑據，因此您可以從檔案中讀取這些憑據，然後按如下方式建立上下文：

```
Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
    .fromFile(AUTH_FILE_PATH)
    .build();

ExecutionContext executionContext = ExecutionContext.create(credentials);
```

接下來，獲取上下文以處理PDF文檔。 以下是您可以執行的操作：

* 將PDF文檔（轉換為Excel、Word或圖形類型）

* 建立PDF文檔(從HTML、Excel、Word等)

* 合併多個PDF文檔

* Protect並取消對PDF文檔的保護（您必須具有密碼）

* 優化PDF文檔以在網路上交付

所有這些示例都可在[GitHub示例](https://github.com/adobe/pdfservices-java-sdk-samples/tree/master/src/main/java/com/adobe/pdfservices/operation/samples)儲存庫中獲得。

接下來，在[!DNL Spring Boot]中，您可以使用字串路徑或正在上載檔案的流獲取檔案。 必須初始化您執行的每個操作，並且必須設定輸入檔案路徑。 在本教程中，您使用[Blackrock](https://www.blackrock.com/us/individual/products/investment-funds)提供的可公開使用的PDF報告。 您可以使用任何其它來源，包括您自己的報表。

從檔案捕獲FileRef對象開始。 為簡單起見，請按字串路徑關注檔案。 下面，您將建立一個操作，將路徑中的檔案從PDF轉換為Excel:

```
ExecutionContext executionContext = ExecutionContext.create(credentials);
ExportPDFOperation exportOperation = ExportPDFOperation.createNew(ExportPDFTargetFormat.XLSX);

// Create the input source
FileRef inputPdf = FileRef.createFromLocalFile(INPUT_PDF);
exportOperation.setInput(inputPdf);
```

在此步驟之後，您的程式已準備好在PDF上運行第一個操作。 然後，執行該操作並在Excel工作表中獲取結果：

```
try {
    FileRef output = exportOperation.execute(executionContext);
    output.saveAs(OUTPUT_EXCEL);
} catch (ServiceApiException e) {
    e.printStackTrace();
}
```

此方案只處理一個PDF檔案。 您也可以從多個PDF檔案開始，並將它們合併為單個檔案。 在財務資料報告中使用多個檔案是常見的，因為您必須處理來自多個來源的資金以提供全面的報告。

## 正在生成報告

[!DNL Adobe Acrobat Services]不支援將Excel文檔從開箱中處理，但您仍然可以使用社區框架和庫來處理內容。

例如，您可以使用[Apache POI](https://poi.apache.org/)在[!DNL Java Spring Boot]應用中處理Excel(或其他Microsoft文檔)，或者可以在Excel檔案上執行其他手動或自動任務。

在此示例中，從PDF文檔開始，提取三個基金的淨資產值，並在表中顯示它們。 您還可以根據您的需求和可用資料提取其它資訊，如圖表和表。 您甚至可以從其他源中導入資料。

生成報告後 — 在本示例中，以Excel格式 — 您可以使用Adobe PDF服務操作將報告轉換回PDF文檔並對其進行保護。

要將報表從Excel格式轉換為PDF文檔，請執行以下操作：

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
> 為防止每次請求時都必須重新建立對象，請使用Spring的依賴關係注入來注入`ExecutionContext`對象。

此代碼從Excel格式的報表生成PDF文檔。

在將此PDF交付給您的客戶之前，您可以使用密碼保護它。 建立另一個為您處理此保護的操作，即ProtectPDFOperation，然後使用ProtectPDFOptions將密碼添加到文檔。

```
ProtectPDFOptions options = ProtectPDFOptions.passwordProtectOptionsBuilder()
                    .setUserPassword("p@55w0rd")
                    .setEncryptionAlgorithm(EncryptionAlgorithm.AES_256)
                    .build();
ProtectPDFOperation operation = ProtectPDFOperation.createNew(options);
```

接下來，指定輸入並執行操作。 生成的檔案應具有密碼以防止未經授權的訪問。

## 顯示報告

現在生成PDF報告後，您可以使用Adobe PDF嵌入API在網站上顯示報告。 此JavaScript API使Web開發人員能夠在Web瀏覽器內以本機方式載入和呈現PDF文檔。

>[!NOTE]
>
> 此時您需要第二個憑據令牌，即客戶端ID。

在[!DNL Spring Boot]應用程式中，添加以下要呈現HTML報告的PDF段：

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

此指令碼載入PDF文檔，並使查看者能夠對文檔進行注釋和注釋。 下面是此嵌入API的視圖，如Firefox所示：

![Firefox中PDF文檔的螢幕快照](assets/FAWJ_2.png)

PDF嵌入API提供了預覽PDF和為報告添加批注所需的所有工具。

## 後續步驟

本實踐教程探索了[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage/)個API，並討論了如何使用這些服務處理PDF資料和生成財務決策報告。 它演示了如何將API整合到系統中，以[!DNL Java Spring Boot]作為示例框架，以顯示快速處理PDF文檔是多麼容易。

瀏覽[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage/)並查看Adobe PDF服務可為您的業務做些什麼。 要瞭解SDK中可用的更多功能，請參考[GitHub儲存庫](https://github.com/adobe/pdftools-java-sdk-samples)以獲取示例，並瞭解[PDF嵌入API](https://developer.adobe.com/document-services/apis/pdf-embed)如何幫助您快速顯示應用程式內部的PDF。

要輕鬆組合和操作文檔，為財務客戶建立有用的PDF報告，請立即註冊免費[Adobe開發人員帳戶](https://developer.adobe.com/document-services/homepage/)。
