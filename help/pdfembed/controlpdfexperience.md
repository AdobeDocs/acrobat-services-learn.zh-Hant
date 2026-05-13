---
title: 掌控您的 PDF 線上體驗並蒐集分析數據
description: 在這個實作教學中，你將學習如何使用 Adobe PDF 嵌入 API 來控制外觀、促進協作，並收集使用者如何與 PDF 互動的分析數據，包括在頁面上停留的時間和搜尋
feature: PDF Embed API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-7487
thumbnail: KT-7487.jpg
exl-id: 64ffdacb-d6cb-43e7-ad10-bbd8afc0dbf4
TQID: https://experienceleague.adobe.com/fsD7hB9-yEhVElQUFkM1RmUB1aVJq6C9USXa3Ru9lks
product_v2:
  - id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2:
  - id: b1809bd0-a86b-4991-8083-2e3b517fc3b8
subfeature_v2:
  - id: c4b1e8f2-d9a8-4792-b5e4-be52bd870028
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 1549
ht-degree: 0%

---

# 掌控您的 PDF 線上體驗並蒐集分析數據

你們組織會在網站上發布 PDF 嗎？ 學習如何使用 Adobe PDF 嵌入 API，來控制外觀、促進協作，並收集使用者如何與 PDF 互動的分析數據，包括在頁面上的停留時間和搜尋結果。 要開始這個四部分的實作教學，請選擇 *「開始使用 PDF 嵌入 API*」。

<table style="table-layout:fixed">
<tr>
  <td>
    <a href="controlpdfexperience.md#part1">
        <img alt="第一部分：開始使用 PDF 嵌入 API" src="assets/ControlPDFPart1_thumb.png" />
    </a>
    <div>
    <a href="controlpdfexperience.md#part1"><strong>第一部分：開始使用 PDF 嵌入 API</strong></a>
    </div>
  </td>
  <td>
    <a href="controlpdfexperience.md#part2">
        <img alt="第二部分：將 PDF 嵌入 API 加入網頁" src="assets/ControlPDFPart2_thumb.png" />
    </a>
    <div>
    <a href="controlpdfexperience.md#part2"><strong>第二部分：將 PDF 嵌入 API 加入網頁</strong></a>
    </div>
  </td>
  <td>
   <a href="controlpdfexperience.md#part3">
      <img alt="第三部分：存取分析 API" src="assets/ControlPDFPart3_thumb.png" />
   </a>
    <div>
    <a href="controlpdfexperience.md#part3"><strong>第三部分：存取分析 API</strong></a>
    </div>
  </td>
  <td>
   <a href="controlpdfexperience.md#part4">
      <img alt="第四部分：根據事件增加互動性" src="assets/ControlPDFPart4_thumb.png" />
   </a>
    <div>
    <a href="controlpdfexperience.md#part4"><strong>第四部分：根據事件增加互動性</strong></a>
    </div>
  </td>
</tr>
</table>

## 第一部分：開始使用 PDF 嵌入 API {#part1}

在第一部分，學習如何開始準備第一到第三部分所需的一切。 你會從取得 API 憑證開始。

**你需要什麼**

