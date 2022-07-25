---
title: 線上託管說明
permalink: index.html
layout: home
ms.openlocfilehash: 8cc220d44fe56f19385b25675c29e391b610db8e
ms.sourcegitcommit: d2d374fffa4fcbf92b9c4bdc9c9ecc470152e033
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2021
ms.locfileid: "145196050"
---
## <a name="content-directory"></a>內容目錄

下列是每個實驗室的超連結。

## <a name="labs"></a>實驗室

{% assign labs = site.pages | where_exp: "page", "page.url contains '/Instructions/Labs'" %}
| 模組 | 實驗室 |
| --- | --- |
{% for activity in labs  %}{% if activity.lab.az204Module %}| {{ activity.lab.az204Module }} | [{{ activity.lab.az204Title }}]({{ site.github.url }}{{ activity.url }}) |
{% endif %}{% endfor %}
