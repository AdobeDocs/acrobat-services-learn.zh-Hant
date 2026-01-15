---
title: 文檔生成APITutorials
description: 文檔生成API教程概述
feature: Document Generation API
role: Developer
level: Beginner, Intermediate, Experienced
type: Tutorial
jira: KT-7480
thumbnail: KT-7480.jpg
exl-id: 519a41a2-33af-4022-8919-2cb69995c46c
source-git-commit: ba73105ecf0bd27b7445ec4388fc4009eec273b8
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---


# 文檔生成API教程

文檔生成API從Word模板和JSON資料建立PDF和Word文檔。

>[!NOTE]
>
>文檔生成API包含在PDF服務API中。

## 生成文檔

<!-- Comment -->
<!-- CARDS

* https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen
  {target = _self}
  {title = Automate document generation}
  {description = Learn how to automatically generate documents at scale}
  {image = https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium}
  {cta = Watch}

-->
<!-- End Comment -->

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Automate document generation">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" title="自動生成文檔" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_120e532325e3fdc7f559ad9b446d9ec08c1e239a1.png?width=400&format=webply&optimize=medium" alt="自動生成文檔"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" title="自動生成文檔">自動生成文檔</a>
                    </p>
                    <p class="is-size-6">瞭解如何按比例自動生成文檔</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/automate-doc-gen" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">監視</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## 建立範本

文檔生成API接受文檔模板（帶有模板標籤）以及輸入資料以生成最終文檔。 通過基於對應於資料輸入的實際值將文檔模板中的所有模板標籤替換為動態內容來生成最終文檔。

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Overview of the Adobe Document Generation Tagger">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" title="Adobe文檔生成標籤概覽" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_17b19864efffdb1f8c54017812c7de662e17bf163.png?width=400&format=webply&optimize=medium" alt="Adobe文檔生成標籤概覽"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" title="Adobe文檔生成標籤概覽">Adobe文檔生成標誌器概述</a>
                    </p>
                    <p class="is-size-6">獲取設計用於Adobe文檔生成API的Adobe文檔生成標誌器的概述</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeroverview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">監視</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding text tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" title="添加文本標籤" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_113bb0b6c3dfa1329810d3afbba3498af82c6c875.png?width=400&format=webply&optimize=medium" alt="添加文本標籤"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" title="添加文本標籤">添加文本標籤</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用Adobe文檔生成標籤將文本標籤添加到MicrosoftWord模板</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddtexttags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">監視</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding image tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" title="添加影像標籤" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_1095ed3adad9c9360bb3184dccc41a72a3da94edc.png?width=400&format=webply&optimize=medium" alt="添加影像標籤"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" title="添加影像標籤">正在添加影像標籤</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用「Adobe文檔生成標籤」將影像標籤動態地推入文檔中，添加到MicrosoftWord模板中</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggeraddimagetags" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">監視</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adding tables and list tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" title="添加表和清單標籤" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_1c2cc8e4bf3a85a977104a3d94073c37b93dcfdf9.png?width=400&format=webply&optimize=medium" alt="添加表和清單標籤"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" title="添加表和清單標籤">添加表和清單標籤</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用Adobe文檔生成標籤將表和清單標籤添加到MicrosoftWord模板中，以根據資料動態添加表或清單行</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggertables" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">監視</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting numerical calculation tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" title="設定數值計算標籤" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_1a64d90689430bd8a1f7a272a66f006f0808ab6cf.png?width=400&format=webply&optimize=medium" alt="設定數值計算標籤"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" title="設定數值計算標籤">正在設定數字計算標籤</a>
                    </p>
                    <p class="is-size-6">瞭解如何在MicrosoftWord模板中使用Adobe文檔生成標籤來設定數字計算標籤以計算資料值的聚合或算術</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggercalculations" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">監視</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Setting conditional content">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" title="設定條件內容" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/media_145cebc42cffee358ed1beffcd5015ecb595fc82a.png?width=400&format=webply&optimize=medium" alt="設定條件內容"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" title="設定條件內容">設定條件內容</a>
                    </p>
                    <p class="is-size-6">瞭解如何使用「Adobe文檔生成標誌器」在MicrosoftWord模板中設定節，以便根據資料動態地包括或排除文檔的節</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/acrobat-services-learn/tutorials/docgen/docgentemplates/taggerconditional" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">監視</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
