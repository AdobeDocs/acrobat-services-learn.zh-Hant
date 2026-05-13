---
title: 在 Microsoft Power Automate 建立你的第一個工作流程
description: 學習如何在 Microsoft Power Automate 中使用 Adobe PDF Services 連接器
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-10379
thumbnail: KT-10379.jpg
exl-id: 095b705f-c380-42cc-9329-44ef7de655ee
TQID: https://experienceleague.adobe.com/xltwAkEl5vPjcTGB1YX1VSC02fIVDWK7nElLTbiMkHo
product_v2:
  - id: acdc2bde-2937-4877-90d9-031dd66278c9
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 2046
ht-degree: 1%

---

# 在 Microsoft Power Automate 建立你的第一個流程

了解如何使用 [Adobe PDF Services](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/) 連接器在 [Microsoft Power Automate](https://flow.microsoft.com/zh-tw/) 建立您的第一個流程。

在這個實作教學中，學習如何：

* 將 Word 文件轉換成 PDF
* 將 PDF 文件合併成一份 PDF
* 用密碼保護 PDF 文件

## 製作過程

### 你需要什麼

* **Adobe PDF 服務的試用或製作憑證**&#x200B;在這裡了解更多如何在 Microsoft Power Automate [&#128279;](https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/pdfservices/getting-credentials-power-automate)中取得及設定憑證。
* **Microsoft Power Automate with Premium connectors**&#x200B;在這裡了解如何查詢 Power Automate [&#128279;](https://docs.microsoft.com/en-us/power-platform/admin/power-automate-licensing/types)的授權等級。
* **OneDrive**&#x200B;本教學使用了 OneDrive 儲存接頭，但任何儲存接頭都可以替代。

### 範例檔案

你需要 [解壓並上傳到OneDrive的兩個範例檔案](assets/sample-assets.zip) ：

* WordDocument01.docx
* WordDocument02.docx

### 取得資格認證

要完成本教學，你需要在 Microsoft Power Automate for Adobe PDF 服務中預先設定好你的憑證。 如果你還沒完成這個步驟，請參考 [這裡](https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/pdfservices/getting-credentials-power-automate)的說明。

## 第一部分：建立新流程並轉換 Word 至 PDF

### 創造流程

在這個部分，你會在 [Microsoft Power Automate](https://flow.microsoft.com/zh-tw/) 使用即時流程建立新流程，加入參數，從 OneDrive 取得檔案，並轉檔成 PDF。

1. 進入 [Microsoft Power Automate](https://flow.microsoft.com/zh-tw/) ，並用你的帳號登入。
1. 在側邊欄，選擇 **[!UICONTROL 「建立]**」。

   ![建立按鈕](assets/createButtonPowerAutomate.png)

1. 選擇 **[!UICONTROL 即時流量]**。
1. 給你的流程取個名字。
1. 在「選擇如何觸發此流程」中&#x200B;*，選擇&#x200B;**[!UICONTROL 手動觸發流程]**。*
1. 選取「**[!UICONTROL 建立]**」。

### 取得檔案內容

接著，取得範例檔案的檔案內容。

>[!PREREQUISITES]
>
>如果你還沒把 [範例檔案](assets/sample-assets.zip) 上傳到 OneDrive，請解壓縮後再上傳。


1. 在 Power Automate 中[，選擇 **[!UICONTROL + 新步驟]**。](https://flow.microsoft.com/zh-tw/)
1. 在搜尋欄搜尋 *OneDrive* 。
1. 選擇你的工作或個人 OneDrive 帳號，請選擇 **[!UICONTROL OneDrive for Business]** 或 **[!UICONTROL OneDrive]**。
1. 在搜尋欄搜尋「 *取得檔案內容* 」。
1. 在 **[!UICONTROL 檔案]** 欄位，選擇資料夾圖示，前往 *OneDrive 的WordDocument01.docx* 檔案。

   ![取得 Microsoft Power Automate 中的 OneDrive 動作檔案內容](assets/getFileContentOneDrive.png)

### 將檔案轉換成 PDF

現在你已經有了檔案內容，就可以將文件轉成 PDF。

1. 在 Power Automate 中[，選擇 **[!UICONTROL + 新步驟]**。](https://flow.microsoft.com/zh-tw/)
1. 在搜尋欄搜尋 *Adobe PDF 服務* 。
1. 選擇 **[!UICONTROL Adobe PDF 服務]**。
1. 在搜尋欄搜尋「 *Convert Word to PDF* 」。
1. 在檔案名稱&#x200B;**中**，依照你的設定名稱，但必須以 *.docx* 結尾。此擴充功能是將文件從 Word 轉換成 PDF 所必需的。
1. 將游標置於「 **[!UICONTROL 檔案內容]** 」欄位。
1. 使用 **[!UICONTROL 動態內容]** 面板，選擇 **[!UICONTROL 檔案內容]**。

   ![在 Microsoft Power Automate 中將 Word 轉換成 PDF 動作](assets/convertWordToPDFActionPowerAutomate.png)

### 將檔案儲存到 OneDrive

文件產生後，將檔案存回 OneDrive。

1. 在 [Microsoft Power Automate](https://flow.microsoft.com/zh-tw/) 中，選擇 **[!UICONTROL + 新步驟]**。
1. 在搜尋欄搜尋 *OneDrive* 。
1. 選擇你的工作或個人 OneDrive 帳號，請選擇 **[!UICONTROL OneDrive for Business]** 或 **[!UICONTROL OneDrive]**。
1. 在搜尋欄搜尋「 *取得檔案內容* 」。
1. 在搜尋欄搜尋「 *建立檔案* 」即可。
1. 選擇 **[!UICONTROL 建立檔案]**。
1. 在 **[!UICONTROL 資料夾路徑]** 欄位，選擇資料夾圖示，指定檔案在 OneDrive 中儲存的位置。
1. 在檔案名稱&#x200B;**中**，依照你的設定名稱，但必須以 *.docx* 結尾。此擴充功能是將文件從 Word 轉換成 PDF 所必需的。
1. 在 **[!UICONTROL 檔案內容]** 欄位，使用 **[!UICONTROL 動態內容]** 面板插入 PDF 檔案內容變數。

### 試試 flow

1. 在左上角選擇 **[!UICONTROL 「Untitled]** 」以重新命名流程。
1. 選取「**[!UICONTROL 儲存]**」。
1. 選擇 **[!UICONTROL 測試]**。
1. 選擇&#x200B;**[!UICONTROL 手動，然後**&#x200B;[!UICONTROL &#x200B;選擇儲存與測試&#x200B;]&#x200B;**]**。
1. 選取「**[!UICONTROL 繼續]**」。
1. 選擇 **[!UICONTROL 「運行流程]**」。

在 OneDrive 資料夾裡，你現在應該會看到已轉換的 PDF。

![OneDrive 中選取的轉換 PDF 文件](assets/selectedGeneratedFileInOneDrive.png)

## 第二部分：從範本產生動態文件

接下來的部分是建立在第一部分的基礎上，使用 *「從 Word* 產生文件」範本，將資料動態合併到你的文件中。

### 檢視文件範本

從 OneDrive 的範例檔案中開啟 *WordDocument02_.docx* 。 Word 文件包含多個不同的文字標籤，代表資料被填充到文件中的位置。

### 新增參數到 trigger

要讓動態資料推入文件，你需要為觸發器建立幾個參數，讓它能提示輸入數值。

1. 編輯流程時，選擇 **[!UICONTROL 手動觸發流程]** 以展開動作。
1. 選擇 **[!UICONTROL 新增輸入]**。
1. 選擇 **[!UICONTROL 文字]**。
1. 把欄位 *命名為 First Name*。

重複步驟 2-4 以加入以下欄位：

* 姓氏
* 薪資

![Power Automate 中的參數欄位觸發器](assets/triggerParametersInPowerAutomate.png)

### 取得範本的檔案內容

要產生文件，首先需要取得 Word 範本的檔案內容。

1. 在 Power Automate 中，選擇 + **[!UICONTROL 新步驟]**。
1. 在搜尋欄搜尋 *OneDrive* 。
1. 選擇你的工作或個人 OneDrive 帳號，請選擇 **[!UICONTROL OneDrive for Business]** 或 **[!UICONTROL OneDrive]**。
1. 在搜尋欄搜尋「 *取得檔案內容* 」。
1. 在檔案&#x200B;**&#x200B;**&#x200B;欄位，選擇資料夾圖示，前往 *OneDrive 的WordDocument02.docx*&#x200B;檔案。

![在 Microsoft Power Automate 中從 OneDrive 取得檔案內容動作](assets/getFileContentAction02.png)

### 從範本產生文件

1. 在 Power Automate 中，選擇 **[!UICONTROL + 新步驟]**。
1. 在搜尋欄搜尋 *Adobe PDF 服務* 。
1. 選擇 **[!UICONTROL Adobe PDF 服務]**。
1. 選擇 **[!UICONTROL 「從 Word 範本]** 產生文件」動作。
1. 在 **[!UICONTROL 範本檔名]** 欄位，請依照喜好命名你的檔案，但必須以 *.docx* 結尾。

#### 合併資料

透過 *「從 Word 範本* 產生文件」動作，你可以將流程中先前不同變數的資料合併到文件中，使用動態內容。

請將下方的 JSON 資料複製到 **Merge Data** 欄位：

```
{
    "FirstName": "",
    "LastName": "",
    "Salary": ""
}
```

1. 將游標放在 FirstName *值兩個引號*&#x200B;之間的欄位。
1. 透過 **[!UICONTROL 動態內容]** 面板，從手動觸發流程動作中插入 *First Name* 值。

   ![以 JSON 產生帶有資料標籤的文件](assets/generateDocumentJSONAction.png)

1. 重複第 7-8 步，針對 **[!UICONTROL 姓氏]** 和 **[!UICONTROL 薪資]** 欄位。
1. 在&#x200B;**[!UICONTROL 範本檔案內容]**&#x200B;欄位，使用&#x200B;**[!UICONTROL 動態內容]**&#x200B;面板插入&#x200B;**[!UICONTROL 「取得檔案內容&#x200B;*」步驟中的*檔案內容]**&#x200B;值。

![在 Power Automate 中從 Word 範本產生文件，動作中所有值都已完成](assets/generateDocumentJSONActionCompleted.png)

>[!TIP]
>
>*從 Word 範本*&#x200B;產生文件的動作使用 Adobe 文件生成 API。如果你想了解更多如何建立範本，這裡有一些資源：
>
>* [了解更多關於 Adobe 文件生成的資訊](https://developer.adobe.com/document-services/apis/doc-generation/)
>* [Microsoft Word 的 Adobe 文件產生標註器](https://appsource.microsoft.com/en-US/product/office/WA200002654)
>* [Adobe 文件產生 API 文件](https://developer.adobe.com/document-services/docs/overview/document-generation-api/)

### 將檔案儲存到 OneDrive

文件產生後，你可以把檔案存回 OneDrive。

1. 在 Power Automate 中，選擇 **+ [!UICONTROL 新步驟]**。
1. 在搜尋欄搜尋 *OneDrive* 。
1. 選擇你的工作或個人 OneDrive 帳號，請選擇 **[!UICONTROL OneDrive for Business]** 或 **[!UICONTROL OneDrive]**。
1. 在搜尋欄搜尋「 *建立檔案* 」即可。
1. 選擇 **[!UICONTROL 建立檔案]**。
1. 在 **[!UICONTROL 資料夾路徑]** 欄位，選擇資料夾圖示，指定檔案在 OneDrive 中儲存的位置。
1. 在檔案 **[!UICONTROL 名稱]** 欄位中，設定檔案名稱。 因為輸出是 PDF，檔案名稱必須以副檔名結尾.pdf。
1. 請使用 **[!UICONTROL 動態內容]** 面板將 PDF 檔案內容變數插入 **[!UICONTROL 檔案內容]** 欄位。

### 試試 flow

![在 Microsoft Power Automate 中執行流程畫面，提示輸入](assets/runFlowParameters.png)

1. 選取「**[!UICONTROL 儲存]**」。
1. 選擇 **[!UICONTROL 測試]**。
1. 選擇&#x200B;**[!UICONTROL 手動，然後**&#x200B;[!UICONTROL &#x200B;選擇儲存與測試&#x200B;]&#x200B;**]**。
1. 選取「**[!UICONTROL 繼續]**」。
1. 輸入名字&#x200B;*、姓氏*&#x200B;和&#x200B;*薪資*&#x200B;的數值&#x200B;**。
1. 選擇 **[!UICONTROL 「運行流程]**」。

在 OneDrive 資料夾中，你現在會看到從 Word 文件產生的 PDF。 當你在 OneDrive 開啟 PDF 文件時，你會看到資料已經合併到文字標籤的位置。


## 第三部分：將 PDF 合併成一個

現在你已經將 Word 文件產生並轉換成 PDF，接下來就是將多個 PDF 文件合併在一起。

>[!NOTE]
>
>在之前的操作中，你將文件的副本存為 OneDrive 檔案。 要使用像 Merge PDF 這類工具，你不需要將檔案儲存到 OneDrive。 相反地，你可以直接將輸出從一個動作傳遞到下一個，這比每次操作後存到 OneDrive 要好得多。 但為了示範，你是把這些檔案儲存到 OneDrive。

### 新增合併 PDF 步驟

1. 編輯流程時，選擇 **[!UICONTROL + 下一步]** ，在流程結束時新增一個動作。
1. 在搜尋欄搜尋 *Adobe PDF 服務* 。
1. 選擇 **[!UICONTROL Adobe PDF 服務]**。
1. 選擇 **[!UICONTROL 合併 PDF]** 的操作。
1. 在&#x200B;**[!UICONTROL 合併 PDF 檔案名稱]**&#x200B;欄位，輸入你想要的檔案名稱（例如 CombinedDocument.pdf **）。
1. 在&#x200B;**[!UICONTROL 檔案內容 -1]** 欄位中，使用&#x200B;**[!UICONTROL 動態內容]**&#x200B;面板插入 *Convert Word to PDF **步驟中的**&#x200B;PDF 檔案內容值*。
1. 要新增下一份文件，請選擇 **+ [!UICONTROL 新增項目]**。
1. 在「**[!UICONTROL 檔案內容 - 2]**」欄位，使用&#x200B;**[!UICONTROL 動態內容]**&#x200B;面板插入&#x200B;**[!UICONTROL 「從 Word 範本&#x200B;*產生文件」步驟中的*輸出檔案內容]**&#x200B;值。

![在 Microsoft Power Automate 中合併 PDF 動作](assets/mergePDFAction.png)

### 將合併後的 PDF 儲存到 OneDrive

文件合併後，你可以把文件存回 OneDrive。

1. 在 Power Automate 中，選擇 **+ [!UICONTROL 新步驟]**。
1. 在搜尋欄搜尋 *OneDrive* 。
1. 選擇你的工作或個人 OneDrive 帳號，請選擇 **[!UICONTROL OneDrive for Business]** 或 **[!UICONTROL OneDrive]**。
1. 在搜尋欄搜尋「 *建立檔案* 」即可。
1. 選擇 **[!UICONTROL 建立檔案]**。
1. 在 **[!UICONTROL 資料夾路徑]** 欄位，選擇資料夾圖示，指定檔案在 OneDrive 中儲存的位置。
1. 在檔案 **[!UICONTROL 名稱]** 欄位中，設定檔案名稱。 因為輸出是 PDF，檔案名稱必須以 .pdf 結尾。
1. 在&#x200B;**[!UICONTROL 檔案內容]**&#x200B;欄位，使用&#x200B;**[!UICONTROL 動態內容]**&#x200B;面板插入&#x200B;*合併 PDF **步驟中的**&#x200B;PDF 檔案內容*&#x200B;值。

   ![Microsoft Power Automate 的流程概覽](assets/flowOverviewSavedMergedDocument.png)

### 試試 flow

1. 選取「**[!UICONTROL 儲存]**」。
1. 選擇 **[!UICONTROL 測試]**。
1. 選擇&#x200B;**[!UICONTROL 手動，然後**&#x200B;[!UICONTROL &#x200B;選擇儲存與測試&#x200B;]&#x200B;**]**。
1. 選取「**[!UICONTROL 繼續]**」。
1. 輸入名字&#x200B;*、姓氏*&#x200B;和&#x200B;*薪資*&#x200B;的數值&#x200B;**。
1. 選擇 **[!UICONTROL 「運行流程]**」。

在 OneDrive 資料夾中，你會看到合併後的 PDF 與第一、第二份文件的頁面。

## 第四部分：保護 PDF 文件

建立文件後，你可以在儲存到 OneDrive 前加入額外步驟，保護它不會被編輯。

### 保護 PDF

1. 在 Power Automate 編輯流程時，請在合併 PDF 動作與&#x200B;**[!UICONTROL 建立檔案 3]** 動作之間&#x200B;**[!UICONTROL 選擇**+**。]**

   ![加上兩個動作之間的符號，以增加一個新的動作](assets/addActionToProtect.png)

1. 選擇 **[!UICONTROL 新增一個動作]**。
1. 在搜尋欄搜尋 *Adobe PDF 服務* 。
1. 選擇 **[!UICONTROL Adobe PDF 服務]**。
1. 選擇 **[!UICONTROL 「從檢視]** 中保護 PDF」的動作。
1. 在檔案 **[!UICONTROL 名稱]** 欄位，將名稱設為你想要的名稱，只要以.pdf副檔名結尾即可。
1. 將密碼&#x200B;**欄位設**&#x200B;為你指定的密碼以開啟文件。
1. 在&#x200B;**[!UICONTROL 檔案內容]**&#x200B;欄位，使用&#x200B;**[!UICONTROL 動態內容]**&#x200B;面板插入&#x200B;*合併 PDF **步驟中的**&#x200B;PDF 檔案內容*&#x200B;值。

### 更新存檔到 OneDrive

文件受保護後，你可以把檔案存回 OneDrive。 在這個例子中，你是用新的&#x200B;*檔案內容*&#x200B;值更新了先前的&#x200B;**「建立檔案 3**」動作。

1. 在「**[!UICONTROL 建立檔案 3]**」動作的&#x200B;**[!UICONTROL 檔案內容]**&#x200B;欄位選擇游標。
1. 使用&#x200B;**[!UICONTROL 動態內容]**&#x200B;面板插入&#x200B;*「保護 PDF 從檢視&#x200B;**中」步驟中**&#x200B;取得的 PDF 檔案內容*&#x200B;值。

### 試試 flow

1. 選取「**[!UICONTROL 儲存]**」。
1. 選擇 **[!UICONTROL 測試]**。
1. 選擇&#x200B;**[!UICONTROL 手動，然後**&#x200B;[!UICONTROL &#x200B;選擇儲存與測試&#x200B;]&#x200B;**]**。
1. 選取「**[!UICONTROL 繼續]**」。
1. 輸入名字&#x200B;*、姓氏*&#x200B;和&#x200B;*薪資*&#x200B;的數值&#x200B;**。
1. 選擇 **[!UICONTROL 「運行流程]**」。

在 OneDrive 資料夾中，你會看到合併後的 PDF，現在會提示你輸入密碼才能查看文件。

## 下一步

在這個教學中，你將 Word 文件轉換成 PDF，根據資料產生文件，將文件合併，並用密碼保護。 欲了解更多，請探索 Microsoft Power Automate 中 Adobe PDF 服務連接器中可用的其他操作：

* 查看 Microsoft Power Automate 中可用的預先建立範本。
* 請參考 [Adobe Tech Blog 上的文章](https://medium.com/adobetech/tagged/microsoft-power-automate) 。
* 請檢視 [&#128279;](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) Adobe 文件生成 API 的文件。
