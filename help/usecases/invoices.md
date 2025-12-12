---
title: 處理髮票
description: 瞭解如何自動生成、密碼保護和交付客戶發票
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8145
thumbnail: KT-8145.jpg
exl-id: 5871ef8d-be9c-459f-9660-e2c9230a6ceb
source-git-commit: bd53d86abb0e5f9ee302c39e07c00101e5a1f8ed
workflow-type: tm+mt
source-wordcount: '1343'
ht-degree: 0%

---

# 處理髮票

![使用案例英雄橫幅](assets/UseCaseInvoicesHero.jpg)

當業務蓬勃發展時，這是件好事，但當準備所有這些發票時，生產率就會下降。 手動生成發票非常耗時，而且您會面臨出錯、可能賠錢或用不正確的金額激怒客戶的風險。

例如，請考慮Danielle在醫療用品公司[會計部](https://developer.adobe.com/document-services/use-cases/financial/invoices) [工作](https://developer.adobe.com/document-services/use-cases/financial/invoices)。 這是月底，所以她從幾個不同的系統中提取資訊，重複檢查其準確性，並格式化發票。 在完成了所有這些工作後，她終於準備好將文檔轉換為PDF（這樣，任何人都可以在不購買特定軟體的情況下查看文檔），並向每個客戶發送其個性化發票。

即使每月開具發票完成，丹妮爾也無法逃過這些發票。 有些客戶有每月的帳單週期，因此她總是為某人建立發票。 偶爾，客戶會編輯發票並支付低額費用。 丹妮爾則花時間解決發票不匹配問題。 這樣，她就需要請個助理來跟上所有的工作！

丹妮爾需要的是一種快速、準確地生成發票的方法，不管是在月底批發，還是在其他時候臨時生成。 理想情況下，如果她能夠保護這些發票不受編輯，她就不必擔心對不匹配的金額進行故障排除。

## 你能學到的

在本實踐教程中，瞭解如何使用Adobe文檔生成API自動生成發票、對PDF進行密碼保護，以及將發票交付給每個客戶。 只需對Node.js、JavaScript、Express.js、HTML和CSS稍加瞭解即可。

此項目的完整代碼是[，可在GitHub](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)上使用。 必須使用模板和原始資料資料夾設定公共目錄。 在生產中，必須從外部API獲取資料。 您還可以瀏覽包含模板資源的應用程式的此存檔版本。

## 相關API和資源

* [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe文檔生成API](https://developer.adobe.com/document-services/apis/doc-generation)

* [Adobe SignAPI](https://developer.adobe.com/adobesign-api/)

* [項目代碼](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)

## 準備資料

本教程不介紹如何從資料倉庫導入資料。 您的客戶訂單可能存在於資料庫、外部API或自定義軟體中。 Adobe文檔生成API需要包含開票資料的JSON文檔，例如來自您的客戶關係管理(CRM)或電子商務平台的資訊。 本教程假定資料已採用JSON格式。

為簡單起見，請使用以下JSON結構進行開票：

```
{ 
    "customerName": "John Doe", 
    "customerEmail": "john-doe@example.com", 
    "order": [ 
        { 
            "productId": 26, 
            "productTitle": "Bandages", 
            "price": 15.82 
        }, 
        { 
            "productId": 54, 
            "productTitle": "Masks", 
            "price": 25 
        }, 
        { 
            "productId": 76, 
            "productTitle": "Gloves", 
            "price": 7.59 
        } 
    ] 
} 
```

JSON文檔包含客戶詳細資訊和訂單資訊。 使用此結構化文檔可生成發票並以PDF格式顯示要素。

## 生成發票模板

Adobe文檔生成API需要基於MicrosoftWord的模板和JSON文檔來建立動態PDF或Word文檔。 為開票應用程式建立MicrosoftWord模板，並使用[免費文檔生成標籤載入項](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)生成模板標籤。 安裝載入項並在MicrosoftWord中開啟頁籤。

![文檔生成標誌器載入項的螢幕快照](assets/invoices_1.png)

將JSON內容貼上到載入項後，按一下「生成標籤」。 現在，此插件顯示了對象的格式。 您的基本模板可以使用客戶的姓名和電子郵件，但不顯示訂單資訊。 本教程的後面將討論訂單資訊。

![文檔生成標籤者作者模板的螢幕快照](assets/invoices_2.png)

在您的MicrosoftWord文檔中，開始編寫發票模板。 將游標保留在必須插入動態資料的位置，然後從「Adobe」載入項窗口中選擇標籤。 按一下&#x200B;**插入文本**，以便Adobe文檔生成標籤載入項可以生成和插入標籤。 要個性化，請插入客戶的姓名和電子郵件。

現在，轉到每張新發票都會更改的資料。 選擇載入項的&#x200B;**高級**&#x200B;頁籤。 要查看根據客戶訂購的產品生成動態表的可用選項，請按一下&#x200B;**表和清單**。

從第一個下拉清單中選擇&#x200B;**訂單**。 在第二個下拉清單中，選擇此表的列。 在本教程中，為對象選擇所有三列以呈現表。

![「文檔生成標誌器高級」頁籤的螢幕快照](assets/invoices_3.png)

文檔生成API還可以執行複雜操作，如在陣列內聚合元素。 在&#x200B;**高級**&#x200B;頁籤中，選擇&#x200B;**數值計算**，在&#x200B;**聚合**&#x200B;頁籤中，選擇要應用計算的欄位。

![文檔生成標籤數字計算的螢幕快照](assets/invoices_4.png)

按一下&#x200B;**插入計算**&#x200B;按鈕，在文檔中需要的位置插入此標籤。 以下文本現在出現在您的MicrosoftWord檔案中：

![MicrosoftWord文檔中標籤的螢幕快照](assets/invoices_5.png)

此發票示例包含客戶資訊、訂購產品和到期總額。

## 使用Adobe單據生成API生成發票

使用Adobe PDF服務Node.js軟體開發工具包(SDK)將MicrosoftWord和JSON文檔組合起來。 生成Node.js應用程式，以使用文檔生成API建立發票。

PDF服務API包括文檔生成服務，因此您可以對兩者使用相同的憑據。 享受[6個月免費試用](https://developer.adobe.com/document-services/pricing/main)，然後每個文檔交易僅支付0.05美元。

下面是要合併PDF的代碼：

```
async function compileDocFile(json, inputFile, outputPdf) { 
    try { 
        // configurations 
        const credentials =  adobe.Credentials 
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

此代碼從輸入JSON文檔和輸入模板檔案獲取資訊。 然後，建立文檔合併操作，將檔案合併到單個PDF報告中。 最後，它使用您的API憑據執行操作。 如果您尚未擁有這些憑據，[將建立憑據](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getting-credentials)&#x200B;(文檔生成和PDF服務API使用相同的憑據)。

在Express路由器內使用此代碼處理文檔請求：

```
// Create one report and send it back
try {
    console.log(\`[INFO] generating the report...\`);
    const fileContent = fs.readFileSync(\`./public/documents/raw/\${vendor}\`,
    'utf-8');
    const parsedObject = JSON.parse(fileContent);

    await pdf.compileDocFile(parsedObject,
    \`./public/documents/template/Adobe-Invoice-Sample.docx\`,
    \`./public/documents/processed/output.pdf\`);

    await pdf.applyPassword("p@55w0rd", './public/documents/processed/output.pdf',
    './public/documents/processed/output-secured.pdf');

    console.log(\`[INFO] sending the report...\`);
    res.status(200).render("preview", { page: 'invoice', filename: 'output.pdf' });
} catch(error) {
    console.log(\`[ERROR] \${JSON.stringify(error)}\`);
    res.status(500).render("crash", { error: error });
}
```

一旦執行該代碼，它就基於所提供的資料提供包含動態生成的發票的PDF文檔。 對於示例JSON資料（上面提供），此代碼的輸出為：

![動態生成的PDF發票螢幕快照](assets/invoices_6.png)

此發票包括JSON文檔中的動態資料。

## 密碼保護髮票

由於會計擔心客戶更改發票，因此請應用密碼以限制編輯。 [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)可以自動將密碼應用於文檔。 在此，您使用Adobe PDF服務SDK使用密碼保護文檔。 代碼為：

```
async function applyPassword(password, inputFile, outputFile) {
    try {
        // Initial setup, create credentials instance.
        const credentials = adobe.Credentials
        .serviceAccountCredentialsBuilder()
        .fromFile("./src/pdftools-api-credentials.json")
        .build();

        // Create an ExecutionContext using credentials
        const executionContext = adobe.ExecutionContext.create(credentials);
        // Create new permissions instance and add the required permissions
        const protectPDF = adobe.ProtectPDF,
        protectPDFOptions = protectPDF.options;
        // Build ProtectPDF options by setting an Owner/Permissions Password, Permissions,
        // Encryption Algorithm (used for encrypting the PDF file) and specifying the type of content to encrypt.
        const options = new protectPDFOptions.PasswordProtectOptions.Builder()
        .setOwnerPassword(password)
        .setEncryptionAlgorithm(protectPDFOptions.EncryptionAlgorithm.AES_256)
        .build();

        // Create a new operation instance.
        const protectPDFOperation = protectPDF.Operation.createNew(options);

        // Set operation input from a source file.
        const input = adobe.FileRef.createFromLocalFile(inputFile);
        protectPDFOperation.setInput(input);

        // Execute the operation and Save the result to the specified location.
        let result = await protectPDFOperation.execute(executionContext);

        result.saveAsFile(outputFile);
    } catch (err) {
        console.log('Exception encountered while executing operation', err);
    }
}
```

使用此代碼時，它會使用密碼保護您的文檔，並將新發票上載到系統。 有關如何使用此代碼或嘗試此代碼的詳細資訊，請參閱[代碼示例](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)。

發票完成後，您可能需要自動將其電子郵件發送給客戶端。 有幾種方法可以自動發送您的客戶端電子郵件。 最快的方法是使用第三方電子郵件API和幫助程式庫，如[sendgrid-nodejs](https://github.com/sendgrid/sendgrid-nodejs)。 或者，如果您已經擁有訪問SMTP伺服器的權限，則可以使用[nodemailer](https://www.npmjs.com/package/nodemailer)通過SMTP發送電子郵件。

## 後續步驟

在本實踐教程中，您建立了一個簡單的應用程式，以幫助Danielle使用[開票](https://developer.adobe.com/document-services/use-cases/financial/invoices)進行記帳。 使用PDF服務API和文檔生成SDK，您使用JSON文檔中的客戶訂單資訊填充了MicrosoftWord模板，從而建立PDF發票。 然後，[PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)使用密碼保護服務對每個文檔進行密碼保護。

由於丹妮爾可以自動生成發票，而且不必擔心客戶編輯發票，因此她不需要聘請助理來幫助完成所有的手工工作。 她可以利用額外時間在應付帳款檔案中查找成本節約。

現在，您已看到它有多容易，您可以使用其他Adobe工具來擴展此簡單的應用，以便在您的網站上嵌入發票。 例如，這樣客戶可以隨時查看其發票或餘額。 [Adobe PDF嵌入API](https://developer.adobe.com/document-services/apis/pdf-embed)可免費使用。 您甚至可以轉到人力資源部門或銷售部門，幫助自動化他們的協定並收集電子簽名。

若要探索所有可能性，並開始構建自己的方便應用程式，請建立一個免費[[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)帳戶，以便立即開始。 享受6個月的免費試用期，然後[按使用付費](https://developer.adobe.com/document-services/pricing/main)
在您的業務擴展中，每筆文檔交易只需0.05美元。

