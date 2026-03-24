---
title: 文件產生 API 教學
description: 文件產生 API 教學概述
feature: Document Generation API
role: Developer
level: Beginner, Intermediate, Experienced
type: Tutorial
jira: KT-7480
thumbnail: KT-7480.jpg
exl-id: 519a41a2-33af-4022-8919-2cb69995c46c
source-git-commit: 4d076f7a05fd20b7e864929e74885957f42c5728
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---


# 文件產生 API 教學

文件產生 API 可從 Word 範本和 JSON 資料中建立 PDF 與 Word 文件。

>[!NOTE]
>
>文件產生 API 包含在 PDF 服務 API 中。

## 產生文件

<!-- Comment -->
<!--
CARDS

* https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen
  {target = _self}
  {title = Automate document generation}
  {description = Learn how to automatically generate documents at scale}
  {image = https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium}
  {cta = Watch}

-->
<!-- End Comment -->

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Automate document generation">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" title="自動化文件產生" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium" alt="自動化文件產生"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" title="自動化文件產生">自動化文件產生</a>
                    </p>
                    <p class="is-size-6">學習如何大規模自動產生文件</p>
                </div>
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">看</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 建立範本

文件生成 API 接受文件範本（帶有模板標籤）及輸入資料以產生最終文件。最終文件是透過將文件範本中所有模板標籤替換為根據資料輸入實際值的動態內容所產生。

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Overview of the Adobe Document Generation Tagger">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" title="Adobe 文件產生標註器概述" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/media_17b19864efffdb1f8c54017812c7de662e17bf163.png?width=400&format=webply&optimize=medium" alt="Adobe 文件產生標註器概述"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" title="Adobe 文件產生標註器概述">Adobe 文件產生標註器概述</a>
                    </p>
                    <p class="is-size-6">了解專為 Adobe 文件產生 API 設計的 Adobe 文件產生標註器</p>
                </div>
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">看</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding text tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" title="新增文字標籤" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/media_113bb0b6c3dfa1329810d3afbba3498af82c6c875.png?width=400&format=webply&optimize=medium" alt="新增文字標籤"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" title="新增文字標籤">新增文字標籤</a>
                    </p>
                    <p class="is-size-6">學習如何使用 Adobe 文件產生標註器為 Microsoft Word 範本添加文字標籤</p>
                </div>
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">看</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding image tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" title="新增圖片標籤" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/media_1095ed3adad9c9360bb3184dccc41a72a3da94edc.png?width=400&format=webply&optimize=medium" alt="新增圖片標籤"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" title="新增圖片標籤">新增圖片標籤</a>
                    </p>
                    <p class="is-size-6">學習如何使用 Adobe 文件產生標註器，將圖片動態推送到 Microsoft Word 範本中，將圖片推送到文件中</p>
                </div>
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">看</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding tables and list tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" title="新增資料表與清單標籤" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/media_1c2cc8e4bf3a85a977104a3d94073c37b93dcfdf9.png?width=400&format=webply&optimize=medium" alt="新增資料表與清單標籤"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" title="新增資料表與清單標籤">新增資料表與清單標籤</a>
                    </p>
                    <p class="is-size-6">學習如何使用 Adobe 文件產生標註器，將表格和列表標籤加入 Microsoft Word 範本，根據資料動態新增表格或列表列</p>
                </div>
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">看</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting numerical calculation tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" title="設定數值計算標籤" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/media_1a64d90689430bd8a1f7a272a66f006f0808ab6cf.png?width=400&format=webply&optimize=medium" alt="設定數值計算標籤"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" title="設定數值計算標籤">設定數值計算標籤</a>
                    </p>
                    <p class="is-size-6">學習如何使用 Adobe 文件產生標註器在 Microsoft Word 範本中設定數值計算標籤，以計算資料值的彙總或算術</p>
                </div>
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">看</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting conditional content">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" title="設定條件內容" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/media_145cebc42cffee358ed1beffcd5015ecb595fc82a.png?width=400&format=webply&optimize=medium" alt="設定條件內容"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" title="設定條件內容">設定條件內容</a>
                    </p>
                    <p class="is-size-6">學習如何利用 Adobe 文件產生標註器在 Microsoft Word 範本中設定區段，根據資料動態包含或排除文件中的區段</p>
                </div>
<a href="https://experienceleague.adobe.com/zh-hant/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">看</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
