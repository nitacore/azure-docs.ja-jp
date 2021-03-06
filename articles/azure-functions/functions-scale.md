---
title: "Azure Functions のスケールとホスティング | Microsoft Docs"
description: "Azure Functions の従量課金プランと App Service プランの選択方法について説明します。"
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure Functions, 関数, 従量課金プラン, App Service プラン, イベント処理, webhook, 動的コンピューティング, サーバーレス アーキテクチャ"
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/12/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5be2fe57287f816434b6d6fdf40dbbcb0dd435f4
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2018
---
# <a name="azure-functions-scale-and-hosting"></a>Azure Functions のスケールとホスティング

Azure Functions は、従量課金プランと Azure App Service プランという 2 つの異なるモードで実行できます。 従量課金プランでは、コードの実行時にコンピューティング能力を自動的に割り当て、負荷の処理の必要性に応じてスケールアウトし、コードを実行していないときはスケールダウンします。 アイドル状態の VM に対して課金されることはなく、事前に容量を予約する必要もありません。 この記事では、従量課金プランである[サーバーレス](https://azure.microsoft.com/overview/serverless-computing/) アプリ モデルを中心に説明します。 App Service プランの仕組みの詳細については、「[Azure App Service プランの詳細な概要](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)」を参照してください。 

>[!NOTE]  
> [Linux ホスティング](functions-create-first-azure-function-azure-cli-linux.md)は現在、App Service プランでのみ利用可能です。

Azure Functions に慣れていない場合は、「[Azure Functions の概要](functions-overview.md)」を参照してください。

関数アプリを作成するときに、アプリに含まれる関数のホスティング プランを構成する必要があります。 どちらのモードでも、関数は "*Azure Functions ホスト*" のインスタンスにより実行されます。 プランの種類のコントロール対象は次のとおりです。

* ホスト インスタンスをスケールアウトする方法。
* 各ホストで使用できるリソース。

現在、ホスティング プランの種類は、関数アプリの作成中に選択する必要があり、 後で変更することはできません。 

App Service プランの階層間で拡大縮小して、さまざまな量のリソースを割り当てることができます。 従量課金プランでは、すべてのリソース割り当てが Azure Functions によって自動的に処理されます。

## <a name="consumption-plan"></a>従量課金プラン

従量課金プランを使用する場合、Azure Functions ホストのインスタンスは、受信イベントの数に基づいて動的に追加および削除されます。 このプランではスケーリングが自動的に行われ、関数の実行中にのみコンピューティング リソースに対して料金が発生します。 従量課金プランでは、関数を最大で 10 分間実行できます。 

> [!NOTE]
> 従量課金プランの関数に対する既定のタイムアウトは 5 分です。 Function App に対してこの値を 10 分まで増やすには、[host.json](functions-host-json.md#functiontimeout) プロジェクト ファイルの `functionTimeout` プロパティを変更します。

課金は、実行数、実行時間、およびメモリの使用量に基づいて行われ、 関数アプリ内のすべての関数にわたって集計されます。 詳細については、[Azure Functions の価格]に関するページを参照してください。

従量課金プランは既定のホスティング プランであり、次の利点があります。
- 関数の実行中にのみ課金されます。
- 負荷が高い期間中であっても、自動的にスケールアウトされます。

## <a name="app-service-plan"></a>App Service プラン

App Service プランでは、Function App は Web Apps、API Apps、Mobile Apps と同様に、Basic、Standard、Premium、および Isolated SKU の専用 VM 上で実行されます。 専用 VM が App Service アプリに割り当てられるので、Functions ホストは継続的に実行されます。 App Service プランでは、Linux をサポートしています。

次のような場合に App Service プランを検討してください。
- 既に他の App Service インスタンスを実行している、使用率の低い既存の VM がある。
- Function App を継続的に、またはほぼ継続的に実行したい。 この場合、App Service プランは、さらにコスト効率性に優れています。
- 従量課金プランで提供されるよりも多くの CPU またはメモリのオプションが必要である。
- 従量課金プランで許可されている最大実行時間 (10 分) よりも長く実行する必要がある。
- App Service 環境、VNET/VPN 接続、大規模な VM のサポートなど、App Service プランでのみ使用できる機能が必要である。 
- Linux 上で Function App を実行するか、または関数を実行するために Linux 上にカスタム イメージを提供したい。

VM を使用すると、コストが実行数、実行時間、メモリの使用量から切り離されます。 このため、割り当てた VM インスタンスのコストを超えて課金されることはありません。 App Service プランの仕組みの詳細については、「[Azure App Service プランの詳細な概要](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)」を参照してください。 

App Service プランでは、VM インスタンスを追加して手動でスケールアウトするか、自動スケールを有効にすることができます。 詳細については、「[手動または自動によるインスタンス数のスケール変更](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json)」を参照してください。 別の App Service プランを選択してスケールアップすることもできます。 詳細については、 [Azure でのアプリのスケールアップ](../app-service/web-sites-scale.md) に関するページを参照してください。 

App Service プランで JavaScript 関数を実行する予定がある場合は、CPUの少ないプランを選択してください。 詳細については、[シングルコア App Service プランの選択](functions-reference-node.md#considerations-for-javascript-functions)に関するページをご覧ください。  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
### 常時接続

App Service プランを実行する場合、関数アプリが正常に実行されるように、**常時接続**設定を有効にする必要があります。 App Service プランでは、関数のランタイムは非アクティブな状態が数分続くとアイドル状態となるため、関数を "起こす" ことができるのは HTTP トリガーのみとなります。 これは、WebJobs で常時接続を有効にしておく必要がある理由と似ています。 

常時接続は App Service プランでのみ使用可能です。 従量課金プランでは、関数アプリはプラットフォームにより自動的にアクティブ化されます。

## <a name="storage-account-requirements"></a>ストレージ アカウントの要件

関数アプリを使用するには、従量課金プランと App Service プランのどちらでも、Azure BLOB、Queue、Files、Table Storage をサポートする一般的な Azure ストレージ アカウントが必要です。 内部的には、Azure Functions はトリガーの管理や関数実行のログなどの操作に Azure Storage を使用します。 BLOB 専用のストレージ アカウント (Premium Storage を含む)、ゾーン冗長ストレージ レプリケーションを使用する汎用ストレージ アカウントなど、一部のストレージ アカウントでは、キューとテーブルがサポートされません。 これらのアカウントは、関数アプリの作成時に **[ストレージ アカウント]** ブレードから除外されます。

<!-- JH: Does using a PRemium Storage account improve perf? -->

ストレージ アカウントの種類の詳細については、[Azure Storage サービスの概要](../storage/common/storage-introduction.md#introducing-the-azure-storage-services)に関する記事をご覧ください。

## <a name="how-the-consumption-plan-works"></a>従量課金プランの仕組み

従量課金プランでは、関数がトリガーされるイベントの数に基づいて Functions ホストのインスタンスを追加することで、スケール コントローラーによって CPU とメモリのリソースが自動的に拡大縮小されます。 Functions ホストの各インスタンスのメモリは、1.5 GB に制限されています。  ホストのインスタンスは Function App です。つまり、関数アプリ内のすべての関数がインスタンス内のリソースを共有し、同時にスケールされます。

従量課金ホスティング プランを使用する場合は、関数コード ファイルが、関数のメイン ストレージ アカウントの Azure Files 共有に保存されます。 関数アプリのメイン ストレージ アカウントを削除すると、関数コード ファイルは削除され、復元できません。

> [!NOTE]
> 従量課金プランで BLOB トリガーを使用しているときに関数アプリがアイドル状態になると、新しい BLOB の処理が最大で 10 分遅延する場合があります。 関数アプリが実行されると、BLOB は直ちに処理されます。 この初期段階の遅延を避けるために、次のオプションのいずれかを検討してください。
> - 常時接続が有効な App Service プランで関数アプリをホストする。
> - Event Grid サブスクリプションや BLOB 名を含むキュー メッセージなど、別のメカニズムを使用して BLOB 処理をトリガーする。 例については、[BLOB の入力のバインディング例](functions-bindings-storage-blob.md#input---example)に関する記事をご覧ください。

### <a name="runtime-scaling"></a>実行時のスケーリング

Azure Functions は "*スケール コントローラー*" と呼ばれるコンポーネントを使用して、イベント レートを監視し、スケールアウトとスケールインのどちらを実行するかを決定します。 スケール コントローラーは、トリガーの種類ごとにヒューリスティックを使用します。 たとえば、Azure Queue Storage トリガーを使用した場合、拡大縮小はキューの長さや最も古いキュー メッセージの経過時間に基づいて実施されます。

スケーリングは Function App 単位で行われます。 関数アプリがスケールアウトするときは、Azure Functions ホストの複数のインスタンスを実行するための追加リソースが割り当てられます。 反対に、コンピューティングの需要が減ると、スケール コントローラーにより、関数ホストのインスタンスが削除されます。 Function App 内で関数が何も実行されていない場合、インスタンスの数は最終的に 0 にスケールダウンされます。

![イベントを監視してインスタンスを作成しているスケール コントローラー](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>スケーリングの動作について

スケーリングはさまざまな要因によって異なる可能性があり、選択したトリガーと言語に基づいて異なる方法でスケールします。 ただし、今日のシステムにはスケーリングのいくつかの側面があります。
* 1 つの関数アプリは、最大 200 インスタンスにスケールできます。 1 つのインスタンスで一度に複数のメッセージや要求を処理できるので、同時実行の数に上限は設定されていません。
* 新しいインスタンスは、10 秒ごとに最大 1 回しか割り当てられません。

次の記事に記載されているように、トリガーごとにスケーリングの上限が異なる場合もあります。

* [イベント ハブ](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>スケーラブルなアプリのベスト プラクティスとパターン

関数アプリには、スケールに影響を及ぼすさまざまな側面 (ホスト構成、ランタイム フットプリント、リソースの効率など) があります。  詳細については、[パフォーマンスの考慮事項に関する記事のスケーラビリティのセクション](functions-best-practices.md#scalability-best-practices)をご覧ください。

### <a name="billing-model"></a>課金モデル

従量課金プランの課金の詳細については、[Azure Functions の価格]に関するページをご覧ください。 使用量は Function App レベルで集計され、関数コードが実行されている期間のみカウントされます。 課金の単位は、次のとおりです。 
* **ギガバイト/秒 (GB/秒) 単位でのリソース使用量**。 メモリ サイズと、関数アプリ内の全関数の実行時間の組み合わせとして計算されます。 
* **実行回数**。 イベント トリガーに応じて関数が実行されるたびにカウントされます。

[Azure Functions の価格]: https://azure.microsoft.com/pricing/details/functions
