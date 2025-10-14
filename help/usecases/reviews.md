---
title: 審閱和批准
description: 瞭解如何構建文檔審閱和批准工作流以實現跨團隊協作
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8094
thumbnail: KT-8094.jpg
exl-id: d704620f-d06a-4714-9d09-3624ac0fcd3a
source-git-commit: b7a20f30a2eb175053c7a25be0411f80dd88899f
workflow-type: tm+mt
source-wordcount: '1540'
ht-degree: 0%

---

# 審查和批准

![使用案例英雄橫幅](assets/UseCaseReviewsHero.jpg)

在COVID-19大流行期間，許多公司都需要遠程跨團隊協作，[共用和審閱數字文檔](https://developer.adobe.com/document-services/use-cases/collaboration/review-and-approval)為團隊和跨職能資源帶來了一系列挑戰。

這些挑戰包括以不同的檔案格式共用文檔、有效地查看和注釋內容，以及與最近的編輯進行同步。 [!DNL Adobe Acrobat Services]個API旨在使應用程式開發人員能夠為其用戶解決這些難題。

## 你能學到的

本操作教程介紹如何在Node.js和Express Web應用程式中構建文檔審閱和批准工作流。 要繼續學完本教程，您需要一些Node.js的經驗。

該應用程式具有以下功能：

* 將不同的檔案類型轉換為PDF

* 啟用檔案上載

* 使用戶能夠添加註釋和注釋

* 顯示PDF和這些注釋

* 啟用用戶配置檔案以標識注釋作者

* 將檔案合併到用戶可下載的最終PDF中

## 相關API和資源

* [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [項目代碼](https://github.com/contentlab-io/adobe_reviews_and_approvals)

## 建立AdobeAPI憑據

在啟動代碼之前，必須[為Adobe PDF嵌入API和Adobe PDF服務API建立憑據](https://www.adobe.com/go/dcsdks_credentials)。 PDF嵌入API可免費使用。 PDF服務API可免費使用6個月，然後您可以以每個文檔交易$0.05的價格切換到[按使用付費計畫](https://developer.adobe.com/document-services/pricing/main)。

為PDF服務API建立憑據時，請選擇&#x200B;**建立個性化代碼示例**&#x200B;選項，然後為語言選擇Node.js。 保存ZIP檔案，並將pdftools-api-credentials.json和private.key解壓到Node.js Express項目的根目錄。

## 設定項目和依賴項

設定Node.js和Express項目，以從名為「public」的資料夾中提供靜態檔案。 您可以根據首選項設定項目方式。 若要快速啟動並運行，可以使用[Express應用生成器](https://expressjs.com/en/starter/generator.html)。 或者，如果希望保持簡單，可以[從頭開始](https://expressjs.com/en/starter/hello-world.html)，並將代碼保留在單個JavaScript檔案中。 在上面連結的示例項目中，您正在使用單檔案方法，並將所有代碼保留在index.js中。

將`pdftools-api-credentials.json`和`private.key`檔案從個性化代碼示例複製到項目的根目錄。 另外，如果您有.gitignore檔案，請將其添加到該檔案中，這樣您的憑據檔案就不會意外發送到儲存庫。

接下來，運行`npm install @adobe/documentservices-pdftools-node-sdk`以安裝Node.js SDK，用於PDF服務。 導入此模組並在代碼（示例項目中的index.js）內建立API憑據對象，其餘依賴項導入如下：

```
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();
```

您的起始代碼應如下所示：

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

現在，您已準備好使用[!DNL Acrobat Services]個API。

## 將檔案轉換為PDF

對於文檔工作流的第一部分，最終用戶必須上載要共用的文檔。 要啟用此功能，請添加一個上載函式，並將不同的文檔檔案格式合併到PDF中，以為審閱過程做好準備。

首先建立函式，以根據PDF服務API[的](https://developer.adobe.com/document-services/apis/pdf-services)示例代碼段將文檔轉換為PDF。 此示例還顯示了許多其他重要功能的代碼段，包括光學字元識別(OCR)、密碼保護和刪除以及壓縮。

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

現在，您可以使用此函式從上載的文檔建立PDF。

## 處理檔案上載

接下來，伺服器需要Web伺服器上的檔案上載終結點來接收和處理文檔。

首先，在上載資料夾內建立一個資料夾並將其命名為「草稿」。 您將上載的檔案和轉換的PDF檔案儲存在此處。 接下來，運行`npm install express-fileupload`以安裝Express-FileUpload模組，並在代碼中將中間件添加到Express中：

```
const fileUpload = require( "express-fileupload" );
app.use( fileUpload() );
```

現在，添加`/upload`終結點，並使用相同的檔案名將上載的檔案保存在草稿資料夾中。 然後，調用您以前編寫的函式以建立同一文檔的PDF檔案(如果該文檔尚未採用PDF格式)。 您可以根據原始上載文檔的名稱為新PDF檔案生成檔案名：

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

## 建立上載頁

現在，要從Web應用程式上載檔案，請在上載資料夾內建立`index.html`個網頁。 在該頁上，添加一個檔案上載表單，該表單將檔案發送到/upload終結點：

```
<form ref="uploadForm" 
      action="/upload"
      method="post" 
      encType="multipart/form-data">
      <input type="file" name="myFile" accept=".doc,.docx,.ppt,.pptx,.xls,.xlsx,.txt,.rtf,.bmp,.jpg,.gif,.tiff,.png">
      <input type="submit" value="Upload File" />
  </form>
```

![網頁上上載檔案功能的螢幕快照](assets/reviews_1.png)

現在，您可以將文檔上載到Node.js伺服器。 伺服器將檔案保存在上載/草稿資料夾內，並在其旁邊建立PDF格式版本。

您現在已準備好嵌入已上載的文檔，因此使用PDF嵌入API使用戶能夠輕鬆地向文檔添加註釋和注釋。

## 枚舉PDF檔案

由於典型的文檔工作流可能涉及多個文檔，因此您必須顯示文檔清單並將每個文檔連結到應用程式中的新文檔審閱頁面。

首先，在伺服器代碼內添加一個/files終結點，該終結點獲取並返回儲存在上載/草稿資料夾中的所有PDF檔案的清單：

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

添加`/download/:file`路由，該路由提供對上載的PDF檔案的訪問，以嵌入到網頁中。

>[!NOTE]
>
>在生產應用程式中，必須添加身份驗證和授權以確保請求來自有效用戶，並且允許用戶訪問文檔。

```
app.get( "/download/:file", function( req, res ){
    // Note: In production code, this should check authentication and user access permissions
    res.download( __dirname + "/uploads/drafts/" + req.params[ "file" ] );
});
```

使用在載入時填充的檔案清單元素更新index.html頁。 每個項目都可以連結到draft.html網頁，並且您使用查詢字串參數將檔案名傳遞到頁面。

>[!NOTE]
>
>您使用jQuery追加每個項，因此必須將jQuery庫載入到網頁上，或使用其他方法追加元素。

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

![選擇要查看的檔案的螢幕快照](assets/reviews_2.png)

## 嵌入PDF

您已準備好在Web應用程式中嵌入和顯示PDF檔案。

建立名為&quot;draft.html&quot;的網頁，並在頁面上為嵌入式PDF添加div元素：

```
  <div id="adobe-dc-view"></div>
```

包括[!DNL Acrobat Services]庫：

```
  <script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

在自定義指令碼標籤內，從查詢字串參數中分析檔案名，以便知道要在頁面上嵌入哪個檔案：

```
  <script type="text/javascript">
          let params = new URLSearchParams( window.location.search );
          let filename = params.get( "file" );
  </script>
```

為adobe_dc_view_sdk.ready事件添加文檔事件偵聽器，該事件將指定的PDF檔案載入到div元素內的嵌入式視圖中。 使用PDF嵌入API憑據中的客戶端ID。 您要啟用注釋和注釋，因此將視圖嵌入到FULL_WINDOW模式中，並將showAnnotationsTools選項設定為true。

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

## 建立用戶配置檔案

預設情況下，注釋和注釋在此視圖中顯示為「來賓」。 通過在頁面代碼中將用戶配置檔案回調註冊到PDF視圖，可以設定注釋和注釋的當前審閱者名稱。 下面是一個示例配置檔案。 在包括用戶驗證的完整應用程式中，可以以這種方式設定登錄的用戶會話的配置檔案資訊，以標識審閱工作流中文檔的每個評論者。

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

當您查看並注釋使用此網頁上載的任何文檔時，您的配置檔案會將您標識為特定用戶。

## 保存文檔反饋

在用戶對文檔發表評論後，他們按一下&#x200B;**「保存」。**&#x200B;預設情況下，按一下&#x200B;**保存**&#x200B;將下載更新的PDF檔案。 更改此操作以更新伺服器上的當前PDF檔案。

將`/save`終結點添加到在上載/草稿資料夾中覆蓋PDF檔案的伺服器代碼：

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

為將內容上載到/save終結點的SAVE_API註冊PDF視圖回調。 如果需要，可以更改autoSaveFrequency值，使應用程式能夠自動將更改保存在計時器上，並在完成時將附加元資料包括到當前嵌入的檔案。

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

草稿文檔上的注釋和注釋現在保存在伺服器上。 您可以[瞭解有關回調在工作流中的適用情況的詳細資訊](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#callbacks-workflows)。 例如，[狀態回調](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#status-callback)幫助處理檔案衝突，如果多個人想同時查看和評論同一文檔。

在最後一步中，您使用PDF服務API將所有編輯的文檔合併到一個PDF檔案中。

## 組合PDF檔案

PDF組合代碼與PDF建立代碼類似，但使用CombineFiles操作並將每個檔案添加為輸入。

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

## 下載最終PDF

添加名為/finalize的終結點，該終結點調用函式以將`uploads/drafts`資料夾內的所有PDF檔案合併到`Final.pdf`檔案中，然後下載。

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

最後，在主index.html網頁中向此/finalize終結點添加一個連結。 此連結使用戶能夠下載文檔工作流的結果。

```
<a href="/finalize">Download final PDF</a>
```

![下載最終文檔的螢幕快照](assets/reviews_3.png)

## 後續步驟

此操作教程展示了[!DNL Acrobat Services]個API如何將[文檔共用和審閱工作流](https://developer.adobe.com/document-services/use-cases/collaboration/review-and-approval)整合到Web應用程式中。 該應用程式允許遠程員工共用檔案並與其同事協作，這對在家工作的員工和承包商特別有幫助。

您可以使用這些技術在應用中啟用協作，或瀏覽[PDF服務節點SDK示例](https://github.com/adobe/pdftools-node-sdk-samples)和[在GitHub上PDF嵌入API示例](https://github.com/adobe/pdf-embed-api-samples)，以瞭解如何使用Adobe的API。

是否準備好在您自己的應用中啟用文檔共用和審閱？ 註冊您的[[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)開發人員帳戶。 免費訪問Adobe PDF嵌入，享受6個月的免費試用期。 試用後，隨著業務增長，您可以[按單價](https://developer.adobe.com/document-services/pricing/main)，每個文檔交易僅為0.05美元。
