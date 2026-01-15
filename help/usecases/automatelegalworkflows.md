---
title: 自動化法律工作流
description: 瞭解如何使用條件內容自動化法律工作流
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-10202
thumbnail: KT-10202.jpg
exl-id: 2a1752b8-9641-40cc-a0af-1dce6cf49346
source-git-commit: ba73105ecf0bd27b7445ec4388fc4009eec273b8
workflow-type: tm+mt
source-wordcount: '2824'
ht-degree: 0%

---

# 自動化法律工作流

![使用案例英雄橫幅](assets/usecaseautomatelegalhero.jpg)

在理想情況下，協定條款被接受而不進行任何修改。 但是，通常，協定需要定制，然後需要法律審查。 法律審查會帶來巨大的成本，並會減慢提供協定條款的過程。 使用根據批准的語言進行更改的預定義模板，可幫助法律團隊管理並更安全地執行協定條款。

本教程使用的法律協定因州和州而異。 要解決這些變體，將建立包含條件部分的協定模板，該模板僅在滿足某些條件時才包括。 生成的文檔可以是Word文檔或PDF文檔。 您還可以學習使用Adobe PDF服務API或Acrobat Sign保護文檔的一些方法。

## 獲取憑據

首先註冊免費Adobe PDF服務憑據：

1. 在[此處](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html)導航以註冊您的憑據。
1. 使用您的Adobe ID登錄。
1. 設定憑據名稱。

   ![設定憑據名稱的螢幕快照](assets/automatelegal_1.png)

1. 選擇下載示例代碼的語言（例如Node.js）。
1. 檢查以同意&#x200B;**[!UICONTROL 開發人員條款]**。
1. 選擇&#x200B;**[!UICONTROL 建立憑據]**。
將檔案下載到您的電腦，其ZIP檔案包含用於驗證的示例檔案、pdfservices-api-credentials.json和private.key。

   ![憑據螢幕快照](assets/automatelegal_2.png)

1. 選擇&#x200B;**[!UICONTROL 獲取MicrosoftWord載入項]**&#x200B;或轉到[AppSource](https://appsource.microsoft.com/en-cy/product/office/WA200002654)進行安裝。

   >[!NOTE]
   >
   >安裝Word載入項需要您有權在Microsoft365中安裝載入項。 如果您沒有權限，請與您的Microsoft365管理員聯繫。

## 您的資料

在此方案中，將傳遞資訊以幫助生成文檔並通知是否應包括某些部分：

```
{
    "customer": {
        "name": "Home Services Company",
        "street": "123 Any Street",
        "city": "Anywhere",
        "state": "CA",
        "zip": "12345",
        "country":"USA",
        "signer": {
            "email": "johnnyechostone@gmail.com",
            "firstName": "John",
            "lastName": "Echostone"
        }
    },
    "company": {
        "name": "Projected Consultants",
        "signer": {
            "email": "maryburostone@gmail.com",
            "firstName": "Mary",
            "lastName": "Burostone"
        }
    },
    "conditions": {
        "includeGeneralTerms": true,
        "includeConsumerDiscloure": true
    }
}
```

在資料中，有關客戶、其姓名、簽名者、所處狀態等資訊。 此外，還有一些部分，用於提供有關正在生成協定的公司的資訊，以及用於包括協定某些部分的條件標誌。

## 將基本標籤添加到文檔

