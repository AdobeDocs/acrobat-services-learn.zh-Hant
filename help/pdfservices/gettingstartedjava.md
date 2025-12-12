---
title: Adobe PDF服務API和Java入門
description: 開發人員只需幾分鐘就可以開始使用，即可運行提供用於訪問所有可用Web服務的示例檔案
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6676
thumbnail: KT-6676.jpg
exl-id: 4a8f2119-c464-496b-bdc8-35dd387bef25
source-git-commit: bd53d86abb0e5f9ee302c39e07c00101e5a1f8ed
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# Adobe PDF服務API和Java入門

![建立PDF英雄影像](assets/GettingStartedJava_hero.jpg)

開發人員只需幾分鐘就可以開始工作，隨時可以運行為訪問所有可用Web服務而提供的示例檔案。 本教程將指導您完成使用PDF服務Java SDK開始運行示例的所有步驟：

## 步驟1：獲取憑據並下載示例檔案

第一步是獲取憑據（API密鑰）以解鎖使用。 [在此處註冊免費試用版](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)，然後按一下「開始」以建立新憑據。

![步驟1](assets/GettingStartedJava_step1.png)

選擇「個人帳戶」註冊免費試用非常重要：

![個人](assets/GettingStartedJava_personal.png)

在下一步中，您將選擇PDF服務API服務，然後為憑據添加名稱和說明。

此時會出現「建立個性化代碼示例」複選框。 選擇此選項可將新憑據自動添加到示例檔案中，這將保存將其添加到項目中的手動步驟。

接下來，選擇Java作為接收Java特定示例的語言，然後按一下「建立憑據」按鈕。

![憑據](assets/GettingStartedJava_credentials.png)

您將收到一個名為PDFToolsSDK-JavaSamples.zip的.zip檔案，該檔案可保存到您的本地檔案系統。

## 步驟2：設定Java環境

1. 安裝[Java 8或更高版本](https://www.oracle.com/java/technologies/javase-downloads.html)（如果尚未安裝）。
1. 運行`javac -version`以驗證您的安裝。
1. 驗證PATH變數中是否包含JDK bin資料夾（方法因作業系統而異）。
1. 如果尚未使用首選工具安裝[Maven](https://maven.apache.org/install.html)。

個性化的示例提供從準備運行的示例代碼、嵌入的憑據json檔案以及預配置到依賴項的連接等一切。

1. 下載[示例項目](https://github.com/adobe/pdftools-java-sdk-samples)。
1. 使用Maven: mvn全新安裝構建示例項目。
1. 在命令行或首選IDE中測試示例代碼。

## 最後的想法

PDF服務API可通過自動化常見工作流和將處理負擔轉移到雲來幫助您消除手動流程。 在每個瀏覽器都以不同方式處理PDF的世界中，利用Adobe PDF嵌入式API和PDF服務API，您可以建立流線型、可靠且可預測的進程，這些進程每次&#x200B;**運行並正確顯示**，而不考慮平台或設備。

## 資源和後續步驟

* 有關其他幫助和支援，請訪問Adobe[[!DNL Acrobat Services] APIs](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&sort=latest_replies&filter=all)社區論壇

* PDF服務API [文檔](https://www.adobe.com/go/pdftoolsapi_doc)

* [FAQ](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197)以瞭解PDF服務API問題

* [請與我們聯繫](https://www.adobe.com/go/pdftoolsapi_requestform)以瞭解有關許可和定價的問題

* 相關文章

  [新PDF服務API為文檔工作流提供了更多功能](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [ [!DNL Adobe Acrobat Services]的7月版：PDF嵌入和PDF服務](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)

