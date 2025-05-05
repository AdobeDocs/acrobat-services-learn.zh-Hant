---
title: 審核與核准
description: 瞭解如何為跨團隊共同作業建立文件審核和核准工作流程
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8094
thumbnail: KT-8094.jpg
exl-id: d704620f-d06a-4714-9d09-3624ac0fcd3a
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1540'
ht-degree: 0%

---

# 審核和核准

![使用案例主打橫幅](assets/UseCaseReviewsHero.jpg)

在 COVID-19 疫情期間，許多公司需要進行遠端跨團隊協作， [共用和審核數位檔](https://developer.adobe.com/document-services/use-cases/collaboration/review-and-approval) 會為團隊和跨職能資源帶來一系列挑戰。

這些挑戰包括以不同的檔格式共享檔、有效檢閱和註釋內容，以及與最近的編輯進行同步。 [!DNL Adobe Acrobat Services] API 的設計是為了讓應用程式開發人員為使用者解決這些挑戰。

## 您可以學習哪些內容

此實作教學課程將說明如何在 Node.js 和 Express 網頁應用程式中建立文件審核和核准工作流程。 若要跟著這個教學課程學習，您需要一些Node.js體驗。

應用程式具有下列功能：

* 將不同檔類型轉換為 PDF

* 啟用檔案上傳

* 讓用戶能夠新增註釋和批註

* 在註釋中顯示 PDF

* 啟用使用者配置檔以識別註釋作者

* 將檔案合併成用戶可下載的最終 PDF

## 相關 API 和資源

* [PDF 服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF 嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [項目代碼](https://github.com/contentlab-io/adobe_reviews_and_approvals)

## 建立Adobe API認證

在啟動程式代碼之前，您必須 [為「嵌入API」和「Adobe PDF服務」API建立Adobe PDF認證](https://www.adobe.com/go/dcsdks_credentials) 。 PDF 內嵌API可供免費使用。 PDF Services API六個月免費使用，然後您就可以轉換為 [依即付費計劃](https://developer.adobe.com/document-services/pricing/main) ，每份檔交易只要 \$0.05。

API建立 PDF 服務認證時，請選取「 **建立個人化程式代碼範例** 」選項，然後選取語言的Node.js。 儲存 ZIP 檔案並解壓縮 pdftools-api-credentials.json，然後將private.key到 Node.js Express 專案的根目錄。

## 設定專案和相依性

設定您的 Node.js 和 Express 專案，以便從名為「public」的資料夾中提供靜態檔案。 您可以根據自己的偏好設定專案方式。 若要快速開始使用，您可以使用 [Express 應用程式產生器](https://expressjs.com/en/starter/generator.html)。 或者，如果您想要讓事情保持簡單，您可以 [從頭](https://expressjs.com/en/starter/hello-world.html) 開始，將程式代碼保存在單一JavaScript檔案中。 在上述連結的範例專案中，您正在使用一個檔案的方法，並將您的所有程式代碼保留在index.js。

`pdftools-api-credentials.json`將個人化程式代碼範例中的檔案和`private.key`檔案複製到專案的根目錄。此外，如果有 .gitignore 檔案，請將其新增至 .gitignore 檔案，這樣您的認證檔案就不會意外傳送至儲存庫。

接下來，執行 `npm install @adobe/documentservices-pdftools-node-sdk` 以安裝適用於 PDF Services 的 Node.js SDK。 在其他相依性匯入后，匯入此模組，並在您的程式代碼內建立API認證物件 （在範例專案中index.js）：

```
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();
```

您的起始程式代碼應如下所示：

```
  
  const express = require( "express" );
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();

  const app = express();

  app.use( express.static( "public" ) );

  app.listen( 8889, function() {
      console.log( "Server started on port", 8889 );
  } );
```

現在您可以使用 [!DNL Acrobat Services] API 了。

## 將檔案轉換為 PDF

在檔工作流程的第一部分，用戶必須上傳要共享的檔。 若要啟用此功能，您可以新增上傳功能，並將不同檔檔格式合併到 PDF 中，為審核程式做準備。

首先，根據 [PDF Services API 的範例片段，建立將文件轉換為 PDF 的功能](https://developer.adobe.com/document-services/apis/pdf-services)。 此範例也顯示了許多其他重要功能的片段，包括光學字元識別 （OCR）、密碼保護和移除以及壓縮。

```
function fileToPDF( filename, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

      // Set operation input from a source file.
      const input = PDFToolsSdk.FileRef.createFromLocalFile( filename );
      createPdfOperation.setInput( input );

      // Execute the operation and Save the result to the specified location.
      createPdfOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
  }
```

您現在可以使用此功能，從已上傳的檔建立 PDF。

## 處理檔案上傳

接下來，伺服器需要在網頁伺服器上上傳檔案端點，才能接收和處理檔。

首先，在上傳檔案夾內建立一個檔案夾，並將其命名為「草稿」。 您可以將已上傳的檔案和轉換後的 PDF 檔案儲存在這裡。 接下來，執行 `npm install express-fileupload` 以安裝 Express-FileUpload 模組，並在程式代碼中將中間件新增至 Express：

```
const fileUpload = require( "express-fileupload" );
app.use( fileUpload() );
```

現在，新增端 `/upload `點，然後使用相同的文件名將上傳的檔案儲存在 Drafts 檔案夾中。 然後，請呼叫您先前撰寫的功能，以建立同一份檔的 PDF 檔案 （如果尚未採用 PDF 格式）。 您可以根據上傳的原始檔案名稱產生新 PDF 檔案的檔案名稱：

```
// Create a PDF file from an uploaded file
app.post( "/upload", ( req, res ) => {
    if( !req.files || Object.keys( req.files ).length === 0 ) {
        return res.status( 400 ).send( "No files were uploaded." );
    }
    
    // Create PDF from the uploaded file
    let file = req.files.myFile;
    file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
        if( err ) {
            return res.status( 500 ).send( err );
        }
        if( file.name.endsWith( ".pdf" ) ) {
            res.redirect( "/" );
        }
        else {
            // Convert to PDF
            fileToPDF( __dirname + "/uploads/drafts/" + file.name, __dirname + "/uploads/drafts/" + file.name.replace( /\./g, "-" ) + ".pdf", ( file ) => {
                res.redirect( "/" );
            } );
        }
    });
} );
```

## 建立上傳頁面

現在，若要從網頁應用程式上傳檔案，請在上傳檔案夾內建立 `index.html` 網頁。 在頁面上，新增檔案上傳表格，將檔案傳送至 /上傳端點：

```
<form ref="uploadForm" 
      action="/upload"
      method="post" 
      encType="multipart/form-data">
      <input type="file" name="myFile" accept=".doc,.docx,.ppt,.pptx,.xls,.xlsx,.txt,.rtf,.bmp,.jpg,.gif,.tiff,.png">
      <input type="submit" value="Upload File" />
  </form>
```

![網頁上傳檔案功能的螢幕擷圖](assets/reviews_1.png)

您現在可以將檔案上傳至 Node.js 伺服器。 伺服器會將檔案儲存在上傳/草稿資料夾內，並在檔案旁邊建立 PDF 格式版本。

您現在已準備好嵌入上傳的檔，所以請使用 PDF 內嵌API，讓使用者能夠輕鬆地將註釋和批註新增至檔。

## 列舉 PDF 檔案

一般檔工作流程可能涉及多個檔，因此您必須顯示檔案清單，並將每個檔案連結至應用程式中的新檔審核頁面。

首先，在伺服器代碼內新增 /檔案端點，該端點會取得並傳回儲存在上傳/草稿檔案夾中的所有 PDF 檔案清單：

```
const fs = require( "fs" );

app.get( "/files", ( req, res ) =\> {

fs.readdir( \_\_dirname + "/uploads/drafts/", ( err, files ) =\> {

if( err ) {

return res.status( 500 ).send( err );using

}

return res.json( files.filter( f =\> f.endsWith( ".pdf" ) ) );

} );

} );
```

`/download/:file`新增可存取已上傳 PDF 檔案的路徑，以便嵌入至網頁。

>[!NOTE]
>
>在生產應用程式中，您必須新增驗證和授權，以確保要求來自有效的使用者，並允許使用者存取檔。

```
app.get( "/download/:file", function( req, res ){
    // Note: In production code, this should check authentication and user access permissions
    res.download( __dirname + "/uploads/drafts/" + req.params[ "file" ] );
});
```

使用在載入時間填滿的檔案清單元素更新index.html頁面。 每個專案都可以連結至draft.html網頁，而且您會使用查詢字串參數將檔名傳遞至頁面。

>[!NOTE]
>
>您可以使用 jQuery 附加每個專案，因此您必須在網頁上載入 jQuery 資料庫，或使用其他方法附加元素。

```
  <ul id="filelist">
      <li>Loading documents...</li>
  </ul>

  ...

  <script>
      // Load current files
      fetch( "/files" )
      .then( r => r.json() )
      .then( files => {
          if( files && files.length > 0 ) {
              $( "#filelist" ).empty();
              files.forEach( file => {
                  $( "#filelist" ).append( `<li><a
  href="/draft.html?file=${file}">${file}</a></li>` );
              })
          } else {
                  $("#filelist").append("<div>No documents found.</div>");
                }
      });
  </script>
```

![選取要審核的檔案的螢幕擷圖](assets/reviews_2.png)

## 嵌入 PDF

您已準備好在網頁應用程式中內嵌入和顯示 PDF 檔案。

建立名為「draft.html」的網頁，並在頁面上為嵌入的 PDF 新增 div 元素：

```
  <div id="adobe-dc-view"></div>
```

包含資料庫 [!DNL Acrobat Services] ：

```
  <script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

在自定義文本標籤中，從查詢字串參數剖析檔名，讓您知道要在頁面上嵌入哪個檔案：

```
  <script type="text/javascript">
          let params = new URLSearchParams( window.location.search );
          let filename = params.get( "file" );
  </script>
```

針對可將指定 PDF 檔案載入 div 元素內的內嵌檢視的 adobe_dc_view_sdk.ready 事件新增檔事件聽眾。 使用 PDF 內嵌API認證中的用戶端 ID。 您想要啟用註釋和批註，所以請將檢視嵌FULL_WINDOW模式，然後將 showAnnotationsTools 選項設為 true。

```
  document.addEventListener( "adobe_dc_view_sdk.ready", () => { 
      var adobeDCView = new AdobeDC.View( { 
          clientId: "YOUR CLIENT ID HERE",
          divId: "adobe-dc-view",
          locale: "en-US",
      } );
      adobeDCView.previewFile( {
          content: { location: { url: "download/" + filename } },
          metaData: { fileName: "Draft Version.pdf" }
      }, {
          embedMode: "FULL_WINDOW",
          showAnnotationTools: true,
          showPageControls: true
      } );
  });
```

## 建立使用者配置檔

依預設，此檢視中的註釋和批註會顯示為「訪客」。 您可以將頁面代碼中的使用者基本數據回呼註冊至 PDF 檢視，以設定註釋和批註的目前審核者名稱。 以下是範例描述檔。 在包含使用者驗證的完整應用程式中，可透過這種方式設定登入使用者會話的配置檔資訊，以識別審核工作流程中檔的每個註釋者。

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.GET_USER_PROFILE_API,
      () => {
          return new Promise( ( resolve, reject ) => {
              resolve({
                  code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                  data: {
                      userProfile: {
                          name: "YOUR NAME",
                          firstName: "FIRST",
                          lastName: "LAST",
                          email: "document.editor@adobe.com"
                      }
                  }
              });
          });
      }
  );
```

當您使用此網頁看到任何已上傳的檔並加上批注時，您的配置檔會將您識別為特定使用者。

## 儲存檔回饋

使用者對文件發表評論后，他們按兩下「儲存 **」。**&#x200B;依預設，按一下「儲存&#x200B;**&#x200B;**」會下載更新後的 PDF 檔案。變更此動作以更新伺服器上目前的 PDF 檔案。

`/save`在上傳/草稿資料夾中，將端點新增至覆寫 PDF 檔案的伺服器程式代碼：

```
  // Overwrite the PDF file with latest PDF changes and annotations
  app.post( "/save", ( req, res ) => {
      if( !req.files || Object.keys( req.files ).length === 0 ) {
          return res.status( 400 ).send( "No files were uploaded." );
      }

      let file = req.files.pdf;
      file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          res.send( "File uploaded" );
      });
  } );
```

針對將內容上傳至 /儲存端點的 SAVE_API 註冊 PDF 檢視回呼。 您可以變更 autoSaveFrequency 值，讓您的應用程式在定時器上自動儲存變更，並在完成時將其他元數據加入目前內嵌的檔案 （如需要）。

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.SAVE_API,
      ( metaData, content, options ) => {
          return new Promise( ( resolve, reject ) => {
              let formData = new FormData();
              formData.append( "pdf", new Blob( [ content ] ), "drafts/" + filename
  );
              fetch( "/save", {
                  method: "POST",
                  body: formData
              }).then( resp => {
                  resolve({
                      code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                      data: {
                          /* Updated file metadata after successful save operation */
                          metaData: Object.assign( metaData, {} )
                      }
                  });
              });
          });
      },
      {
          autoSaveFrequency: 0,
          enableFocusPolling: false,
          showSaveButton: true
      }
  );
```

現在，伺服器上會儲存檔草稿上的註釋和附註。 您可以 [進一步瞭解回](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#callbacks-workflows) 呼如何融入您的工作流程。 例如， [如果多人想要同時審閱同一份檔並加上註釋，狀態回](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#status-callback) 呼有助於處理檔案衝突。

在最後的步驟中，您可以使用「PDF 服務」API將所有編輯過的檔合併為一個 PDF 檔案。

## 合併 PDF 檔案

PDF 組合程式代碼就像 PDF 建立程式碼，但使用 CombineFiles作，然後將每個檔案新增為輸入。

```
  function combineFilesToPDF( files, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          combineFilesOperation = PDFToolsSdk.CombineFiles.Operation.createNew();

      // Set operation inputs from source files.
      files.forEach( file => {
          const input = PDFToolsSdk.FileRef.createFromLocalFile( file );
          combineFilesOperation.addInput( input );
      } );

      // Execute the operation and Save the result to the specified location.
      combineFilesOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
 }
```

## 下載最終 PDF

新增名為 /finalize 的端點，呼叫功能，將資料夾內 `uploads/drafts` 的所有 PDF 檔案合併到 `Final.pdf` 檔案中，然後下載。

```
  app.get( "/finalize", ( req, res ) => {
      fs.readdir( __dirname + "/uploads/drafts/", ( err, files ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          combineFilesToPDF(
              files.filter( f => f.endsWith( ".pdf" ) ).map( f => __dirname + 
  "/uploads/drafts/" + f ),
              __dirname + "/uploads/Final.pdf", ( file ) => {
              res.download( file );
          } );
      } );
  } );
```

最後，在主要index.html網頁中新增連結至此 /finalize 端點。 此連結可讓使用者下載檔工作流程的結果。

```
<a href="/finalize">Download final PDF</a>
```

![下載最終文件的螢幕擷圖](assets/reviews_3.png)

## 後續步驟

本實作教學課程展示了 API 如何[!DNL Acrobat Services]將檔共享和審核工作流程[&#128279;](https://developer.adobe.com/document-services/use-cases/collaboration/review-and-approval)整合到網頁應用程式中。此應用程式可讓遠端工作人員共用檔案並與團隊成員共同作業，這對在家工作的員工和承包商特別有説明。

您可以使用這些技術在您的應用程式中啟用共同作業，或探索 [PDF Services Node SDK 範例](https://github.com/adobe/pdftools-node-sdk-samples) 和 [PDF 內嵌API在 GitHub 上的範](https://github.com/adobe/pdf-embed-api-samples) 例，以獲得關於如何使用 Adobe API 的靈感。

準備好在自己的應用程式中啟用檔共享和審核了嗎？ 註冊您的 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 開發人員帳戶。 免費存取Adobe PDF嵌入」，並享有其他 API 的六個月免費試用版。 試用之後，您可以 [隨即付費](https://developer.adobe.com/document-services/pricing/main) ，隨著業務成長，每份檔交易只要 \$0.05。
