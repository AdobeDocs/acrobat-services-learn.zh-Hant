---
title: 處理發票
description: 瞭解如何自動產生、以密碼保護和傳送客戶發票
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8145
thumbnail: KT-8145.jpg
exl-id: 5871ef8d-be9c-459f-9660-e2c9230a6ceb
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1343'
ht-degree: 0%

---

# 處理發票

![使用案例 英雄橫幅](assets/UseCaseInvoicesHero.jpg)

當業務蓬勃發展時，這很好，但在準備所有這些發票時生產力會受到影響。 手動生成發票非常耗時，而且您還冒著出錯、可能賠錢或因金額不正確而激怒客戶的風險。

例如，Danielle 在一家醫療供應公司的](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html)會計部門[](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html)工作[。現在是月底，所以她從幾個不同的系統中提取資訊，兩次檢查其準確性，並格式化發票。 在完成所有工作之後，她終於準備好將檔轉換為 PDF （如此一來，任何人都可以檢視檔而不購買特定軟體），並將他們的個人化發票傳送給每位客戶。

即使月度發票完成，丹妮爾也無法逃脫這些發票。 有些客戶有非月帳單週期，所以她總是為某人創建發票。 有時，客戶會編輯他們的發票並少付錢。 然後，Danielle 花時間對此發票不匹配進行故障排除。 按照這個速度，她需要聘請一名助手來跟上所有的工作！

Danielle 需要的是一種快速準確地生成發票的方法，既可以在月底批量生成發票，也可以在其他時間廣告臨時生成發票。 理想情況下，如果她可以保護這些發票免受編輯，她就不必擔心對不匹配的金額進行故障排除。

## 您可以學習的內容

在此實作教學課程中，瞭解如何使用 Adobe 產生檔API自動產生發票、以密碼保護 PDF，以及向每位客戶傳送發票。 只需要對 Node.js、JavaScript、Express.js、HTML 和 CSS 略知一下。

GitHub](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation) 提供此專案的[完整程序代碼。您必須使用範本和 Raw 資料資料資料夾來設定公開目錄。 在生產中，您必須從外部API擷取數據。 您也可以探索這個應用程式的封存版本，其中包含範本資源。

## 相關 API 和資源

* [PDF 服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe檔產生API](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html)

* [項目代碼](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)

## 準備數據

本教學課程並未說明數據如何從數據倉儲匯入。 您的客戶訂單可能存在於資料庫、外部API或自定義軟體中。 Adobe產生文件API預期會有包含開票數據的 JSON 文件，例如來自您的 客戶關係管理 （CRM） 或電子商務平台的信息。 本教學課程假設數據已採用 JSON 格式。

如需簡化，請使用下列 JSON 結構開啟：

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

JSON 檔包含客戶詳細數據和訂單資訊。 使用此結構化檔以建立發票並顯示 PDF 格式的元素。

## 建立發票範本

Adobe檔產生API預期會有Microsoft Word 型範本和 JSON 檔建立動態 PDF 或 Word 檔。 為開啟應用程式建立Microsoft Word 範本，並使用 [免費的 Document Generation Tagger 載入宏](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) 來產生範本標籤。 安裝載入宏，然後在 Microsoft Word 中開啟索引標籤。

![檔世代 Tagger 載入宏的螢幕擷圖](assets/invoices_1.png)

將 JSON 內容貼到載入宏後（如上所示），請按兩下「產生標籤」。 現在，此增效模組會顯示物件的格式。 您的基本範本可以使用客戶名稱和電子郵件，但不會顯示訂單資訊。 此教學課程稍後會討論訂單資訊。

![Document Generation Tagger Author 範本螢幕擷圖](assets/invoices_2.png)

在您的 Microsoft Word 檔內，開始撰寫發票範本。 離開您必須插入動態數據的游標，然後從Adobe載入宏視窗中選取標籤。 按兩下 **「插入文字** 」，這樣Adobe文件產生Tagger載宏就可以產生和插入卷標。 若要進行個人化，讓我們插入客戶名稱和電子郵件。

現在，繼續處理每張新發票所變更的數據。 選取&#x200B;****&#x200B;載入宏的「進階」索引標籤。若要查看根據客戶訂購的產品產生動態表格的可用選項，請按兩下「 **表格和清單」** 。

從第一個下拉式清單中選 **取「順序** 」。 在第二個下拉式清單中，選取此表格的欄。 在此教學課程中，選取物件演算表格的所有三欄。

![「檔案世代Tagger進階」索引標籤的螢幕擷圖](assets/invoices_3.png)

檔產生API還可以執行複雜的操作，例如將陣列內的元素彙集在一起。 在「進&#x200B;**階**」標籤中，選&#x200B;**取「數字計算**」，然後在「匯總&#x200B;**」索引標籤中**，選取您要套用計算的欄位。

![檔世代 Tagger 數字計算的螢幕擷圖](assets/invoices_4.png)

按下「 **插入計算」** 按鈕，在檔中需要的地方插入此標籤。 以下文字現在會出現在您的 Microsoft Word 檔案中：

![Microsoft Word 檔中標籤的螢幕擷圖](assets/invoices_5.png)

此發票範例包含客戶資訊、訂購產品和到期金額總額。

## 使用 Adobe 產生檔API產生發票

使用 Adobe PDF Services Node.js 軟體開發工具包 （SDK） 來合併Microsoft Word 和 JSON 檔。 建立Node.js應用程式，以使用 Document Generation API 建立發票。

PDF Services API包含「文件產生服務」，因此您可以為兩者使用相同的認證。 [享受六個月免費試](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)用，然後每份檔交易只需支付 $0.05 美元。

以下是合併 PDF 的程式代碼：

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

此程式代碼會從輸入 JSON 檔和輸入範本檔案中取得資訊。 然後，建立文件合併作業，將檔案合併為單一 PDF 報告。 最後，它會使用您的API認證執行操作。 如果您還沒有認證， [請建立認證](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getting-credentials) （文件產生和 PDF 服務API使用相同的認證）。

在 Express 路由器中使用此代碼來處理檔要求：

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

一旦執行此代碼，它將根據提供的數據提供包含動態產生發票的 PDF 檔。 透過範例 JSON 資料 （如上所述），此程式代碼的輸出為：

![動態產生的 PDF 發票螢幕擷圖](assets/invoices_6.png)

此發票內含 JSON 檔的動態數據。

## 保護密碼的發票

Danielle 擔心客戶改變發票，請套用密碼來限制編輯。 [PDF 服務 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html) 可以自動將密碼套用至文件。 在這裡，您可以使用 Adobe PDF Services SDK 通過密碼保護文件。 代碼為：

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

使用此代碼時，它會使用密碼保護您的檔，並將新發票上傳到系統。 有關如何使用此代碼的詳細資訊或嘗試一下，請參閱 [代碼範例](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)。

一旦使用發票完成後，您可能會想要自動將發票寄給用戶端。 有幾種方法可以完成自動向您的客戶發送電子郵件。 最快速的方式是使用第三方電子郵件API以及 sendgrid-nodejs 等 [輔助程序](https://github.com/sendgrid/sendgrid-nodejs)資料庫。 或者，如果您已經可以存取 SMTP 伺服器，則可以使用 [節點郵件透過](https://www.npmjs.com/package/nodemailer) SMTP 傳送電子郵件。

## 後續步驟

在這個動手教學課程中，你創建了一個簡單的應用程式來説明 Danielle 進行開具發票](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html)的[會計核算。使用 PDF 服務 API 和文檔生成 SDK，您使用 JSON 文件中的客戶訂單信息填充了 Microsoft Word 範本，從而創建了 PDF 發票。 然後，利用 PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html) 提供的[密碼保護服務，對每個文件進行密碼保護。

由於 Danielle 可以自動生成發票，並且不必擔心客戶編輯發票，因此她不需要聘請助手來説明完成所有手動工作。 她可以利用額外的時間在應付賬款檔中節省成本。

現在您已經看到了它是多麼容易，您可以使用其他Adobe Systems工具擴展這個簡單的應用程式，以在您的網站上嵌入發票。 例如，因此客戶可以隨時檢視其發票或結餘。 [Adobe PDF內嵌API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 可供免費使用。 您甚至可以前往人力資源或銷售部門，協助自動化他們的合約並收集電子簽名。

若要探索所有的可能性，並開始建立您專屬的便利應用程式，請建立免費 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 帳戶立即開始使用。 享受六個月免費試用，然後 [按即](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)付費
業務規模化時，每份檔交易只要 $0.05。