* 教學資源 [下載](https://github.com/benvanderberg/adobe-pdf-embed-api-tutorial)
* Adobe ID [在這裡取得](https://account.adobe.com/)
* 網頁伺服器（Node JS、PHP 等）
* 具備 HTML / JavaScript / CSS 的基本知識

**我們正在使用什麼**

* 一個基本的網頁伺服器（Node）
* Visual Studio 程式碼
* GitHub

### 取得資格認證

1. 請前往 [Adobe.io 官網](https://developer.adobe.com/)。
1. 點擊 **[!UICONTROL 「建立引人入勝的文件體驗中的了解更多]** 」。

   ![「了解更多」按鈕截圖](assets/ControlPDF_1.png)

   這會帶你到 [!DNL Adobe Acrobat Services] 首頁。

1. 在導航欄點擊 **[!UICONTROL 「開始]** 使用」。

   你會在 **「開始使用 [!DNL Acrobat Services] API」** 中看到一個選項，可以建立 **新的憑證** 或 **管理現有憑證**。

1. 點擊&#x200B;**[!UICONTROL 「建立新憑證]**」下的&#x200B;**[!UICONTROL 「開始]**」按鈕。

   ![開始按鈕截圖](assets/ControlPDF_2.png)

1. 選擇 **[!UICONTROL PDF 嵌入 API]** 單選按鈕，並在下一個視窗新增你選擇的憑證名稱和應用程式網域。

   >[!NOTE]
   >
   >這些憑證只能用於此處列出的應用程式域。 你可以使用任何你選擇的網域。

   ![憑證截圖](assets/ControlPDF_3.png)

1. 點擊 **[!UICONTROL 建立憑證]**。

   嚮導的最後一頁會提供你的客戶憑證資訊。 請保持這個視窗開啟，這樣你就可以回來複製客戶端 ID（API 金鑰）以便日後使用。

1. 點擊 **[!UICONTROL 「檢視文件]** 」即可前往詳細說明如何使用此 API 的文件。

   ![建立憑證按鈕的截圖](assets/ControlPDF_4.png)

## 第二部分：將 PDF 嵌入 API 加入網頁 {#part2}

在第二部分，你將學習如何輕鬆地將 PDF Embed API 嵌入網頁。 你將透過使用 Adobe PDF 嵌入 API 線上示範來建立我們的程式碼來完成這件事。

### 取得運動代碼

我們為你創建了程式碼供你使用。 雖然你可以使用自己的程式碼，但示範會在教學資源的脈絡中進行。 請在此[&#128279;](https://github.com/benvanderberg/adobe-pdf-embed-api-tutorial)下載範例程式碼。

1. 前往 [[!DNL Adobe Acrobat Services] 網站](https://developer.adobe.com/document-services/homepage/)。

   ![網站截圖[!DNL Adobe Acrobat Services]](assets/ControlPDF_6.png)

1. 在導覽欄點選 **[!UICONTROL API]** ，然後在下拉連結中前往 **[!UICONTROL PDF 嵌入 API]** 頁面。

   ![PDF 嵌入 API 下拉選單截圖](assets/ControlPDF_7.png)

1. 點擊 **[!UICONTROL 「試用試玩版]**」。

   會跳出一個新視窗，顯示 PDF 嵌入 API 的開發者沙盒。

   ![試玩試玩版截圖](assets/ControlPDF_8.png)

   這裡可以看到不同觀看模式的選項。

1. 點選不同的觀賞模式，分別是全視窗、尺寸容器、線上和Lightbox。

   ![觀看模式截圖](assets/ControlPDF_9.png)

1. 點選 **[!UICONTROL 「全視窗]** 觀看模式」，然後點擊 **[!UICONTROL 「自訂]** 」按鈕來切換選項的開關。

   ![自訂按鈕截圖](assets/ControlPDF_10.png)

1. 關閉 **[!UICONTROL 下載]** PDF 選項。
1. 點擊 **[!UICONTROL 「產生程式碼]** 」按鈕可查看程式碼預覽。
1. 從第一部分的客戶憑證視窗複製 **[!UICONTROL 客戶 ID]** 。

   ![客戶 ID 截圖](assets/ControlPDF_11.png)

1. 在你的程式碼編輯器中開啟 **&#x200B;**&#x200B;Web -> **[!UICONTROL resources]** -> **[!UICONTROL js]** -> dc-config.js **&#x200B;**&#x200B;檔案。

   你會看到 clientID 變數存在。

1. 把你的客戶憑證貼在雙引號之間，讓 clientID 變成你的憑證。

1. 回到開發者沙盒程式碼預覽。

1. 複製第二行包含 Adobe 腳本的行：

   ```
   <script src=https://documentccloud.adobe.com/view-sdk/main.js></script>
   ```

   ![劇本截圖](assets/ControlPDF_12.png)

1. 到你的程式碼編輯器，打開 **[!UICONTROL Web]** -> **[!UICONTROL 練習]** -> **[!UICONTROL index.html]** 檔案。

1. 將腳本程式碼貼到 `<head>` 第 18 行檔案的註解下，註解內容為： **TODO： EXERCISE 1： 插入 EMBED API SCRIPT TAG**。

   ![腳本程式碼貼上位置的截圖](assets/ControlPDF_13.png)

1. 回到開發者沙盒程式碼預覽，複製第一行包含：

   ```
   <div id="adobe-dc-view"></div>
   ```

   ![截圖顯示程式碼要複製哪裡](assets/ControlPDF_14.png)

1. 回到你的程式碼編輯器，再次打開 **[!UICONTROL 網頁]** -> **[!UICONTROL 練習]** -> **[!UICONTROL index.html]** 檔案。

1. 將程式碼貼 `<div>` 到 `<body>` 檔案第 67 行註解 **下的 TODO：練習 1：插入 PDF 嵌入 API 程式碼**。

   ![截圖顯示程式碼要貼在哪裡](assets/ControlPDF_15.png)

1. 回到開發者沙盒程式碼預覽，複製以下程式碼行數：`<script>`

   ```
   <script type="text/javascript">
       document.addEventListener("adobe_dc_view_sdk.ready",             function(){ 
           var adobeDCView = new AdobeDC.View({clientId:                     "<YOUR_CLIENT_ID>", divId: "adobe-dc-view"});
           adobeDCView.previewFile({
               content:{location: {url: "https://documentcloud.                adobe.com/view-sdk-demo/PDFs/Bodea Brochure.                    pdf"}},
               metaData:{fileName: "Bodea Brochure.pdf"}
           }, {showDownloadPDF: false});
       });
   </script>
   ```

1. 回到你的程式碼編輯器，再次打開 **[!UICONTROL 網頁]** -> **[!UICONTROL 練習]** -> **[!UICONTROL index.html]** 檔案。

1. 把程式碼貼 `<script>` 到 檔案的 in 中 `<body>` ，在第 68 行的標籤下 `<div>` 。

1. 修改同一 **index.html** 檔案的第 70 行，加入先前建立的 clientID 變數。

   ![第70行的截圖](assets/ControlPDF_16.png)

1. 修改同一 **index.html** 檔案的第 72 行，以更新 PDF 檔案的位置，以使用本地檔案。

   教學檔 **裡有 /resources/pdfs/whitepaper.pdf** 裡有一份。

1. 儲存修改過的檔案，並瀏覽 **`<your domain>`/summit21/web/exercise/** 預覽你的網站。

   你應該會在瀏覽器中看到技術白皮書以全視窗模式呈現。

## 第三部分：存取分析 API {#part3}

現在你已經成功建立一個包含 PDF 嵌入 API 呈現 PDF 的網頁，在第三部分你可以探索如何利用 JavaScript 事件來衡量分析，了解使用者如何使用 PDF。

### 尋找文件

PDF Embed API 包含許多不同的 JavaScript 事件。 你可以從 [!DNL Adobe Acrobat Services] 文件中取得它們。

1. 前往 [文件](https://developer.adobe.com/document-services/docs/overview) 網站。
1. 檢視 API 中可用的不同事件類型。 這些資料對參考很有幫助，對你未來的專案也很有幫助。

   ![參考指南截圖](assets/ControlPDF_17.png)

1. 複製網站上列出的範例程式碼。

   以此為基礎，並修改程式碼。

   ![範例程式碼的截圖](assets/ControlPDF_18.png)

   ```
   const eventOptions = {
     //Pass the PDF analytics events to receive.
      //If no event is passed in listenOn, then all PDF         analytics events will be received.
   listenOn: [ AdobeDC.View.Enum.PDFAnalyticsEvents.    PAGE_VIEW, AdobeDC.View.Enum.PDFAnalyticsEvents.DOCUMENT_DOWNLOAD],
     enablePDFAnalytics: true
   }
   
   
   adobeDCView.registerCallback(
     AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
     function(event) {
       console.log("Type " + event.type);
       console.log("Data " + event.data);
     }, eventOptions
   );
   ```

1. 找到你之前加的程式碼區塊，看起來像下面，然後在index.html **的這段程式碼**&#x200B;後面加上上面的程式碼：

   ![截圖顯示程式碼要貼在哪裡](assets/ControlPDF_19.png)

1. 在瀏覽器中載入該頁面，並開啟主控台，查看不同事件在與 PDF 檢視器互動時的主控台輸出。

   ![載入頁面的截圖](assets/ControlPDF_20.png)

   ![載入頁面的程式碼截圖](assets/ControlPDF_21.png)

### 新增捕捉事件的開關

既然你已經把事件輸出到console.log，讓我們根據事件的行為來改變。 為此，你會用一個開關的例子。

1. 請前往 **教學程式碼中的片段/eventsSwitch.js** ，複製檔案內容。

   ![截圖顯示程式碼要複製哪裡](assets/ControlPDF_22.png)

1. 把程式碼貼到事件監聽器函式裡。

   ![截圖顯示程式碼要貼在哪裡](assets/ControlPDF_23.png)

1. 確認頁面載入並操作 PDF 檢視器時，主控台是否正確輸出。

### Adobe Analytics

如果你想在檢視器中加入 Adobe Analytics 支援，可以依照網站上的說明操作。

>[!IMPORTANT]
>
>你的網頁標頭頁面上必須已經載入了 Adobe Analytics。

請前往 [Adobe Analytics 文件](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtodata.html#adobe-analytics) ，檢視你是否已在網頁上啟用 Adobe Analytics。 依照指示設定 reportSuite。

### Google Analytics

![如何整合 Google Analytics 的截圖](assets/ControlPDF_24.png)

Adobe PDF 嵌入 API 提供與 Adobe Analytics 的開箱即用整合。 然而，由於所有事件皆以 JavaScript 事件形式提供，可透過擷取 PDF 事件並使用 ga（） 函式將事件加入 Adobe Analytics 與 Google Analytics 整合。

1. 請瀏覽 **片段/eventsSwitchGA.js** ，看看如何整合 Google Analytics。
1. 如果你的網頁被 Adobe Analytics 追蹤，且已經嵌入在網頁上，請檢視並使用這段程式碼作為範例。

   ![Adobe Analytics 程式碼截圖](assets/ControlPDF_25.png)

## 第四部分：根據事件增加互動性 {#part4}

在第四部分，你將一步步說明如何在 PDF 檢視器上方疊加付費牆，該牆會在你滑過第二頁後顯示。

### 付費牆範例

請瀏覽這個 [付費牆](https://www3.technologyevaluation.com/research/white-paper/the-forrester-wave-digital-decisioning-platforms-q4-2020.html)後的PDF範例。 在這個範例中，你會學到如何在 PDF 瀏覽體驗之上加入互動性。

### 新增付費牆代碼

1. 到 snippets/paywallCode.html 複製內容。
1. 在 exercise/index.html 中搜尋。`<!-- TODO: EXERCISE 3: INSERT PAYWALL CODE -->`

   ![截圖顯示程式碼要複製哪裡](assets/ControlPDF_26.png)

1. 把複製的程式碼貼到註解後面。
1. 到 **片段/paywallCode.js** 複製內容。

   ![截圖顯示程式碼要貼在哪裡](assets/ControlPDF_27.png)

1. 把程式碼貼到那個位置。

### 試試付費牆的試玩版

現在你可以觀看試玩版。

1. 在你的網站上重新index.html **。**
1. 往下捲動到> 2.
1. 在第二頁後，顯示對話框會顯示給挑戰使用者。

   ![觀看示範的截圖](assets/ControlPDF_28.png)

## 其他資源

更多資源可在此[&#128279;](https://developer.adobe.com/document-services/docs/overview)處找到。
