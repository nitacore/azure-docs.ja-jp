---
title: "Open Service Broker for Azure (OSBA) を使用して Azure で管理されたサービスと統合する"
description: "Open Service Broker for Azure (OSBA) を使用して Azure で管理されたサービスと統合する"
services: container-service
author: sozercan
manager: timlt
ms.service: container-service
ms.topic: overview
ms.date: 12/05/2017
ms.author: seozerca
ms.openlocfilehash: 594cb0afbdb0a44e9f092b9afc5af13b21e763a4
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="integrate-with-azure-managed-services-using-open-service-broker-for-azure-osba"></a>Open Service Broker for Azure (OSBA) を使用して Azure で管理されたサービスと統合する

[Kubernetes サービス カタログ][kubernetes-service-catalog]と共に Open Service Broker for Azure (OSBA) を使うと、開発者は Azure で管理されたサービスを Kubernetes で利用できます。 このガイドでは、Kubernetes サービス カタログ、Open Service Broker for Azure (OSBA)、および Kubernetes を使ってAzure で管理されたサービスを使うアプリケーションのデプロイに注目します。

## <a name="prerequisites"></a>前提条件
* Azure サブスクリプション

* Azure CLI 2.0: [ローカルにインストールする][azure-cli-install]ことも、[Azure Cloud Shell][azure-cloud-shell] で使うこともできます。

* Helm CLI 2.7 以降: [ローカルにインストールする][helm-cli-install]ことも、[Azure Cloud Shell][azure-cloud-shell] で使うこともできます。

* Azure サブスクリプションの共同作成者ロールでサービス プリンシパルを作成するためのアクセス許可。

* 既存の Azure Container Service (AKS) クラスター。 AKS クラスターが必要な場合は、「[AKS クラスターの作成][create-aks-cluster]」クイックスタートに従ってください。

## <a name="install-service-catalog"></a>サービス カタログをインストールする

最初に、Helm チャートを使って Kubernetes クラスターにサービス カタログをインストールします。 クラスターの Tiller (Helm サーバー) のインストールを、次のコマンドでアップグレードします。

```azurecli-interactive
helm init --upgrade
```

次に、サービス カタログのチャートを Helm リポジトリに追加します。

```azurecli-interactive
helm repo add svc-cat https://svc-catalog-charts.storage.googleapis.com
```

最後に、Helm チャートでサービス カタログをインストールします。

```azurecli-interactive
helm install svc-cat/catalog --name catalog --namespace catalog --set rbacEnable=false
```

Helm チャートを実行した後、次のコマンドの出力に `servicecatalog` が表示されることを確認します。

```azurecli-interactive
kubectl get apiservice
```

たとえば、次のような出力が表示される必要があります (切り捨ててあります)。

```
NAME                                 AGE
v1.                                  10m
v1.authentication.k8s.io             10m
...
v1beta1.servicecatalog.k8s.io        34s
v1beta1.storage.k8s.io               10
```

## <a name="install-open-service-broker-for-azure"></a>Open Service Broker for Azure をインストールする

次のステップでは、[Open Service Broker for Azure][open-service-broker-azure] をインストールします。これには、Azure で管理されたサービスのカタログが含まれます。 使用可能な Azure サービスの例は、Azure Database for PostgreSQL、Azure Redis Cache、Azure Database for MySQL、Azure Cosmos DB、Azure SQL Database などです。

最初に、Open Service Broker for Azure の Helm リポジトリを追加します。

```azurecli-interactive
helm repo add azure https://kubernetescharts.blob.core.windows.net/azure
```

続けて、次のスクリプトを使用して[サービス プリンシパル][create-service-principal]を作成し、いくつかの変数の値を設定します。 Helm チャートを実行してサービス ブローカーをインストールするときに、これらの変数を使用します。

```azurecli-interactive
SERVICE_PRINCIPAL=$(az ad sp create-for-rbac)
AZURE_CLIENT_ID=$(echo $SERVICE_PRINCIPAL | cut -d '"' -f 4)
AZURE_CLIENT_SECRET=$(echo $SERVICE_PRINCIPAL | cut -d '"' -f 16)
AZURE_TENANT_ID=$(echo $SERVICE_PRINCIPAL | cut -d '"' -f 20)
AZURE_SUBSCRIPTION_ID=$(az account show --query id --output tsv)
```

