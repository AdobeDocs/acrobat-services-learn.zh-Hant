---
title: Acrobat SignAPI入門
description: 瞭解如何將Acrobat SignAPI包括在您的應用程式中以收集簽名和其他資訊
feature: Acrobat Sign API
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8089
thumbnail: KT-8089.jpg
exl-id: ae1cd9db-9f00-4129-a2a1-ceff1c899a83
source-git-commit: bd53d86abb0e5f9ee302c39e07c00101e5a1f8ed
workflow-type: tm+mt
source-wordcount: '1905'
ht-degree: 0%

---

# Adobe SignAPI入門

[Acrobat SignAPI](https://developer.adobe.com/adobesign-api/)是增強您管理已簽署協定的方式的絕佳方法。 開發人員可以輕鬆地將其系統與Sign API整合，這為上傳文檔、發送文檔進行簽名、發送提醒和收集電子簽名提供了可靠、簡便的方法。

## 你能學到的

本實踐教程介紹了開發人員如何使用Sign API增強使用[!DNL Adobe Acrobat Services]建立的應用程式和工作流。 [!DNL Acrobat Services]包括[Adobe PDF服務API](https://developer.adobe.com/document-services/apis/pdf-services)、[Adobe PDF嵌入API](https://developer.adobe.com/document-services/apis/pdf-embed/)（免費）和[Adobe文檔生成API](https://developer.adobe.com/document-services/apis/doc-generation)。

更具體地說，瞭解如何將Acrobat SignAPI包括在您的應用程式中以收集簽名和其他資訊，如保險單上的員工資訊。 使用簡化的HTTP請求和響應的一般步驟。 您可以使用您喜歡的語言來實現這些請求。 您可以使用[[!DNL Acrobat Services] API](https://developer.adobe.com/document-services/homepage/)的組合建立PDF，將其作為[臨時](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/overview/terminology.md)文檔上載到簽名API，並使用協定或[小部件](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/overview/terminology.md)工作流請求最終用戶簽名。

## 建立PDF文檔

首先建立一個MicrosoftWord模板並將其保存為PDF。 或者，您可以使用文檔生成API自動化管道，以上載在Word中建立的模板，然後生成PDF文檔。 文檔生成API是[!DNL Acrobat Services]的一部分，[免費6個月，然後按時付費，每個文檔交易僅為0.05 USD](https://developer.adobe.com/document-services/pricing/main)。

在本示例中，模板只是一個包含幾個要填寫的簽名者欄位的簡單文檔。 請立即命名欄位，然後在本教程中插入實際欄位。

![具有幾個欄位的保險表格螢幕快照](assets/GSASAPI_1.png)

## 發現有效的API訪問點

在使用Sign API之前，[建立免費開發人員帳戶](https://acrobat.adobe.com/ca/en/sign/developer-form.html)以訪問API、測試文檔交換和執行以及測試電子郵件功能。

Adobe將Acrobat SignAPI分佈到全球許多稱為「碎片」的部署單位。 每個碎片都為客戶帳戶提供服務，如NA1、NA2、NA3、EU1、JP1、AU1、IN1等。 碎片名稱與地理位置相對應。 這些分片構成API端點的基本URI（接入點）。

要訪問簽名API，您必須首先發現帳戶的正確訪問點，該訪問點可以是api.na1.adobesign.com、api.na4.adobesign.com、api.eu1.adobesign.com或其他，具體取決於您的位置。

```
  GET /api/rest/v6/baseUris HTTP/1.1
  Host: https://api.adobesign.com
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body (example):

  {
    "apiAccessPoint": "https://api.na4.adobesign.com/", 
    "webAccessPoint": "https://secure.na4.adobesign.com/" 
  }
```

在上例中，是以值作為訪問點的響應。

>[!IMPORTANT]
>
>在這種情況下，您對Sign API進行的所有後續請求都必須使用該接入點。 如果您使用的接入點不服務於您的區域，則會出錯。

## 上載臨時文檔

Adobe Sign使您能夠建立不同的流，為簽名或資料收集準備文檔。 無論應用程式的流程如何，您都必須先上載文檔，該文檔僅可用七天。 隨後的API調用必須引用此臨時文檔。

文檔使用POST請求上載到`/transientDocuments`終結點。 多部件請求由檔案名、檔案流和文檔檔案的MIME（媒體）類型組成。 終結點響應包含標識文檔的ID。

此外，您的應用程式可以為Acrobat Sign指定回調URL以ping，並在簽名過程完成時通知應用程式。


```
  POST /api/rest/v6/transientDocuments HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: multipart/form-data
  File-Name: "Insurance Form.pdf"
  File: "[path]\Insurance Form.pdf"
  Accept: application/json

  Response Body (example):

  {
     "transientDocumentId": "3AAA...BRZuM"
  }
```

## 建立Web窗體

Web表單（以前稱為簽名小部件）是托管文檔，任何有權訪問的人都可以對其進行簽名。 Web表單的示例包括註冊表、豁免以及許多人線上訪問和登錄的其他文檔。

要使用Sign API建立新Web表單，必須先上載臨時文檔。 對`/widgets`終結點的POST請求使用返回的`transientDocumentId`。

在此示例中，Web表單為`ACTIVE`，但您可以以以下三種不同狀態之一建立它：

* DRAFT — 增量構建Web表單

* 創作 — 在Web表單中添加或編輯表單域

* 活動 — 立即托管Web表單

還必須定義表單參與者的資訊。 `memberInfos`屬性包含參與者的資料，如電子郵件。 當前，此集不支援多個成員。 但是，由於Web表單簽名者的電子郵件在建立Web表單時未知，因此電子郵件應保持為空，如下例所示。 `role`屬性定義`memberInfos`中的成員（如SIGNER和APPROVER）承擔的角色。

```
  POST /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: application/json

  Request Body:

  {
    "fileInfos": [
      {
      "transientDocumentId": "YOUR-TRANSIENT-DOCUMENT-ID"
      }
     ],
    "name": "Insurance Form",
      "widgetParticipantSetInfo": {
          "memberInfos": [{
              "email": ""
          }],
      "role": "SIGNER"
      },
      "state": "ACTIVE"
  }

  Response Body (example):

  {
     "id": "CBJ...PXoK2o"
  }
```

您可以將Web表單建立為`DRAFT`或`AUTHORING`，然後在表單通過應用程式管道時更改其狀態。 要更改Web表單狀態，請參閱[PUT/widgets/{widgetId}/state](https://secure.na4.adobesign.com/public/docs/restapi/v6#!/widgets/updateWidgetState)終結點。

## 正在讀取承載URL的Web表單

下一步是發現承載Web表單的URL。 /widgets終結點檢索Web表單資料清單，包括您轉發給用戶的Web表單的托管URL，以收集簽名和其他表單資料。

此終結點返回清單，因此在獲取承載Web表單的URL之前，您可以在`userWidgetList`中按其ID查找特定表單：

```
  GET /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body:

  {
    "userWidgetList": [
      {
        "id": "CBJCHB...FGf",
        "name": "Insurance Form",
        "groupId": "CBJCHB...W86",
        "javascript": "<script type='text/javascript' ...
        "modifiedDate": "2021-03-13T15:52:41Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...Rag*",
        "hidden": false
      },
      {
        "id": "CBJCHB...I8_",
        "name": "Insurance Form",
        "groupId": "CBJCHBCAABAAyhgaehdJ9GTzvNRchxQEGH_H1ya0xW86",
        "javascript": "<script type='text/javascript' language='JavaScript'
        src='https://sec
        "modifiedDate": "2021-03-13T02:47:32Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...AAB",
        "hidden": false
      },
      {
        "id": "CBJCHB...Wmc",
```

## 管理Web表單

此表單是供用戶填寫的PDF文檔。 但是，您仍然需要告訴表單的編輯器用戶必須填寫哪些欄位以及他們在文檔中的位置：

![具有幾個欄位的保險表格螢幕快照](assets/GSASAPI_1.png)

上面的文檔尚未顯示欄位，但尚未顯示。 在定義收集簽名者資訊的欄位及其大小和位置時，會添加這些欄位。

現在，轉到「您的協定」頁上的[Web表單](https://secure.na4.adobesign.com/public/agreements/#agreement_type=webform)頁籤，並查找您建立的表單。

![「Acrobat Sign管理」頁籤的螢幕截圖](assets/GSASAPI_2.png)

![已選擇Web表單的「Acrobat Sign管理」頁籤的螢幕快照](assets/GSASAPI_3.png)

按一下&#x200B;**編輯**&#x200B;以開啟文檔編輯頁面。 可用的預定義欄位位於右面板中。

![Acrobat Sign窗體創作環境的螢幕快照](assets/GSASAPI_4.png)

編輯器允許您拖放文本和簽名欄位。 添加所有必需欄位後，可以調整大小並對齊它們以拋光表單。 最後，按一下&#x200B;**保存**&#x200B;以建立表單。

![添加了表單域的Acrobat Sign表單創作環境的螢幕快照](assets/GSASAPI_5.png)

## 發送用於簽名的Web表單

完成Web表單後，必須提交它，以便用戶可以填寫並簽名。 保存表單後，可以查看和複製URL和嵌入代碼。

**複製Web表單URL**：使用此URL將用戶發送到此協定的托管版本以供審閱和簽名。 例如：

[https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3...babw\*](https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3AAABLblqZhCndYscuKcDMPiVfQlpaGPb-5D7ebE9NUTQ6x6jK7PIs8HCtTzr3HOx8U6D5qqbabw*)

**複製Web表單嵌入代碼**：將協定複製並貼上到您的HTML中，以將協定添加到您的網站。

例如：

```
<iframe
src="https://secure.na4.adobesign.com/public/esignWidget?wid=CBFC
...yx8*&hosted=false" width="100%" height="100%" frameborder="0"
style="border: 0;
overflow: hidden; min-height: 500px; min-width: 600px;"></iframe>
```

![最終Web表單的螢幕截圖](assets/GSASAPI_6.png)

當您的用戶訪問您的表單的托管版本時，他們將首先查看上載的臨時文檔，其中的欄位按指定位置放置。

![最終Web表單的螢幕截圖](assets/GSASAPI_7.png)

然後，用戶填入欄位並簽名表單。

![選擇「簽名」欄位的用戶螢幕快照](assets/GSASAPI_8.png)

接下來，您的用戶使用先前儲存的簽名或使用新簽名對文檔進行簽名。

![簽名體驗螢幕截圖](assets/GSASAPI_9.png)

![簽名截圖](assets/GSASAPI_10.png)

當用戶按一下&#x200B;**應用**&#x200B;時，Adobe會指示他們開啟電子郵件並確認簽名。 在確認到達之前，簽名仍處於掛起狀態。

![僅再執行一步的螢幕截圖](assets/GSASAPI_11.png)

這種認證增加了多重認證並增強了簽名過程的安全性。

![確認消息的螢幕快照](assets/GSASAPI_12.png)

![完成消息的螢幕快照](assets/GSASAPI_13.png)

## 正在讀取已完成的Web表單

現在是時候獲取用戶填寫的表單資料了。 `/widgets/{widgetId}/formData`終結點在用戶簽名表單時將用戶輸入的資料檢索到互動式表單中。

```
GET /api/rest/v6/widgets/{widgetId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
```

生成的CSV檔案流包含表單資料。

```
Response Body:
"Agreement
name","completed","email","role","first","last","title","company","agreementId",
"email verified","web form signed/approved"
"Insurance Form","","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","","","2021-03-07 19:32:59"
```

## 建立協定

作為Web表單的替代方法，您可以建立協定。 以下各節演示了使用Sign API管理協定的一些簡單步驟。

將文檔發送到指定的收件人以進行簽名或批准會建立協定。 您可以使用API跟蹤協定的狀態和完成。

可以使用[臨時文檔](https://helpx.adobe.com/sign/kb/how-to-send-an-agreement-through-REST-API.html)、[庫文檔](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/samples/send_using_library_doc.md)或URL建立協定。 在此示例中，協定基於`transientDocumentId`，與先前建立的Web表單相同。

```
POST /api/rest/v6/agreements HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
Request Body:
{
    "fileInfos": [
      {
      "transientDocumentId": "{transientDocumentId}"
      }
     ],
    "name": "{agreementName}",
    "participantSetsInfo": [
      {
      "memberInfos": [
          {
          "email": "{signerEmail}"
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

在本示例中，協定以IN_PROCESS的形式建立，但您可以在以下三種不同狀態之一建立協定：

* DRAFT — 在發送協定之前逐步構建協定

* AUTHORING — 添加或編輯協定中的表單域

* IN_PROCESS — 立即發送協定

要更改協定狀態，請使用`PUT /agreements/{agreementId}/state`終結點執行以下允許的狀態轉換之一：

* 草稿到創作

* 創作到IN_PROCESS

* 取消的進程(_P)

上面的`participantSetsInfo`屬性提供了預期參與協定的人員的電子郵件以及他們執行的操作（簽名、批准、確認等）。 在上例中，只有一個參與者：簽名者。 書面簽名限制為每個文檔4個。

與Web表單不同，在您建立協定時，Adobe會自動將其發送出去進行簽名。 終結點返回協定的唯一標識符。


```
  Response Body:

  {
     id (string): The unique identifier of the agreement
  }
```

## 正在檢索有關協定成員的資訊

建立協定後，可以使用`/agreements/{agreementId}/members`終結點檢索有關協定成員的資訊。 例如，您可以檢查參與者是否已簽署協定。

```
GET /api/rest/v6/agreements/{agreementId}/members HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: application/json
```

生成的JSON響應正文包含有關參與者的資訊。

```
  Response Body:

  {
     "participantSets":[
        {
           "memberInfos":[
              {
                 "id":"CBJ...xvM",
                 "email":"participant@email.com",
                 "self":false,
                 "securityOption":{
                    "authenticationMethod":"NONE"
                 },
                 "name":"John Doe",
                 "status":"ACTIVE",
                 "createdDate":"2021-03-16T03:48:39Z",
                 "userId":"CBJ...vPv"
              }
           ],
           "id":"CBJ...81x",
           "role":"SIGNER",
           "status":"WAITING_FOR_MY_SIGNATURE",
           "order":1
        }
     ],
```

## 正在發送協定提醒

根據業務規則，截止日期可能會阻止參與者在特定日期之後簽署協定。 如果協定具有到期日期，則可以在該日期臨近時提醒參與者。

根據您在上一節中呼叫`/agreements/{agreementId}/members`終結點後收到的協定成員資訊，您可以向尚未簽署協定的所有參與者發出電子郵件提醒。

向`/agreements/{agreementId}/reminders`終結點的POST請求為由`agreementId`參數標識的協定的指定參與者建立提醒。

```
POST /agreements/{agreementId}/reminders HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
  Request Body:

  {
    "recipientParticipantIds": [{agreementMemberIdList}],
    "agreementId": "{agreementId}",
    "note": "This is a reminder that you haven't signed the agreement yet.",
    "status": "ACTIVE"
  }

  Response Body:

  {
     id (string, optional): An identifier of the reminder resource created on the
     server. If provided in POST or PUT, it will be ignored
  }
```

在您發佈提醒後，用戶將收到一封電子郵件，其中包含協定的詳細資訊和指向協定的連結。

![提醒消息螢幕快照](assets/GSASAPI_14.png)

## 正在讀取已完成的協定

與Web表單一樣，您可以閱讀有關收件人簽署的協定的詳細資訊。 `/agreements/{agreementId}/formData`終結點檢索用戶在對Web表單進行簽名時輸入的資料。

```
GET /api/rest/v6/agreements/{agreementId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
Response Body:
"completed","email","role","first","last","title","company","agreementId"
"2021-03-16 18:11:45","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","CBJCHBCAABAA5Z84zy69q_Ilpuy5DzUAahVfcNZillDt"
```

## 後續步驟

Acrobat SignAPI使您能夠管理文檔、 Web表單和協定。 使用Web表單和協定建立的簡化而完整的工作流採用通用方式完成，使開發人員能夠使用任何語言來實施這些工作流。

有關Sign API如何工作的概述，可以在[API使用開發人員指南](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/api_usage.md)中找到示例。 本文檔包含有關文章中遵循的許多步驟以及其他相關主題的簡短文章。

Acrobat SignAPI可通過[個單電子簽名計畫和多用戶電子簽名計畫](https://acrobat.adobe.com/tw/zh-Hant/sign/pricing/plans.html)的多個層提供，因此您可以選擇最適合您需要的定價模型。 既然您知道將Sign API合併到您的應用中是多麼容易，您可能對[Acrobat SignWebhooks](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/webhooks.md)等其他功能感興趣。 Webhooks不要求您的應用在Acrobat Sign事件中執行頻繁檢查，而是允許您註冊一個HTTP URL，每當發生事件時，Sign API會為其執行POST回調請求。 Webhooks通過即時和即時更新為應用程式提供電源，從而實現強健的寫程式。

查看[按使用付費定價](https://developer.adobe.com/document-services/pricing/main)，瞭解您的6個月免費Adobe PDF服務API試用期何時結束以及免費Adobe PDF嵌入式API。

若要將諸如自動文檔建立和文檔簽名等令人興奮的功能添加到你的應用中，請開始使用[[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)。

