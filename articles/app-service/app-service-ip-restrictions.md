---
title: "Azure App Service の IP 制限 | Microsoft Docs"
description: "Azure App Service で IP 制限を使用する方法"
author: btardif
manager: stefsch
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/23/2017
ms.author: byvinyal
ms.openlocfilehash: 551f34436dfeac14bb6e8676dd7c9b10d30a3af3
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="azure-app-service-static-ip-restrictions"></a>Azure App Service 静的 IP 制限 #

IP 制限を使用すると、アプリへのアクセスを許可されている IP アドレスの一覧を定義できます。 許可一覧には、個々 の IP アドレスまたはサブネット マスクによって定義された IP アドレスの範囲を含めることができます。

アプリへの要求がクライアントから生成されるとき、許可一覧に対して IP アドレスが評価されます。 IP アドレスが一覧にない場合、アプリは[HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) ステータス コードで応答します。

IP 制限は、アプリが実行時に使う web.config で定義されています (より正確には、制限は applicationHost.config ファイルの許可される IP アドレスのセットに挿入されるので、許可される IP アドレスのセットを web.config ファイルにも追加すると、それが優先されます)。 特定の状況下では、HTTP パイプラインの IP 制限ロジックの前にモジュールが実行されることがあります。 この場合、要求は異なる HTTP エラー コードで失敗します。

IP 制限は、アプリに割り当てられた同じ App Service プラン インスタンスで評価されます。

IP 制限の規則をアプリに追加するには、メニューを使って **[ネットワーク]**>**[IP 制限]** の順にクリックし、**[IP 制限の構成]** をクリックします。

![IP 制限] (media/app-service-ip-restrictions/ip-restrictions.png)

ここでは、アプリに対して定義されている IP 制限の一覧を確認できます。

![IP 制限を一覧表示する](media/app-service-ip-restrictions/browse-ip-restrictions.png)

**[[+] 追加]** をクリックすると、新しい IP 制限の規則を追加できます。

![IP 制限を追加する](media/app-service-ip-restrictions/add-ip-restrictions.png)