環境変数の設定が済んだので、次のコマンドを実行してサービス ブローカーをインストールします。

```azurecli-interactive
helm install azure/open-service-broker-azure --name osba --namespace osba \
    --set azure.subscriptionId=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantId=$AZURE_TENANT_ID \
    --set azure.clientId=$AZURE_CLIENT_ID \
    --set azure.clientSecret=$AZURE_CLIENT_SECRET
```

OSBA のデプロイが完了したら、[サービス カタログ CLI][service-catalog-cli] をインストールします。これは、サービス ブローカー、サービス クラス、サービス プランなどのクエリを行うための使いやすいコマンド ライン インターフェイスです。

次のコマンドを実行して、サービス カタログ CLI のバイナリをインストールします。

```azurecli-interactive
curl -sLO https://servicecatalogcli.blob.core.windows.net/cli/latest/$(uname -s)/$(uname -m)/svcat
chmod +x ./svcat
```

ここで、インストールされているサービス ブローカーの一覧を表示します。

```azurecli-interactive
./svcat get brokers
```

次のような出力が表示されます。

```
  NAME                               URL                                STATUS
+------+--------------------------------------------------------------+--------+
  osba   http://osba-open-service-broker-azure.osba.svc.cluster.local   Ready
```

次に、利用可能なサービス クラスの一覧を表示します。 表示されるサービス クラスは使用可能な Azure で管理されたサービスであり、Open Service Broker for Azure を使ってプロビジョニングできます。

```azurecli-interactive
./svcat get classes
```

最後に、すべての利用可能なサービス プランの一覧を表示します。 サービス プランは、Azure で管理されたサービスのサービス レベルです。 たとえば、Azure Database for MySQL の場合、プランの範囲は、50 データベース トランザクション ユニット (DTU) の Basic レベルである `basic50` から、800 DTU の Standard レベルである `standard800` までです。

```azurecli-interactive
./svcat get plans
```

## <a name="install-wordpress-from-helm-chart-using-azure-database-for-mysql"></a>Azure Database for MySQL を使って Helm チャートから WordPress をインストールする

このステップでは、Helm を使って WordPress の更新された Helm チャートをインストールします。 このチャートは、WordPress が使うことのできる外部 Azure Database for MySQL インスタンスをプロビジョニングします。 このプロセスには数分かかることがあります。

```azurecli-interactive
helm install azure/wordpress --name wordpress --namespace wordpress --set resources.requests.cpu=0
```

インストールで適切なリソースがプロビジョニングされたことを確認するには、インストールされているサービス インスタンスとバインディングの一覧を表示します。

```azurecli-interactive
./svcat get instances -n wordpress
./svcat get bindings -n wordpress
```

インストールされているシークレットの一覧を表示します。

```azurecli-interactive
kubectl get secrets -n wordpress -o yaml
```

## <a name="next-steps"></a>次の手順

この記事では、Azure Container Service (AKS) クラスターにサービス カタログをデプロイしました。 Open Service Broker for Azure を使って、Azure で管理されたサービス (この例では Azure Database for MySQL) を使う WordPress のインストールをデプロイしました。

他の更新された OSBA ベースの Helm チャートにアクセスするには、[Azure/helm-charts][helm-charts] リポジトリを参照してください。 OSBA で動作する独自のチャートの作成に関心がある場合は、「[Creating a New Chart][helm-create-new-chart]」(新しいチャートの作成) をご覧ください。

<!-- LINKS - external -->
[helm-charts]: https://github.com/Azure/helm-charts
[helm-cli-install]: kubernetes-helm.md#install-helm-cli
[helm-create-new-chart]: https://github.com/Azure/helm-charts#creating-a-new-chart
[kubernetes-service-catalog]: https://github.com/kubernetes-incubator/service-catalog
[open-service-broker-azure]: https://github.com/Azure/open-service-broker-azure
[service-catalog-cli]: https://github.com/Azure/service-catalog-cli

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[create-aks-cluster]: ./kubernetes-walkthrough.md
[create-service-principal]: ./kubernetes-service-principal.md