此方案使用條款和條件文檔，可以在[此處下載](https://github.com/benvanderberg/adobe-document-generation-samples/blob/main/Agreement/exercise/TermsAndConditions_Sample.docx?raw=true)。

![條款和條件文檔的螢幕快照](assets/automatelegal_3.png)

1. 在MicrosoftWord中開啟&#x200B;*TermsAndConditions.docx*&#x200B;示例文檔。
1. 如果安裝了[文檔生成](https://appsource.microsoft.com/en-cy/product/office/WA200002654)插件，請在功能區中選擇&#x200B;**[!UICONTROL 文檔生成]**。 如果您在功能區中未看到「文檔生成」，請按照以下說明操作。
1. 選擇&#x200B;**[!UICONTROL 開始]**。
1. 將上面寫入的JSON示例資料複製到「JSON資料」欄位。

   ![文檔和JSON資料的螢幕快照](assets/automatelegal_4.png)

導航到&#x200B;*文檔生成標籤*&#x200B;面板以將標籤放在文檔中。

## 插入公司名稱

1. 選擇要替換的文本。 在此方案中，您將替換文檔的開頭部分中的COMPANY。
1. 在&#x200B;*文檔生成標籤*&#x200B;中，搜索「名稱」。
1. 在「公司」下，選擇&#x200B;*名稱*。

   ![在文檔生成標籤中搜索名稱的螢幕快照](assets/automatelegal_5.png)

1. 選擇&#x200B;**[!UICONTROL 插入文本]**。

這將放置一個名為`{{company.name}}`的標籤，因為該標籤位於JSON中該路徑下。

```
{
    "company": {
        "name": "Projected Consultants",
        ...
    }
    ...
}
```

接下來，在「客戶」文本的開始部分重複此步驟。 重複&#x200B;**步驟1-4**，將CUSTOMER替換為「name」（客戶名稱）。 輸出應為`{{customer.name}}`，反映文本來自客戶對象的下方。

Adobe文檔生成API還允許您在頁眉和頁腳中以及簽名標題需要轉到的最後部分包含標籤。

對頁腳中的COMPANY文本和CUSTOMER文本，再次重複此過程&#x200B;**步驟1-4**。

![頁腳中添加COMPANY和CUSTOMER標籤的螢幕快照](assets/automatelegal_6.png)

最後，您需要&#x200B;**重複步驟1-4**，將簽名頁的「客戶」部分下的FIRST NAME和LAST NAME分別替換為`{{customer.signer.firstName}}`和`{{customer.signer.lastName}}`的標籤。 不要擔心標籤長度過長，並重新流到下一行，因為在生成文檔時會替換標籤。

文檔和頁腳的開頭應如下所示：

* 開始部分：

![開始部分的螢幕快照](assets/automatelegal_7.png)

* 頁腳：

![頁腳截圖](assets/automatelegal_8.png)

* 簽名頁：

![簽名頁螢幕快照](assets/automatelegal_9.png)

現在，您的標籤已放在文檔中，您已準備好預覽生成的協定。

## 預覽生成的文檔

直接在MicrosoftWord中，您可以基於示例JSON資料預覽生成的文檔。

1. 在&#x200B;*文檔生成標籤*&#x200B;中，選擇&#x200B;**[!UICONTROL 生成文檔]**。
1. 第一次可能會提示您登錄您的Adobe ID。 選擇&#x200B;**[!UICONTROL 登錄]**，並完成提示以使用您的憑據登錄。

   ![選擇「生成文檔」按鈕的螢幕快照](assets/automatelegal_10.png)

1. 選擇&#x200B;**[!UICONTROL 查看文檔]**。

   ![「查看文檔」按鈕的螢幕截圖](assets/automatelegal_11.png)

1. 將開啟瀏覽器窗口，允許您預覽文檔結果。

   ![特定於狀態的文本的螢幕快照](assets/automatelegal_12.png)

## 為每個狀態添加條件項

在下一節中，您僅根據某些輸入資料標準設定要包括的某些部分。 在示例文檔中，第4和第5節僅與特定狀態有關。 對於此方案，當客戶駐留在該狀態時，只應包括特定於州的條款。 此外，如果刪除了MicrosoftWord中的編號，則不應包括該節。 使用文檔生成API的條件內容功能來標籤此內容。

![特定於狀態的文本的螢幕快照](assets/automatelegal_13.png)

![選擇California Disclosure節的螢幕快照](assets/automatelegal_14.png)

1. 在文檔中，選擇「California Disclosure」部分和所有子項目符號。

   ![條件節標籤的螢幕快照](assets/automatelegal_15.png)

1. 在&#x200B;*[!UICONTROL 文檔生成標籤]*&#x200B;中，選擇&#x200B;**[!UICONTROL 高級]**。
1. 展開&#x200B;**[!UICONTROL 條件內容]**。
1. 在&#x200B;*[!UICONTROL 選擇記錄]*&#x200B;欄位中，搜索並選擇&#x200B;**[!UICONTROL customer.state]**。
1. 在&#x200B;*[!UICONTROL 選擇運算子]*&#x200B;欄位中，選擇&#x200B;**=**。
1. 在&#x200B;*[!UICONTROL 值]*&#x200B;欄位中，鍵入&#x200B;*CA*。
1. 選擇&#x200B;**[!UICONTROL 插入條件]**。

現在，該節將包含一些稱為條件節標籤的標籤。 添加標籤時，它可能已將條件截面標籤添加為編號行。 您可以通過在標籤前反向間隔來刪除該項，否則，它會對項目編號，就好像生成文檔時標籤不在一樣。 條件節以`{% end-section %}`標籤結束。

![條件節標籤的螢幕快照](assets/automatelegal_16.png)

**對** Washington Disclosure *節重複步驟1-7*，將&#x200B;*CA*&#x200B;值替換為&#x200B;*WA*，以表示僅當客戶的狀態為Washington時才顯示該節。

![WA](assets/automatelegal_17.png)的Conditional-section標籤的螢幕快照

## 使用條件節進行測試

條件節就位後，您可以通過選擇&#x200B;**生成文檔**&#x200B;來預覽文檔。

在生成文檔時，請注意所包含的部分僅滿足資料條件。 在下面的示例中，由於州與CA相等，因此只包括California部分。

![California Disclosure資訊螢幕截圖](assets/automatelegal_18.png)

另一個顯著變化是，後續部分「服務和軟體的使用」的編號為5。 這意味著，當華盛頓部分被省略時，編號將繼續。

![連續編號的螢幕截圖](assets/automatelegal_19.png)

要測試當客戶位於華盛頓州而不是加利福尼亞州時，模板是否正確行事，請更改模板的示例資料：

1. 在&#x200B;*文檔生成標誌符*&#x200B;中，選擇&#x200B;**[!UICONTROL 編輯輸入資料]**。

   ![文檔生成標誌的螢幕快照](assets/automatelegal_20.png)

1. 選擇&#x200B;**[!UICONTROL 編輯]**。

1. 在JSON資料中，將&#x200B;*CA*&#x200B;更改為&#x200B;*WA*。

   ![JSON資料的螢幕快照](assets/automatelegal_21.png)

1. 選擇&#x200B;**[!UICONTROL 生成標籤]**。
1. 選擇&#x200B;**[!UICONTROL 生成文檔]**&#x200B;以重新生成文檔。

請注意，該檔案只包括華盛頓州部分。

![只包含Wasington狀態節的文檔螢幕快照](assets/automatelegal_22.png)

## 添加條件句

與條件段一樣，您也可以包含滿足特定條件時包含的特定句子。 例如，加州和華盛頓的回返政策不同。

1. 在第3.1節中，選擇第一句「在華盛頓州購買時，必須在原始交易後30天內通過MAIL返回一句，以獲得全額退款。」
1. 在&#x200B;*[!UICONTROL 文檔生成標籤]*&#x200B;中，選擇&#x200B;**[!UICONTROL 高級]**。
1. 展開&#x200B;**[!UICONTROL 條件內容]**。
1. 在&#x200B;*[!UICONTROL 內容類型]*&#x200B;下，選擇&#x200B;**[!UICONTROL 短語]**。
1. 在&#x200B;*[!UICONTROL 選擇記錄]*&#x200B;欄位中，搜索並選擇&#x200B;**[!UICONTROL customer.state]**。
1. 在&#x200B;*[!UICONTROL 選擇運算子]*&#x200B;欄位中，選擇&#x200B;**=**。
1. 在&#x200B;*[!UICONTROL 值]*&#x200B;欄位中，鍵入&#x200B;*CA*。
1. 選擇&#x200B;**[!UICONTROL 插入條件]**。

雖然標籤名稱相同，但「短語」和「節」之間的主要區別是，某個短語的節不包含新行。 條件節標籤和結束節標籤必須位於同一段落中。

![短語標籤的螢幕快照](assets/automatelegal_23.png)

## 為Acrobat Sign添加標籤

Acrobat Sign允許您發送協定進行簽名或嵌入到Web體驗中，以便用戶輕鬆查看和簽名。 AdobeWord中的文檔生成標籤允許您在將文檔與Acrobat Sign一起發送之前輕鬆地對它們進行預標籤，因此簽名始終放在正確的位置。 在此方案中，有兩個簽名者需要一個位置來簽名和日期文檔。

1. 導航到客戶必須簽名的位置。
1. 將游標置於簽名所需的位置。

   ![簽名需要轉到的螢幕截圖](assets/automatelegal_24.png)

1. 在&#x200B;*[!UICONTROL 文檔生成標籤]*&#x200B;中，選擇&#x200B;**[!UICONTROL Adobe Sign]**。
1. 在&#x200B;*[!UICONTROL 指定收件人數]*&#x200B;欄位中，設定收件人數（此示例使用2）。
1. 在&#x200B;*[!UICONTROL 收件人]*&#x200B;欄位中，選擇&#x200B;**[!UICONTROL 簽名者–1]**。
1. 在&#x200B;*[!UICONTROL 欄位]*&#x200B;類型中，選擇&#x200B;**[!UICONTROL 簽名]**。
1. 選擇&#x200B;**[!UICONTROL 插入Adobe Sign文本標籤]**。

   ![在文檔生成標籤中插入Adobe Sign文本標籤的螢幕快照](assets/automatelegal_25.png)

>[!NOTE]
>
>如果&#x200B;**插入Adobe Sign文本標籤**&#x200B;按鈕似乎丟失，請向下滾動。

這將放置簽名欄位，第一個簽名者需要在該欄位簽名。

![簽名文本標籤的螢幕快照](assets/automatelegal_26.png)

接下來，為簽名時自動填充的簽名者放置一個資料欄位。

1. 將游標移到應將日期放置的位置。

   ![日期應位於的螢幕快照](assets/automatelegal_27.png)

1. 將「欄位類型」設定為「日期」。
1. 選擇&#x200B;**[!UICONTROL 插入Adobe Sign文本標籤]**。

放置的日期標籤相當長： `{{Date 3_es_:signer1:date:format(mm/dd/yyyy):font(size=Auto)}}`。 Acrobat Sign文本標籤必須保留在同一行上，這與文檔生成標籤不同。 `:format()`和`font()`參數是可選的，因此對於此方案，我們可以將標籤縮短到`{{Date 3_es_:signer1:date}}`。

重複&#x200B;*公司簽名*&#x200B;部分上面的步驟。 執行此操作時，必須將「收件人」欄位更改為&#x200B;**Signer-2**，否則所有簽名欄位都分配給同一人。

## 生成協定

您現在已標籤文檔，並準備開始使用。 在下一節中，瞭解如何使用Node.js的文檔生成API示例生成文檔。 這些樣本可以用任何語言。

開啟註冊憑據時下載的pdfservices-node-sdk-samples-master檔案。 這些檔案包括pdfservices-api-credentials.json和private.key檔案。

1. 開啟&#x200B;**[!UICONTROL 終端]**&#x200B;以使用`npm install`安裝依賴項。
1. 將示例&#x200B;*data.json*&#x200B;複製到&#x200B;*資源*&#x200B;資料夾中。
1. 將您建立的Word模板複製到&#x200B;*資源*&#x200B;資料夾中。
1. 在示例資料夾的根目錄中建立名為&#x200B;*generate-salesOrder.js*&#x200B;的新檔案。

   ```
   const PDFServicesSdk = require('@adobe/pdfservices-node-sdk').
   const fs = require('fs');
   const path = require('path');
   
   var dataFileName = path.join('resources', '<INSERT JSON FILE');
   var outputFileName = path.join('output', 'salesOrder_'+Date.now()+".pdf");
   var inputFileName = path.join('resources', '<INSERT DOCX>');
   
   //Loads credentials from the file that you created.
   const credentials =  PDFServicesSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile("pdfservices-api-credentials.json")
      .build();
   
   // Setup input data for the document merge process
   const jsonString = fs.readFileSync(dataFileName),
   jsonDataForMerge = JSON.parse(jsonString);
   
   // Create an ExecutionContext using credentials
   const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);
   
   // Create a new DocumentMerge options instance
   const documentMerge = PDFServicesSdk.DocumentMerge,
   documentMergeOptions = documentMerge.options,
   options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);
   
   // Create a new operation instance using the options instance
   const documentMergeOperation = documentMerge.Operation.createNew(options)
   
   // Set operation input document template from a source file.
   const input = PDFServicesSdk.FileRef.createFromLocalFile(inputFileName);
   documentMergeOperation.setInput(input);
   
   // Execute the operation and Save the result to the specified location.
   documentMergeOperation.execute(executionContext)
   .then(result => result.saveAsFile(outputFileName))
   .catch(err => {
      if(err instanceof PDFServicesSdk.Error.ServiceApiError
         || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
         console.log('Exception encountered while executing operation', err);
      } else {
         console.log('Exception encountered while executing operation', err);
      }
   });
   ```

1. 將`<JSON FILE>`替換為/resources中JSON檔案的名稱。
1. 將`<INSERT DOCX>`替換為DOCX檔案的名稱。
1. 要運行，請使用&#x200B;**[!UICONTROL 終端]**&#x200B;執行節點`generate-salesOrder.js`。

輸出檔案位於/output資料夾中，文檔生成正確。

可通過更改下面的行來更改格式。 如果要發送此文檔供某人在Word中編輯或供合同審核時使用，則DOCX格式很有幫助。

PDF:

```
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge,
documentMergeOptions.OutputFormat.PDF);
```

字：

```
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.DOCX);
```

您還必須將輸出檔案的名稱分別更改為.pdf或.docx，以便PDF或DOCX輸出格式：

```
var outputFileName = path.join('output', 'salesOrder_'+Date.now()+".docx");
```

## 發送協定以供簽署

[Adobe Acrobat Sign](https://www.adobe.com/acrobat/business/sign.html)允許您向一個或多個收件人發送協定，以便他們查看和簽署文檔。 除了易於使用的用戶體驗，您還可以使用REST API發送文檔進行簽名，這些REST API允許您採用Word、PDF、HTML和其他格式併發送它們進行簽名。

下面的示例介紹如何使用REST API文檔頁獲取先前生成的文檔並將其發送以進行簽名。 首先，瞭解如何通過Acrobat SignWeb介面完成此操作，然後如何使用REST API完成此操作。

## 獲取Acrobat Sign帳戶

如果您沒有Acrobat Sign帳戶，請註冊開發人員帳戶並查看文檔[此處](https://developer.adobe.com/adobesign-api/)，然後選擇&#x200B;**開發人員帳戶註冊**。 系統將提示您填寫表單並接收驗證電子郵件。 一旦您這樣做，您就會被指定到一個網站，以設定您的密碼和帳戶，然後您可以登錄到Acrobat Sign。

## 從Web介面發送協定

1. 從導航欄中選擇&#x200B;**[!UICONTROL 發送]**。

   ![Acrobat Sign「發送」頁籤的螢幕快照](assets/automatelegal_28.png)

1. 在&#x200B;*收件人*&#x200B;欄位中，指定兩個電子郵件地址。 最好使用與您的Acrobat Sign帳戶無關的電子郵件地址。

   ![「收件人」欄位的螢幕快照](assets/automatelegal_29.png)

1. 設定&#x200B;**[!UICONTROL 協定名稱]**&#x200B;和&#x200B;**[!UICONTROL 消息]**。
1. 選擇&#x200B;**[!UICONTROL 添加檔案]**，然後從電腦上載生成的檔案。
1. 選取&#x200B;**[!UICONTROL 「預覽和新增簽名欄位」]**。
1. 選取「**[!UICONTROL 下一步]**」。
1. 向下滾動到簽名頁時，可以根據標籤查看已放置的簽名欄位。

   ![簽名欄位的螢幕快照](assets/automatelegal_30.png)

1. 選取「**[!UICONTROL 傳送]**」。
1. 在您的電子郵件中，將顯示一條包含查看和簽名連結的消息。

   ![電子郵件螢幕截圖](assets/automatelegal_31.png)

1. 選擇&#x200B;**[!UICONTROL 審閱並簽名]**。
1. 選擇&#x200B;**[!UICONTROL 繼續]**&#x200B;以接受使用條款。
1. 選擇&#x200B;**[!UICONTROL 開始]**&#x200B;以跳轉到需要簽名的位置。

   ![開始標籤螢幕快照](assets/automatelegal_32.png)

1. 選擇&#x200B;**[!UICONTROL 按一下這裡簽署]**。

   ![按一下這裡簽署的螢幕快照](assets/automatelegal_33.png)

1. 鍵入您的簽名。

   ![鍵入簽名的螢幕快照](assets/automatelegal_34.png)

1. 選擇&#x200B;**[!UICONTROL 應用]**。
1. 選擇&#x200B;**[!UICONTROL 按一下以簽名]**。

向下一個簽名者發送電子郵件。 重複步驟9-16，查看和簽名第二簽名者。

協定完成後，協定的簽名副本通過電子郵件發送給各方。 此外，還可以從&#x200B;**管理**&#x200B;頁面的Acrobat SignWeb介面檢索已簽名的協定。

![Acrobat Sign「管理」頁籤的螢幕快照](assets/automatelegal_35.png)

接下來，通過REST API文檔瞭解如何執行相同方案。

## 獲取憑據

1. 導航到[Acrobat SignREST文檔](https://secure.na1.adobesign.com/public/docs/restapi/v6)。
1. 展開&#x200B;*tranientDocuments*&#x200B;和[POST/tranientDocuments](https://benprojecteddemo.na1.adobesign.com/public/docs/restapi/v6#!/tranientDocuments/createTranientDocument)。
1. 選擇&#x200B;**[!UICONTROL OAUTH ACCESS-TOKEN]**。

   ![選擇OAUTH ACCESS-TOKEN的位置螢幕快照](assets/automatelegal_36.png)

1. 檢查&#x200B;*agreementwrite*、*agreementsign*、*widgetwrite*&#x200B;和&#x200B;*librarywrite*&#x200B;的OAUTH權限。
1. 選擇&#x200B;**[!UICONTROL 授權]**。
1. 系統會通過彈出窗口提示您使用您的Acrobat Sign帳戶登錄。 登錄管理員的用戶名和密碼。
1. 系統將提示您允許訪問REST文檔。 選取「**[!UICONTROL 允許存取]**」。

然後將持有者令牌添加到&#x200B;**授權**&#x200B;欄位。

要瞭解如何為Acrobat Sign建立授權令牌的詳細資訊，請按照[此處概述的步驟](https://opensource.adobe.com/acrobat-sign/developer_guide/helloworld.html)操作。

## 上載臨時文檔

由於授權令牌是從前面的步驟添加的，因此您需要上載文檔以調用API:

1. 在&#x200B;*檔案*&#x200B;欄位中，上載在以前步驟中生成的PDF文檔。

   ![要上載PDF的螢幕快照](assets/automatelegal_37.png)

1. 選擇&#x200B;**[!UICONTROL 嘗試！]**。
1. 在&#x200B;**[!UICONTROL 響應正文]**&#x200B;中，複製&#x200B;*tranientDocumentId*&#x200B;值。

*tranientDocumentId*&#x200B;用於引用臨時儲存在Acrobat Sign的文檔，以便在後續的API調用中引用它。

## 傳送以供簽署

上傳文檔後，您需要發送協定進行簽名。

1. 展開協定部分和POST協定部分。
1. 在&#x200B;*AgreementInfo*&#x200B;欄位中，用以下JSON填充它：

   ```
   {
   "fileInfos": [
      {
         "transientDocumentId": "3AAABLblqZhAJeoswpyslef8_toTGT1WgBLk3TlhfJXy_uSLlKyre2hjF0-J1meBDn0PlShk0uQy6JghlqEoqXNnskq7YawteF6QWtHefP9wN2CW_Xbt0O9kq1tkpznG0a5-mEm4bYAV1FGOnD1mt_ooYdzKxm7KzTB11DLX2-81Zbe2Z1suy7oXiWNR3VSb-zMfIb5D4oIxF8BiNfN0q08RwT108FcB1bx4lekkATGld3nRbf8ApVPhB72VNrAIF0F1rAFBWTtfgvBKZaxrYSyZq73R_neMdvZEtxWTk5fii_bLVe7VdNZMcO55sofH61eQC_QIIsoYswZP4rw6dsTa68ZRgKUNs"
      }
   ],
   "name": "Terms and Conditions",
   "participantSetsInfo": [
      {
         "memberInfos": [
         {
            "email": "adobesigndemo+customer@outlook.com"
         }
         ],
         "order": 1,
         "role": "SIGNER"
      },
      {
         "memberInfos": [
            {
               "email": "adobesigndemo+company@outlook.com"
            }
         ],
         "order": 1,
         "role": "SIGNER"
         }
   ],
   "signatureType": "ESIGN",
   "state": "IN_PROCESS"
   }
   ```

1. 選擇&#x200B;**[!UICONTROL 嘗試！]**。

**POST協定API**&#x200B;返回協定的ID。 要獲取JSON模型架構的模板，請選擇&#x200B;**最小模型架構**。 **完整模型架構**&#x200B;部分提供了參數的完整清單。

## 檢查協定的狀態

一旦您擁有協定ID，就可以發送協定狀態。

1. 展開&#x200B;**[!UICONTROL GET/協定/{agreementId}]**。
1. 由於可能需要額外的OAUTH作用域，請再次選擇&#x200B;**[!UICONTROL OAUTH-ACCESS-TOKEN]**。
1. 將agreementId從先前的API調用響應複製到agreementId欄位。
1. 選擇&#x200B;**[!UICONTROL 嘗試退出！]**。

現在你掌握了有關協定的資訊。

```
{
    "id": "CBJCHBCAABAAc6LyP4SVuKXP_pNstzIzyripanRdz4IB",
    "name": "Terms and Conditions",
    "groupId": "CBJCHBCAABAAoyMb1yIgczAGhBuJeHf99mglPtM7ElEu",
    "type": "AGREEMENT",
    "participantSetsInfo": [
      {
        "id": "CBJCHBCAABAAzZE-IcHHkt05-AVbxas4Jz7DUl3oEBO6",
        "memberInfos": [
          {
            "email": "adobesigndemo+customer@outlook.com",
            "id": "CBJCHBCAABAAyWgMMReqbxUFM7ctI5xz16c2kOmEy-IQ",
            "securityOption": {
              "authenticationMethod": "NONE"
            }
          }
        ],
        "role": "SIGNER",
        "order": 1
      },
      {
        "id": "CBJCHBCAABAAaRHz3gY2W0w5n_6pj1GMMuZAfhBihc1j",
        "memberInfos": [
          {
            "email": "adobesigndemo+company@outlook.com",
            "id": "CBJCHBCAABAAOZQwjPwJXFiX8YDKPYtzMpftsmxYrIo9",
            "securityOption": {
              "authenticationMethod": "NONE"
            }
          }
        ],
        "role": "SIGNER",
        "order": 1
      }
    ],
    "senderEmail": "adobesigndemo+new@outlook.com",
    "createdDate": "2022-03-22T02:59:36Z",
    "lastEventDate": "2022-03-22T02:59:41Z",
    "signatureType": "ESIGN",
    "locale": "en_US",
    "status": "OUT_FOR_SIGNATURE",
    "documentVisibilityEnabled": true,
    "hasFormFieldData": false,
    "hasSignerIdentityReport": false,
    "documentRetentionApplied": false
  }
```

在更新更改時獲取通知的更有效方法是通過Webhooks，您可以在此[瞭解更多資訊](https://opensource.adobe.com/acrobat-sign/developer_guide/webhookapis.html)。

## 儲存簽名文檔

文檔簽名後，可以使用GET/agreements/combinedDocument檔案檢索它。

1. 展開&#x200B;**[!UICONTROL GET/協定/{agreementId}/combinedDocument]**。
1. 將&#x200B;**[!UICONTROL agreementId]**&#x200B;設定為從上次API調用提供的&#x200B;*agreementId*。
1. 選擇&#x200B;**[!UICONTROL 嘗試退出！]**。

可以使用attachSupportingDocuments和attachAuditReport參數設定附加審計報告或支援文檔的其他參數。

在&#x200B;**響應正文**&#x200B;中，它隨後可以下載到您的電腦並儲存在您喜歡的位置。

## 更多選項

除了生成文檔併發送以供簽名外，還可以執行其他操作。

例如，如果文檔沒有簽名，則Adobe PDF服務API提供了在生成協定後轉換文檔的多種方法，例如：

* 使用密碼保護文檔
* 如果有大影像，則壓縮PDF
* 要瞭解有關其他可用操作的詳細資訊，請查看Adobe PDF服務API示例檔案/src資料夾中的指令碼。 您還可以通過查看可使用的不同操作的文檔來瞭解更多資訊。

此外，Acrobat Sign還提供以下幾項附加功能：

* 將簽名體驗嵌入應用程式
* 為簽名者添加身份驗證方法
* 配置電子郵件通知設定
* 作為協定的一部分下載單獨的文檔

## 進一步學習

有興趣學習更多內容嗎？ 請查看其他使用[!DNL Adobe Acrobat Services]的方法：

* 從[文檔瞭解更多資訊](https://developer.adobe.com/document-services/docs/overview/)
* 查看有關Adobe Experience League的更多教程
* 使用/src資料夾中的示例指令碼查看如何使用PDF
* 有關最新提示和技巧，請關注[Adobe技術部落格](https://medium.com/adobetech/tagged/adobe-document-cloud)
* 訂閱[紙片剪輯（每月即時流）](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF)以瞭解使用[!DNL Adobe Acrobat Services]的自動化。


