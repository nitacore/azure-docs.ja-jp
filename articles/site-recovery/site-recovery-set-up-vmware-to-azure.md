---
title: "ソース環境のセットアップ (VMware から Azure へ) | Microsoft Docs"
description: "この記事では、VMware 仮想マシンを Azure にレプリケートする前に、オンプレミス環境をセットアップする方法について説明します。"
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: anoopkv
ms.openlocfilehash: 2b6b0e5cc95f28b5596e7e898f5f035e3647d9c5
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>ソース環境のセットアップ (VMware から Azure へ)
> [!div class="op_single_selector"]
> * [VMware から Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [物理から Azure](./site-recovery-set-up-physical-to-azure.md)

この記事では、ソースのオンプレミス環境をセットアップし、VMware 上で実行されている仮想マシンを Azure にレプリケートする方法について説明します。 これには、レプリケーション シナリオを選択するための手順、オンプレミス コンピューターを Site Recovery の構成サーバーとして設定するための手順、およびオンプレミスのVM を自動的に検出するための手順が含まれます。 

## <a name="prerequisites"></a>前提条件

この記事は、既に以下の操作を行っていることを前提としています。
- [Azure Portal](http://portal.azure.com) で[リソースを設定する](tutorial-prepare-azure.md)。
- [オンプレミスの VMware 設定する](tutorial-prepare-on-premises-vmware.md) (自動検出のための専用アカウントを含む)。



## <a name="choose-your-protection-goals"></a>保護の目標を選択する

1. Azure Portal で、**Recovery Services** コンテナー ブレードに移動し、コンテナーを選択します。
2. コンテナーのリソース メニューで、**[作業の開始]** > **[Site Recovery]** > **[手順 1: インフラストラクチャを準備する]** > **[保護の目標]** の順にクリックします。

    ![Choose goals](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. **[保護の目標]** で、**[To Azure (Azure へ)]** を選択し、**[Yes, with VMware vSphere Hypervisor (はい、VMware vSphere ハイパーバイザーを使用する)]** を選択します。 次に、 **[OK]**をクリックします

    ![Choose goals](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-configuration-server"></a>構成サーバーを設定する

Open Virtualization Format (OVF) テンプレートを使用して、構成サーバーをオンプレミスの VMware VM として設定します。 VMware VM にインストールされるコンポーネントについては、[こちら](concepts-vmware-to-azure-architecture.md)をご覧ください。 

1. 構成サーバー デプロイの[前提条件](how-to-deploy-configuration-server.md#prerequisites)を確認します。 デプロイに必要な[容量を確認](how-to-deploy-configuration-server.md#capacity-planning)します。
2. OVF テンプレート (how-to-deploy-configuration-server.md) を[ダウンロード](how-to-deploy-configuration-server.md#download-the-template)して[インポート](how-to-deploy-configuration-server.md#import-the-template-in-vmware)し、構成サーバーを実行するオンプレミス VMware VM を設定します。
3. VMware VM を有効にし、Recovery Services コンテナーに[登録](how-to-deploy-configuration-server.md#register-the-configuration-server)します。


## <a name="add-the-vmware-account-for-automatic-discovery"></a>自動検出用の VMware アカウントを追加する

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

## <a name="connect-to-the-vmware-server"></a>VMware サーバーに接続する

オンプレミス環境で実行されている仮想マシンを Azure Site Recovery で検出できるようにするには、VMware vCenter サーバーまたは vSphere ESXi ホストを Site Recovery に接続する必要があります。

**[+vCenter]** を選択して、VMware vCenter サーバーまたは VMware vSphere ESXi ホストの接続を開始します。

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>一般的な問題
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>次の手順
Azure で[ターゲット環境を設定](./site-recovery-prepare-target-vmware-to-azure.md)します。
