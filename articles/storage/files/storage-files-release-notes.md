---
title: "Azure File Sync エージェントのリリース ノート | Microsoft Docs"
description: "Azure File Sync のリリース ノート"
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: tamram
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 9b6dfec6465482efcbf55d0441e44a0278f44a22
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2018
---
# <a name="azure-file-sync-agent-release-notes"></a>Azure File Sync エージェントのリリース ノート
Azure ファイル同期 (プレビュー) を使用すると、オンプレミスのファイル サーバーの柔軟性、パフォーマンス、互換性を損なわずに Azure Files で組織のファイル共有を一元化できます。 これは、Windows Server を Azure ファイル共有のクイック キャッシュに変換することで行います。 Windows Server で使用可能な任意のプロトコル (SMB、NFS、FTPS など) を使用してデータにローカル アクセスすることができ、世界中に必要な数だけキャッシュを持つことができます。

この記事では、サポートされているバージョンの Azure File Sync エージェントのリリース ノートについて取り上げます。

## <a name="supported-versions"></a>サポートされているバージョン
Azure File Sync でサポートされるバージョンは次のとおりです。

| エージェントのバージョン番号 | リリース日 | サポート対象 |
|----------------------|--------------|------------------|
| 2.0.11.0 | 2018-02-08 | 現在のバージョン |
| 1.1.0.0 | 2017-09-26 | 2018-07-30 |

### <a name="azure-file-sync-agent-update-policy"></a>Azure ファイル同期エージェントの更新ポリシー
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-20110"></a>エージェント バージョン 2.0.11.0
次のリリース ノートは、2018 年 2 月 9 日にリリースされたエージェント バージョン 2.0.11.0 のものです。 

### <a name="agent-installation-and-server-configuration"></a>エージェントのインストールとサーバー構成
Windows Server で Azure File Sync エージェントをインストールして構成する方法の詳細については、「[Azure File Sync (プレビュー) のデプロイの計画](storage-sync-files-planning.md)」および「[Azure File Sync をデプロイする方法 (プレビュー)](storage-sync-files-deployment-guide.md)」を参照してください。

- エージェント インストール パッケージ (MSI) は、昇格された (管理者) 特権でインストールする必要があります。
- Windows Server Core または Nano Server のデプロイ オプションではサポートされません。
- Windows Server 2016 および 2012 R2 でのみサポートされます。
- エージェントには、少なくとも 2 GB の物理メモリが必要です。

