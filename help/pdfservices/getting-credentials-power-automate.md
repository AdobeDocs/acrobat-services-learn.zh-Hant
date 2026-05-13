---
title: 取得 Microsoft Power Automate 的憑證
description: 了解如何取得憑證以開始使用或試用 Adobe PDF 服務
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-10382
thumbnail: KT-10382.jpg
exl-id: 68ec654f-74aa-41b7-9103-44df13402032
TQID: https://experienceleague.adobe.com/NagNLc23IZyxJtLrW-Ig3-r38gqNECoQq2Pn2ZdxKC8
product_v2: id: acdc2bde-2937-4877-90d9-031dd66278c9
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 930
ht-degree: 2%

---

# 取得 Microsoft Power Automate 的憑證

[Microsoft Power Automate](https://powerautomate.microsoft.com/) 為公民開發者和開發者提供了一種強大的自動化流程，讓他們能在不寫程式碼的情況下，建立強大的自動化流程來改善他們的業務。 [Adobe [[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services)PDF Services](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/) 連接器作為 的一部分，允許使用者執行 Microsoft Power Automate 中 Adobe PDF Services API 中提供的任何動作。

在這個教學中，學習如何取得憑證以開始使用或試用 Adobe PDF 服務。 根據你是試用用戶還是現有客戶，這個教學會一步步說明如何取得憑證。

## Microsoft Power Automate 使用者該如何開始使用 Adobe PDF 服務連接器？

現有的 Microsoft Power Automate 使用者可以 [取得 Adobe PDF 服務的試用憑證](https://www.adobe.com/go/powerautomate_getstarted_tw) 。 上述連結是專為 Microsoft Power Automate 使用者提供此流程的特別註冊連結。

![Adobe 開發者使用者登入](assets/credentials_1.png)


>[!IMPORTANT]
> 如果你是為了試用登入，必須使用 Adobe ID，而不是企業 ID。 如果您目前不是 Adobe PDF Services API 的訂閱者，且嘗試以您的企業 ID 登入，可能會因為您的企業未授權您使用 Adobe PDF Services API 而出現權限錯誤。 因此，建議您使用免費的個人 Adobe ID。
>

1. 登入後，系統會提示您選擇新憑證名稱。 請輸入您的 *證件姓名*。
1. 選擇勾選方塊以同意開發者條款。
1. 選擇 **[!UICONTROL 建立憑證]**。

   ![說明你的資歷](assets/credentials_2.png)

這些資格涵蓋五項不同的價值：

* 用戶端 ID (API 金鑰)
* 用戶端密碼
* 組織 ID
* 技術帳戶 ID
* Base64（編碼私鑰）

![新資格](assets/credentials_3.png)

包含所有這些數值的 JSON 檔案也會自動下載到你的系統。 這個檔案的名稱 `pdfservices-api-pa-credentials.json` 和外觀如下：

```json
{
 "client_id": "client id value",
 "client_secret": "client secret value",
 "organization_id": "organized id value",
 "account_id": "account id value",
 "base64_encoded_private_key": "base64 version of the private key"
}
```

此檔案存放於安全位置，因為無法再次取得私鑰副本。

### 在 Microsoft Power Automate 中新增連線

現在你有了憑證，就可以開始在 Microsoft Power Automate 流程中使用它們。

1. 在側邊欄選單中，打開&#x200B;**[!UICONTROL 資料]**&#x200B;選單並選擇&#x200B;**連接：**

   ![Microsoft Power Automate 網站中的連線選單](assets/credentials_4.png)

1. 選擇 **+ [!UICONTROL 新連線]**。

1. 下一個畫面顯示可能的連線類型清單。 在右上角輸入「adobe」以篩選選項：

   ![Adobe 關聯列表](assets/credentials_5.png)

1. 選擇 **[!UICONTROL Adobe PDF 服務（預覽）。]**
1. 在模態視窗中，輸入你之前產生的五個數值。 完成後選擇 **[!UICONTROL 建立]** 。

   ![表格欄位用以輸入憑證資訊](assets/credentials_6.png)

您現在已準備好在 Microsoft Power Automate 中使用 Adobe PDF 服務。

### 憑證建立後的存取

如果你已經建立了憑證，卻遺失了下載的憑證，你可以在 [Adobe 開發者控制台](https://developer.adobe.com/console)重新找回。

1. 登入 [Adobe 開發者主控台](https://developer.adobe.com/console)後，先找到你的專案並選擇它。
1. 在左側選單的&#x200B;*憑證*&#x200B;中，選擇&#x200B;**服務帳戶（JWT）：**

   ![現有資格](assets/credentials_7.png)

1. 請注意此處呈現的五個值： *客戶識別碼*、 *客戶秘密*、 *技術帳戶識別碼*、 *技術帳戶電子郵件*&#x200B;及 *組織識別碼*。

很遺憾，你無法下載之前的私鑰，但你可以使用「產生公私密金鑰對」按鈕來建立新的。

## 使用現有的 Adobe PDF 服務憑證

如果你已有網站 [!DNL Adobe Acrobat Services] 產生的 Adobe PDF Services API 憑證，你可以搭配 Microsoft Power Automate 使用。 如果你在註冊時下載了 SDK，你現有的憑證通常會以一個名為 `pdfservices-api-credentials.json`的 JSON 檔案的形式出現。 那個 JSON 檔案包含建立連線憑證所需的五個鍵。 將每個 JSON 檔案的值複製到對應的連線欄位。

你的私鑰值來自另一個名為 `private.key`的檔案。

你也可以像上面描述的 Adobe 開發者控制台取得這些數值。

## 使用者該 [!DNL Adobe Acrobat Services] 如何開始使用 Microsoft Power Automate？

要開始使用 Power Automate，請先點擊 <https://powerautomate.microsoft.com> 「免費開始」按鈕。 如果你沒有 Microsoft 帳號，就需要自己建立一個。 登入後，您會看到 Power Automate 儀表板。

![PA 儀表板，初始視圖](assets/credentials_8.png)

如本教學開頭所述，建立一個新流程，新增步驟，並找到 Adobe PDF 服務。 選擇一個行動，可能會提醒您需要使用高級帳戶。

![高級帳號警告](assets/credentials_9.png)

如上方截圖所示，你可以切換到工作帳號或建立新的組織帳號。 完成後，你就可以新增 Adobe PDF 服務的操作。

想深入了解如何建立您的第一個 Microsoft Power Automate 流程 [!DNL Adobe Acrobat Services]，請參閱 [「在 Microsoft Power Automate](https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/pdfservices/create-workflow-power-automate) 建立您的第一個工作流程」。

## 其他資源

為了幫助你，這裡有一份額外資源清單：

* 首先是 Adobe PDF 服務的 Power Automate 文件： <https://docs.microsoft.com/en-us/connectors/adobepdftools/>。 這些資源補充了你在這裡學到的內容。
* 需要範例嗎？ 你可以找到許多 [Power Automate](https://powerautomate.microsoft.com/en-us/connectors/details/shared_adobepdftools/adobe-pdf-services/) 範本來示範 PDF 服務。
* 我們的直播影片內容《 [迴紋針](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF)》也包含示範 Power Automate 使用情況的影片。
* [Adobe 技術部落格](https://medium.com/adobetech/tagged/microsoft-power-automate)有許多關於使用 Power Automate 的文章。
* 最後，務必參考 PDF 服務](https://developer.adobe.com/document-services/docs/overview/)的核心[文件。
