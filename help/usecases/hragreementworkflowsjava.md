---
title: Java中的HR文檔工作流
description: '[!DNL Adobe Acrobat Services]個API可輕鬆將PDF功能納入您的HR Web應用程式'
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-7474
thumbnail: KT-7474.jpg
exl-id: add4cc5c-06e3-4ceb-930b-e8c9eda5ca1f
source-git-commit: ba73105ecf0bd27b7445ec4388fc4009eec273b8
workflow-type: tm+mt
source-wordcount: '1777'
ht-degree: 0%

---

# Java中的HR文檔工作流

![使用案例英雄橫幅](assets/UseCaseHRHero.jpg)

許多企業都需要有關新員工的文檔，例如為在家工作的員工簽訂的工作協定。 傳統上，企業以難以管理和儲存的形式對這些文檔進行物理管理。 切換到電子文檔時，PDF檔案是理想的選擇，因為它們比其他檔案類型更安全，修改量也更小。 此外，它們支援數字簽名。

## 你能學到的

在本實踐教程中，瞭解如何實施基於Web的HR表單，該表單將工作區協定保存為在簡單的Java Spring MVC應用程式中通過註銷進行PDF。

## 相關API和資源

* [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe SignAPI](https://developer.adobe.com/adobesign-api/)

* [項目代碼](https://github.com/dawidborycki/adobe-sign)

## 正在生成API憑據

首先註冊Adobe PDF服務API免費試用版。 轉到[Adobe](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) [網站](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK)，然後按一下&#x200B;*新建憑據*&#x200B;下的&#x200B;*開始*&#x200B;按鈕。 免費試用提供1,000個可在6個月內使用的單據交易。 在下一頁（請參閱下面）中，選擇服務(PDF服務API)，設定憑據名稱（例如HRDocumentWFCredentials），然後輸入說明。

選擇語言（此示例為Java）並選中&#x200B;*建立個性化代碼示例*。 最後一步確保代碼示例已包含您使用的預填充pdftools-api-credentials.json檔案，以及在API中驗證應用程式的私鑰。

最後，按一下&#x200B;*建立憑據*&#x200B;按鈕。 這將生成憑據，並自動開始下載示例。

![建立新憑據螢幕快照](assets/HRWJ_1.png)

要確保憑據正在工作，請開啟下載的示例。 這裡，您正在使用IntelliJ IDEA。 開啟原始碼時，整合開發環境(IDE)會要求生成引擎。 此示例中使用Maven，但您也可以使用Gradle，具體取決於您的首選項。

接下來，執行`mvn clean install` Maven目標以生成jar檔案。

最後，運行CombinePDF示例，如下所示。 代碼在輸出資料夾內生成PDF。

![運行CombinePDF示例螢幕快照的菜單](assets/HRWJ_2.png)

## 建立Spring MVC應用程式

給定然後建立應用程式的憑據。 此示例使用Spring Initializr。

首先，配置項目設定以使用Java 8語言和Jar打包（請參閱下面的螢幕截圖）。

![Spring Initializr的螢幕快照](assets/HRWJ_3.png)

其次，添加Spring Web（從Web）和Thymeleaf（從模板引擎）:

![廣告Spring Web和Thymeleaf的螢幕截圖](assets/HRWJ_4.png)

建立項目後，轉到pom.xml檔案，並使用pdftools-sdk和log4j-slf4j-impl補充依賴項部分：

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

</dependencies>
```

然後，使用您下載的兩個檔案（示例代碼）補充項目的根資料夾：

* pdftools-api-credentials.json

* private.key

## 呈現Web窗體

要呈現Web表單，請使用呈現個人資料表單並處理過帳表單的控制器修改應用程式。 因此，首先使用PersonForm模型類修改應用程式：

```
package com.hr.docsigning;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

public class PersonForm {
    @NotNull
    @Size(min=2, max=30)
    private String firstName;

    @NotNull
    @Size(min=2, max=30)
    private String lastName;

    public String getFirstName() {
            return this.firstName;
    }


    public void setFirstName(String firstName) {
            this.firstName = firstName;
    }

    public String getLastName() {
           return this.lastName;
    }

    public void setLastName(String lastName) {
            this.lastName = lastName;
    }

    public String GetFullName() {
           return this.firstName + " " + this.lastName;
    }
}
```

此類包含兩個屬性： `firstName`和`lastName`。 此外，使用此簡單驗證可檢查它們是否介於2到30個字元之間。

給定模型類，可以建立控制器（請參見來自夥伴代碼的PersonController.java）:

```
package com.hr.docsigning;
import org.springframework.stereotype.Controller;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import javax.validation.Valid;


@Controller
public class PersonController {
    @GetMapping("/")
    public String showForm(PersonForm personForm) {
        return "form";
    }
}
```

控制器只有一種方法：showForm。 它負責使用resources/templates/form.html中的HTML模板呈現表單：

```
<html>
<head>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
</head>
 
<body>
<div class="w3-container">
    <h1>HR Department</h1>
</div>
 
<form class="w3-panel w3-card-4" action="#" th:action="@{/}"
        th:object="${personForm}" method="post">
    <h2>Personal data</h2>
    <table>
        <tr>
            <td>First Name:</td>
            <td><input type="text" class="w3-input"
                placeholder="First name" th:field="*{firstName}" /></td>
            <td class="w3-text-red" th:if="${#fields.hasErrors('firstName')}"
                th:errors="*{firstName}"></td>
        </tr>
        <tr>
            <td>Last Name:</td>
            <td><input type="text" class="w3-input"
                placeholder="Last name" th:field="*{lastName}" /></td>
            <td class="w3-text-red" th:if="${#fields.hasErrors('lastName')}"
                th:errors="*{lastName}"></td>
        </tr>
        <tr>
            <td><button class="w3-button w3-black" type="submit">Submit</button></td>
        </tr>
    </table>
</form>
</body>
</html>
```

為了呈現動態內容，使用Thymeleaf模板呈現引擎。 因此，在運行應用程式後，您應看到以下內容：

![已呈現內容的螢幕快照](assets/HRWJ_5.png)

## 使用動態內容生成PDF

現在，通過在呈現個人資料表單後動態填充選定欄位來生成包含虛擬合同的PDF文檔。 具體來說，您必須將人員資料填充到預先建立的合同中。

在此，為了簡單起見，您只有一個標題、一個子標題和一個字串常數：「此合同是為\&lt;人員的全名\>準備的」。

要實現此目標，請從Adobe的[從動態HTML](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf-from-dynamic-html)示例建立PDF開始。 通過分析該示例代碼，可以看到動態HTML欄位填充的過程如下所示。

首先，必須準備包含靜態和動態內容的HTML頁。 動態部件使用JavaScript進行更新。 即，PDF服務API將JSON對象注入HTML。

然後，使用載入HTML文檔時調用的JavaScript函式獲取JSON屬性。 此JavaScript函式更新選定的DOM元素。 下面是填充span元素並保存人員資料的示例(請參閱伴隨代碼的src\\main\\resources\\contract\\index.html):

```
<html>
<head>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
</head>
 
<body onload="updateFullName()">
    <script src="./json.js"></script>
    <script type="text/javascript">
        function updateFullName()
        {
            var document = window.document;
            document.getElementById("personFullName").innerHTML = String(
                window.json.personFullName);
        }
    </script>
 
    <div class="w3-container ">
        <h1>HR Department</h1>
 
        <h2>Contract details</h2>
 
        <p>This contract was prepared for:
            <strong><span id="personFullName"></span></strong>
        </p>
    </div>
</body>
</html>
```

然後，必須使用所有相關的JavaScript和CSS檔案對HTML進行壓縮。 PDF服務API不接受HTML檔案。 而是需要一個zip檔案作為輸入。 在這種情況下，您將壓縮檔案儲存在src\\main\\resources\\contract\\index.zip中。

之後，您可以使用處理POST請求的其他方法來補充`PersonController`:

```
@PostMapping("/")
public String checkPersonInfo(@Valid PersonForm personForm,
    BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
        return "form";
    }
 
    CreateContract(personForm);
 
    return "contract-actions";
}
```

以上方法使用提供的個人資料建立PDF合同並呈現合同 — 活動視圖。 後者提供到所生成PDF的連結和用於簽署PDF。

現在，讓我們看看`CreateContract`方法的工作原理（完整清單如下）。 該方法依賴於兩個欄位：

* `LOGGER`，從log4j調試有關任何異常的資訊

* `contractFilePath`，包含生成的PDF的檔案路徑

`CreateContract`方法設定憑據並從HTML建立PDF。 要傳遞並填充合同中人員的資料，請使用`setCustomOptionsAndPersonData`幫助程式。 此方法從表單中檢索人員的資料，然後通過上面說明的JSON對象將其發送到生成的PDF。

此外，`setCustomOptionsAndPersonData`還顯示了如何通過禁用頁眉和頁腳來控制PDF外觀。 完成這些步驟後，將PDF檔案保存到output/contract.pdf並最終刪除先前生成的檔案。

```
private static final Logger LOGGER = LoggerFactory.getLogger(PersonController.class);
private String contractFilePath = "output/contract.pdf"; 
private void CreateContract(PersonForm personForm) {
    try {
        // Initial setup, create credentials instance.
        Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
                .fromFile("pdftools-api-credentials.json")
                .build();

        //Create an ExecutionContext using credentials 
       //and create a new operation instance.
        ExecutionContext executionContext = ExecutionContext.create(credentials);
        CreatePDFOperation htmlToPDFOperation = CreatePDFOperation.createNew();

        // Set operation input from a source file.
        FileRef source = FileRef.createFromLocalFile(
           "src/main/resources/contract/index.zip");
       htmlToPDFOperation.setInput(source);

        // Provide any custom configuration options for the operation
        // You pass person data here to dynamically fill out the HTML
        setCustomOptionsAndPersonData(htmlToPDFOperation, personForm);

        // Execute the operation.
        FileRef result = htmlToPDFOperation.execute(executionContext);

        // Save the result to the specified location. Delete previous file if exists
        File file = new File(contractFilePath);
        Files.deleteIfExists(file.toPath());

        result.saveAs(file.getPath());

    } catch (ServiceApiException | IOException | 
             SdkException | ServiceUsageException ex) {
        LOGGER.error("Exception encountered while executing operation", ex);
    }
}
 
private static void setCustomOptionsAndPersonData(
    CreatePDFOperation htmlToPDFOperation, PersonForm personForm) {
    //Set the dataToMerge field that needs to be populated 
    //in the HTML before its conversion
    JSONObject dataToMerge = new JSONObject();
    dataToMerge.put("personFullName", personForm.GetFullName());
 
    // Set the desired HTML-to-PDF conversion options.
    CreatePDFOptions htmlToPdfOptions = CreatePDFOptions.htmlOptionsBuilder()
        .includeHeaderFooter(false)
        .withDataToMerge(dataToMerge)
        .build();
    htmlToPDFOperation.setOptions(htmlToPdfOptions);
}
```

在生成合同時，您還可以將動態的、特定於人員的資料與固定的合同條款合併。 為此，請按照[從靜態PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf-from-dynamic-html)示例建立HTML。 或者，您可以[合併兩個PDF](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf-from-static-html)。

## 正在呈現PDF檔案以供下載

現在，您可以顯示到生成的PDF的連結，供用戶下載。 為此，請首先建立contract-actions.html檔案（請參見附加代碼的resources/templates contract-actions.html）:

```
<html>
<head>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
</head>
 
<div class="w3-container ">
    <h1>HR Department</h1>
 
    <h2>Contract file</h2>
 
    <p>Click <a href="/pdf">here</a> to download your contract</p>
</div>
</body>
</html>
```

然後，按如下方式在`downloadContract`類中實現`PersonController`方法：

```
@RequestMapping("/pdf")
public void downloadContract(HttpServletResponse response)
{
    Path file = Paths.get(contractFilePath);
 
    response.setContentType("application/pdf");
    response.addHeader(
        "Content-Disposition", "attachment; filename=contract.pdf");

    try
    {
        Files.copy(file, response.getOutputStream());
        response.getOutputStream().flush();
    }
    catch (IOException ex) 
    {
        ex.printStackTrace();
    }
}
```

運行應用後，您會得到以下流。 第一個螢幕顯示個人資料表單。 要測試，請用介於2到30個字元之間的任何值填充它：

![資料值螢幕快照](assets/HRWJ_6.png)

按一下&#x200B;*提交*&#x200B;按鈕後，表單將驗證並基於PDF(resources/contract/index.html)生成HTML。 應用產品將顯示另一個視圖（合同詳細資訊），您可以在其中下載PDF:

![可下載PDF的螢幕截圖](assets/HRWJ_7.png)

PDF在Web瀏覽器中呈現後，如下所示。 即，您輸入的個人資料將傳播到PDF:

![用個人資料呈現的PDF螢幕快照](assets/HRWJ_8.png)

## 啟用簽名和安全性

當協定達成時，Adobe Sign可以添加代表批准的數字簽名。 Adobe Sign驗證與OAuth有些不同。 現在，讓我們看看如何將應用程式與Adobe Sign整合。 為此，必須為應用程式準備訪問令牌。 然後，使用Adobe SignJava SDK編寫客戶端代碼。

要獲取授權令牌，必須執行以下幾個步驟：

首先，註冊[開發人員帳戶](https://acrobat.adobe.com/tw/zh-Hant/acrobat/contact.html)。

在[Adobe Sign門戶](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/gstarted/create_app.md)中建立CLIENT應用程式。

按照[此處](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/gstarted/configure_oauth.md)和[此處](https://secure.eu1.adobesign.com/public/static/oauthDoc.jsp)的說明為應用程式配置OAuth。 請注意您的客戶端標識符和客戶端密碼。 然後，可以使用`https://www.google.com`作為重定向URI和以下作用域：

* user_login:self

* agreement_read：帳戶

* agreement_write：帳戶

* agreement_send：帳戶

使用客戶端ID代替\&lt;CLIENT_ID\>，按如下方式準備URL:

```
https://secure.eu1.adobesign.com/public/oauth?redirect_uri=https://www.google.com
&response_type=code
&client_id=<CLIENT_ID>
&scope=user_login:self+agreement_read:account+agreement_write:account+agreement_send:account
```

在Web瀏覽器中鍵入以上URL。 您被重定向到google.com，並且代碼以代碼=\&lt;YOUR_CODE\>的形式顯示在地址欄中，
示例：

```
https://www.google.com/?code=<YOUR_CODE>&api_access_point=https://api.eu1.adobesign.com/&web_access_point=https://secure.eu1.adobesign.com%2F
```

請注意為\&lt;YOUR_CODE\>和api_access_point提供的值。

要發送為您提供訪問令牌的HTTPPOST請求，請使用客戶端ID、\&lt;YOUR_CODE\>和api_access_point值。 您可以使用[Postman](https://helpx.adobe.com/sign/kb/how-to-create-access-token-using-postman-adobe-sign.html)或cURL:

```
curl --location --request POST "https://**api.eu1.adobesign.com**/oauth/token"
\\

\--data-urlencode "client_secret=**\<CLIENT_SECRET\>**" \\

\--data-urlencode "client_id=**\<CLIENT_ID\>**" \\

\--data-urlencode "code=**\<YOUR_CODE\>**" \\

\--data-urlencode "redirect_uri=**https://www.google.com**" \\

\--data-urlencode "grant_type=authorization_code"
```

示例響應如下所示：

```
{
    "access_token":"3AAABLblqZhByhLuqlb-…",
    "refresh_token":"3AAABLblqZhC_nJCT7n…",
    "token_type":"Bearer",
    "expires_in":3600
}
```

請注意access_token。 你需要它來授權你的客戶代碼。

## 使用Adobe SignJava SDK

一旦您擁有了訪問令牌，就可以將REST API調用發送到Adobe Sign。 要簡化此過程，請使用Adobe SignJava SDK。 原始碼可在[AdobeGitHub儲存庫](https://github.com/adobe-sign/AdobeSignJavaSdk)中使用。

要將此包與應用程式整合，必須克隆代碼。 然後，建立Maven包（mvn包），並將以下檔案安裝到項目中（可以在adobe-sign-sdk資料夾的配套代碼中找到這些檔案）:

* target/swagger-java-client-1.0.0.jar

* target/lib/gson-2.8.1.jar

* target/lib/gson-fire-1.8.0.jar

* target/lib/hamcrest-core-1.3.jar

* target/lib/junit-4.12.jar

* target/lib/logging intercepter-2.7.5.jar

* target/lib/okhttp-2.7.5.jar

* target/lib/okio-1.6.0.jar

* target/lib/swagger-annotations-1.5.15.jar

在IntelliJ IDEA中，可以使用&#x200B;*項目結構*（檔案/項目結構）將這些檔案添加為依賴項。

## 正在發送PDF以進行簽名

您現在已準備好發送協定供簽署。 為此，請首先將contract-details.html與發送請求的另一個超連結補充：

```
<html>
<head>
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
</head>
 
<div class="w3-container ">
    <h1>HR Department</h1>
 
    <h2>Contract file</h2>
 
    <p>Click <a href="/pdf"> here</a> to download your contract</p>
 
    
</div>
</body>
</html>
```

然後，您添加另一個控制器`AdobeSignController`，在該控制器中您實現了`sendContractMethod`（請參閱伴侶代碼）。 該方法的工作原理如下：

首先，它使用`ApiClient`獲取API終結點。

```
ApiClient apiClient = new ApiClient();

//Default baseUrl to make GET /baseUris API call.
String baseUrl = "https://api.echosign.com/";
String endpointUrl = "/api/rest/v6";
apiClient.setBasePath(baseUrl + endpointUrl);

// Provide an OAuth Access Token as "Bearer access token" in authorization
String authorization = "Bearer ";

// Get the baseUris for the user and set it in apiClient.
BaseUrisApi baseUrisApi = new BaseUrisApi(apiClient);
BaseUriInfo baseUriInfo = baseUrisApi.getBaseUris(authorization);
apiClient.setBasePath(baseUriInfo.getApiAccessPoint() + endpointUrl);
```

然後，該方法使用contract.pdf檔案建立臨時文檔：

```
// Get PDF file
String filePath = "output/";
String fileName = "contract.pdf";
File file = new File(filePath + fileName);
String mimeType = "application/pdf";
 
//Get the id of the transient document.
TransientDocumentsApi transientDocumentsApi =
    new TransientDocumentsApi(apiClient);
TransientDocumentResponse response = transientDocumentsApi.createTransientDocument(authorization,
    file, null, null, fileName, mimeType);
String transientDocumentId = response.getTransientDocumentId();
```

接下來，必須建立協定。 為此，請使用contract.pdf檔案，並將協定狀態設定為IN_PROCESS，以立即發送檔案。 此外，您還選擇電子簽名：

```
// Create AgreementCreationInfo
AgreementCreationInfo agreementCreationInfo = new AgreementCreationInfo();
 
// Add file
FileInfo fileInfo = new FileInfo();
fileInfo.setTransientDocumentId(transientDocumentId);
agreementCreationInfo.addFileInfosItem(fileInfo);
 
// Set state to IN_PROCESS, so the agreement is be sent immediately
agreementCreationInfo.setState(AgreementCreationInfo.StateEnum.IN_PROCESS);
agreementCreationInfo.setName("Contract");
agreementCreationInfo.setSignatureType(AgreementCreationInfo.SignatureTypeEnum.ESIGN);
```

接下來，按如下方式添加協定收件人。 在這裡，您將添加兩個收件人（請參閱「員工」和「經理」部分）:

```
// Provide emails of recipients to whom agreement is be sent
// Employee
ParticipantSetInfo participantSetInfo = new ParticipantSetInfo();
ParticipantSetMemberInfo participantSetMemberInfo = new ParticipantSetMemberInfo();
participantSetMemberInfo.setEmail("");
participantSetInfo.addMemberInfosItem(participantSetMemberInfo);
participantSetInfo.setOrder(1);
participantSetInfo.setRole(ParticipantSetInfo.RoleEnum.SIGNER);
agreementCreationInfo.addParticipantSetsInfoItem(participantSetInfo);
 
// Manager
participantSetInfo = new ParticipantSetInfo();
participantSetMemberInfo = new ParticipantSetMemberInfo();
participantSetMemberInfo.setEmail("");
participantSetInfo.addMemberInfosItem(participantSetMemberInfo);
participantSetInfo.setOrder(2);
participantSetInfo.setRole(ParticipantSetInfo.RoleEnum.SIGNER);
agreementCreationInfo.addParticipantSetsInfoItem(participantSetInfo);
```

最後，使用Adobe SignJava SDK中的`createAgreement`方法發送協定：

```
// Create agreement using the transient document.
AgreementsApi agreementsApi = new AgreementsApi(apiClient);
AgreementCreationResponse agreementCreationResponse = agreementsApi.createAgreement(
    authorization, agreementCreationInfo, null, null);
 
System.out.println("Agreement sent, ID: " + agreementCreationResponse.getId());
```

運行此代碼後，您將收到一封電子郵件(指向代碼中指定為`<email_address>)`的地址，並帶有協定簽名請求。 電子郵件中包含超連結，該超連結將收件人引導到Adobe Sign門戶以執行簽名。 您可以在Adobe Sign開發人員門戶中看到文檔（參見下圖），還可以使用[getAgreementInfo](https://github.com/adobe-sign/AdobeSignJavaSdk/blob/master/docs/AgreementsApi.md#getAgreementInfo)方法以寫程式方式跟蹤簽名過程。

最後，您還可以使用PDF服務API對PDF進行密碼保護，如以下[示例](https://github.com/adobe/pdfservices-java-sdk-samples/tree/master/src/main/java/com/adobe/pdfservices/operation/samples/protectpdf)中所示。

![合同詳細資訊螢幕快照](assets/HRWJ_9.png)

## 後續步驟

如您所見，通過利用快速啟動，您可以實現一個簡單的Web表單，以使用Adobe PDF服務API在Java中建立一個已批准的PDF。 Adobe PDFAPI可無縫整合到您現有的客戶端應用程式中。

進一步舉例，您可以建立表單收件人可以遠程安全地簽名。 當需要多個簽名時，您甚至可以自動將表單發送給工作流中的一系列人員。 員工入職情況有所改善，人力資源部門會愛你。

請簽出[[!DNL Adobe Acrobat Services]](https://developer.adobe.com/document-services/homepage/)，以立即向您的應用程式添加多個PDF功能。