### <a name="interoperability"></a>相互運用性
- 階層化されたファイルにアクセスするウイルス対策やバックアップなどのアプリケーションは、オフライン属性を考慮してそれらのファイルのコンテンツの読み取りをスキップする場合を除き、望ましくない再呼び出しを行う可能性があります。 詳細については、「[Azure File Sync のトラブルシューティング (プレビュー)](storage-sync-files-troubleshoot.md)」を参照してください
- この更新で、新たに DFS-R がサポートされています。  詳細については、[計画ガイド](storage-sync-files-planning.md#distributed-file-system-dfs)を参照してください。
- ファイル サーバー リソース マネージャー (またはその他の) ファイル スクリーンを使用しないでください。ファイル スクリーンが原因でファイルがブロックされた場合、無限の同期エラーが発生する可能性があります。
- 登録済みサーバーの複製 (VM の複製を含む) により、予期せぬ結果が生じる可能性があります (特に、同期が収束しなくなる可能性があります)。
- データ重複除去とクラウドの階層化は、同じボリューム上ではサポートされません。
 
### <a name="sync-limitations"></a>同期の制限事項
次の項目は同期されませんが、システムの残りの部分は引き続き正常に動作します。
- 2,048 文字を超えるパス
- 2K を超える場合はセキュリティ記述子の DACL の部分 (これは、1 つの項目に約 40 を超えるアクセス制御エントリがある場合のみの問題です)
- セキュリティ記述子の SACL の部分 (監査に使用されます)
- 拡張属性
- 代替データ ストリーム
- 再解析ポイント
- ハード リンク
- 圧縮がサーバー ファイルに設定されている場合、変更が他のエンドポイントからそのファイルに同期されるときに、圧縮は保持されません
- サービスがデータを読み取ることを妨げる、EFS (またはその他のユーザー モードの暗号化) で暗号化されたファイル 
    
    > [!Note]  
    > Azure File Sync は、転送中のデータを常に暗号化します。Azure に保存してあるデータを暗号化することもできます。
 
### <a name="server-endpoints"></a>サーバー エンドポイント
- サーバー エンドポイントは、NTFS ボリューム上にのみ作成できます。 現在、ReFS、FAT、FAT32 などのファイル システムは Azure File Sync でサポートされていません。
- サーバー エンドポイントはシステム ボリューム上に配置できません (たとえば、C:\MyFolder は、C:\MyFolder がマウント ポイントでない限り、許容されるパスではありません)。
- フェールオーバー クラスタリングは、クラスター化ディスクでのみサポートされ、クラスターの共有ボリューム (CSV) ではサポートされません。
- サーバー エンドポイントは、入れ子にできませんが、同じボリューム上に互いに並列に共存させることができます。
- (10,000 個を超える) 多数のディレクトリを一度にサーバーから削除すると、同期エラーが発生する可能性があります。10,000 個未満のバッチに分けてディレクトリを削除し、削除操作が正常に同期していることを確認してから次のバッチを削除してください。
- このリリースでは、ボリュームのルートにある同期ルートのサポートが追加されました。
- サーバー エンドポイント内に OS またはアプリケーションのページング ファイルを格納しないでください。
- このリリースでの変更として、新しいイベントが追加されました。クラウド階層化の合計実行時間を追跡するためのイベント (EventID 9016)、同期のアップロードの進捗を追跡するためのイベント (EventID 9302)、および同期されなかったファイルを追跡するためのイベント (EventID 9900) です。
- このリリースでの変更として、高速 DR 名前空間同期パフォーマンスが大幅に向上しました。
 
### <a name="cloud-tiering"></a>クラウドの階層化
- 前バージョンからの変更として、新しいファイルが階層化ポリシー設定に従って 1 時間以内に階層化されます (以前は 32 時間) - オンデマンドで階層化するための PowerShell コマンドレットが提供されるため、バックグラウンドでの処理を待たずに、より効率的に階層化を評価することができます。
- 階層化されたファイルを Robocopy を使用して別の場所にコピーする場合、結果のファイルは階層化されませんが、オフライン属性が設定される場合があります。これは、その属性が Robocopy によってコピー操作の対象として誤って扱われるためです。
- SMB クライアントからファイルのプロパティを表示するとき、SMB によるファイルのメタデータのキャッシュが原因で、オフライン属性が正しく設定されていないように見えることがあります。
- 以前のバージョンからの変更として、ファイルが新規であるか、既に階層化されたファイルである場合、階層化されたファイルとして他のサーバーにダウンロードされるようになりました。

## <a name="agent-version-1100"></a>エージェント バージョン 1.1.0.0
次のリリース ノートは、2017 年 9 月 9 日にリリースされたエージェント バージョン 1.1.0.0 のものです。 このリリースは、Azure File Sync の初期プレビュー リリースです。

### <a name="agent-installation-and-server-configuration"></a>エージェントのインストールとサーバー構成
Windows Server で Azure File Sync エージェントをインストールして構成する方法の詳細については、「[Azure File Sync (プレビュー) のデプロイの計画](storage-sync-files-planning.md)」および「[Azure File Sync をデプロイする方法 (プレビュー)](storage-sync-files-deployment-guide.md)」を参照してください。

- エージェント インストール パッケージ (MSI) は、昇格された (管理者) 特権でインストールする必要があります。
- Windows Server Core または Nano Server のデプロイ オプションではサポートされません。
- Windows Server 2016 および 2012 R2 でのみサポートされます。
- エージェントには、少なくとも 2 GB の物理メモリが必要です。

### <a name="interoperability"></a>相互運用性
- 階層化されたファイルにアクセスするウイルス対策やバックアップなどのアプリケーションは、オフライン属性を考慮してそれらのファイルのコンテンツの読み取りをスキップする場合を除き、望ましくない再呼び出しを行う可能性があります。 詳細については、「[Azure File Sync のトラブルシューティング (プレビュー)](storage-sync-files-troubleshoot.md)」を参照してください
- ファイル サーバー リソース マネージャー (またはその他の) ファイル スクリーンを使用しないでください。ファイル スクリーンが原因でファイルがブロックされた場合、無限の同期エラーが発生する可能性があります。
- 登録済みサーバーの複製 (VM の複製を含む) により、予期せぬ結果が生じる可能性があります (特に、同期が収束しなくなる可能性があります)。
- データ重複除去とクラウドの階層化は、同じボリューム上ではサポートされません。
 
### <a name="sync-limitations"></a>同期の制限事項
次の項目は同期されませんが、システムの残りの部分は引き続き正常に動作します。
- 2,048 文字を超えるパス
- 2K を超える場合はセキュリティ記述子の DACL の部分 (これは、1 つの項目に約 40 を超えるアクセス制御エントリがある場合のみの問題です)
- セキュリティ記述子の SACL の部分 (監査に使用されます)
- 拡張属性
- 代替データ ストリーム
- 再解析ポイント
- ハード リンク
- 圧縮がサーバー ファイルに設定されている場合、変更が他のエンドポイントからそのファイルに同期されるときに、圧縮は保持されません
- サービスがデータを読み取ることを妨げる、EFS (またはその他のユーザー モードの暗号化) で暗号化されたファイル 
    
    > [!Note]  
    > Azure File Sync は、転送中のデータを常に暗号化します。Azure に保存してあるデータを暗号化することもできます。
 
### <a name="server-endpoints"></a>サーバー エンドポイント
- サーバー エンドポイントは、NTFS ボリューム上にのみ作成できます。 現在、ReFS、FAT、FAT32 などのファイル システムは Azure File Sync でサポートされていません。
- サーバー エンドポイントはシステム ボリューム上に配置できません (たとえば、C:\MyFolder は、C:\MyFolder がマウント ポイントでない限り、許容されるパスではありません)。
- フェールオーバー クラスタリングは、クラスター化ディスクでのみサポートされ、クラスターの共有ボリューム (CSV) ではサポートされません。
- サーバー エンドポイントは、入れ子にできませんが、同じボリューム上に互いに並列に共存させることができます。
- (10,000 個を超える) 多数のディレクトリを一度にサーバーから削除すると、同期エラーが発生する可能性があります。10,000 個未満のバッチに分けてディレクトリを削除し、削除操作が正常に同期していることを確認してから次のバッチを削除してください。
- ボリュームのルートではサポートされません。
- サーバー エンドポイント内に OS またはアプリケーションのページング ファイルを格納しないでください。
 
### <a name="cloud-tiering"></a>クラウドの階層化
- ファイルを正しく再呼び出しできるようにするため、新しいサーバー エンドポイントを構成した後の初回の階層化を含め、最大 32 時間にわたって新しいファイルや変更されたファイルが自動的に階層化されないことがあります。オンデマンドで階層化するための PowerShell コマンドレットが用意されているため、バックグラウンド プロセスを待つことなく、より効率的に階層化を評価できます。
- 階層化されたファイルを Robocopy を使用して別の場所にコピーする場合、結果のファイルは階層化されませんが、オフライン属性が設定される場合があります。これは、その属性が Robocopy によってコピー操作の対象として誤って扱われるためです。
- SMB クライアントからファイルのプロパティを表示するとき、SMB によるファイルのメタデータのキャッシュが原因で、オフライン属性が正しく設定されていないように見えることがあります。
