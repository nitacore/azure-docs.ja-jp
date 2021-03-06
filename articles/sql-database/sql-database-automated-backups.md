---
title: "Azure SQL Database のバックアップ - 自動、geo 冗長 | Microsoft Docs"
description: "SQL Database は数分ごとにローカル データベースをバックアップし、Azure 読み取りアクセス geo 冗長ストレージを利用して地理的冗長性を提供します。"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3ee3d49d-16fa-47cf-a3ab-7b22aa491a8d
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Active
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 57132367faaa4c4869f2415f32897562c8056fb1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2018
---
# <a name="learn-about-automatic-sql-database-backups"></a>SQL Database 自動バックアップについての詳細情報

SQL Database はデータベースをバックアップし、Azure 読み取りアクセス geo 冗長ストレージ (RA-GRS) を利用して地理的冗長性を提供します。 バックアップは自動的に作成され、追加料金は発生しません。 ユーザー側の操作は必要ありません。 データの不慮の破損または削除から保護するデータベース バックアップは、ビジネス継続性およびディザスター リカバリー戦略の最も重要な部分です。 独自のストレージ コンテナーにバックアップを保持したい場合は、長期的なバックアップの保持ポリシーを構成できます。 詳細については、「[長期保存](sql-database-long-term-retention.md)」をご覧ください。

## <a name="what-is-a-sql-database-backup"></a>SQL Database バックアップとは何か。

SQL Database は SQL Server 技術を利用し、[完全](https://msdn.microsoft.com/library/ms186289.aspx)バックアップ、[差分](https://msdn.microsoft.com/library/ms175526.aspx)バックアップ、[トランザクション ログ](https://msdn.microsoft.com/library/ms191429.aspx) バックアップを作成します。 トランザクション ログ バックアップはパフォーマンス レベルとデータベース活動の量に基づいて、一般的には 5 - 10 分ごとに発生します。 完全バックアップと差分バックアップによるトランザクション ログ バックアップにより、データベースをホストする同じサーバーに、データベースを特定の時点に復元できます。 データベースを復元するとき、どのバックアップを復元する必要があるかをサービスが判定します (完全、差分、トランザクション ログ)。


これらのバックアップを使用して、以下を行うことができます。

* リテンション期間内の特定の時点にデータベースを復元します。 この操作により、元のデータベースと同じサーバーに新しいデータベースが作成されます。
* 削除したデータベースを、削除された時点または保有期間内の任意の時点に復元します。 削除されたデータベースは、元のデータベースが作成されたサーバーと同じサーバーにのみ復元できます。
* 別の地理的リージョンにデータベースを復元します。 これにより、サーバーやデータベースにアクセスできないときに、地理的な障害から復旧できます。 世界中のどこでも、あらゆる既存のサーバーで新しいデータベースを作成します。 
* Azure Recovery Services コンテナーに格納されている特定のバックアップからデータベースを復元します。 これにより、データベースの古いバージョンに復元でき、コンプライアンスの要求を満たし、またはアプリケーションの古いバージョンを実行できます。 [長期保存](sql-database-long-term-retention.md)に関する記事を参照してください。
* 復元を実行するには、[バックアップからのデータベースの復元](sql-database-recovery-using-backups.md)に関する記事を参照してください。

> [!NOTE]
> Azure Storage の " *レプリケーション* " という用語は、ある場所から別の場所にファイルをコピーすることを表します。 SQL の " *データベース レプリケーション* " は、複数のセカンダリ データベースとプライマリ データベースとの同期を保つことを意味します。 
> 

## <a name="how-often-do-backups-happen"></a>バックアップはどのくらいの頻度で行われますか。
完全データベース バックアップは毎週作成され、差分データベース バックアップは通常、数時間に 1 回作成されます。また、トランザクション ログのバックアップは通常 5 - 10 分間隔で実行されます。 初回の完全バックアップは、データベースの作成直後にスケジュールされます。 通常この操作は 30 分以内に終了しますが、データベースのサイズが大きい場合はそれ以上かかることがあります。 たとえば、復元されたデータベースまたはデータベースのコピーでは、初期バックアップに時間がかかります。 初回の完全バックアップ以降のバックアップは、すべて自動的にスケジュールされ、バックグラウンドで自動的に管理されます。 データベースのバックアップの正確なタイミングは、全体的なシステムのワークロードのバランスを図りながら SQL Database サービスによって決定されます。 

バックアップ ストレージの geo レプリケーションは、Azure Storage のレプリケーション スケジュールに基づいて発生します。

## <a name="how-long-do-you-keep-my-backups"></a>バックアップはどのくらいの期間保存されますか。
各 SQL Database バックアップには、データベースの[サービス レベル](sql-database-service-tiers.md)に基づくリテンション期間が指定されます。 データベースのリテンション期間:


* Basic サービスレベルの場合、7 日間です。
* Standard サービス レベルの場合、35 日間です。
* Premium サービス レベルの場合、35 日間です。

Standard または Premium サービス レベルから Basic にデータベースをダウングレードした場合、バックアップは 7 日間保存されます。 7 日間を過ぎた場合、既存のバックアップはすべて利用できなくなります。 

Basic サービス レベルから Standard または Premium にデータベースをアップグレードした場合、SQL Database は 35 日間が経過するまで既存のバックアップを保持します。 新しいバックアップは 35 日間保持されます。

データベースを削除した場合、SQL Database はオンライン データベースの場合と同じようにバックアップを保持します。 たとえば、リテンション期間が 7 日間の Basic データベースを削除するとします。 3 日経過したバックアップはあと 4 日間保存されます。

> [!IMPORTANT]
> SQL Database をホストする Azure SQL サーバーを削除すると、サーバーに属するすべてのデータベースも削除され、復元できなくなります。 削除されたサーバーを復元することはできません。
> 

## <a name="how-to-extend-the-backup-retention-period"></a>バックアップの保有期間を延長するにはどうすればよいですか。
アプリケーションで、バックアップがより長期間使用可能である必要がある場合、個別のデータベースの長期的バックアップ保持ポリシー (LTR ポリシー) を構成することによって、組み込みの保有期間を延長できます。 これにより、組み込みの保有期間を 35 日から 10 年に延長できます。 詳細については、「[長期保存](sql-database-long-term-retention.md)」をご覧ください。

Azure Portal または API を使用して LTR ポリシーをデータベースに追加すると、週単位のデータベースの完全バックアップが自動的にユーザー独自の Azure Backup サービス コンテナーにコピーされます。 データベースが TDE で暗号化されている場合、バックアップは保存中に自動的に暗号化されます。  期限切れのバックアップは、そのタイムスタンプおよび LTR ポリシーに基づいて、サービス コンテナーによって自動的に削除されます。  そのため、バックアップのスケジュールを管理したり、古いファイルのクリーンアップについて心配したりする必要はありません。 復元の API は、資格情報コンテナーが SQL Database と同じサブスクリプションにある限り、資格情報コンテナーに格納されているバックアップをサポートします。 Azure Portal または PowerShell を使用してこれらのバックアップにアクセスできます。

> [!TIP]
> ハウツー ガイドについては、「[Azure SQL Database を構成して長期のバックアップ リテンション期間から復元する](sql-database-long-term-backup-retention-configure.md)」を参照してください
>

## <a name="are-backups-encrypted"></a>バックアップは暗号化されますか?

Azure SQL データベースに対して TDE が有効になっているとき、バックアップも暗号化されます。 新しい Azure SQL データベースはすべて、既定で TDE が有効になった状態で構成されます。 TDE に関する詳細については、「[Azure SQL Database での Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)」をご覧ください。

## <a name="next-steps"></a>次の手順

- データの不慮の破損または削除から保護するデータベース バックアップは、ビジネス継続性およびディザスター リカバリー戦略の最も重要な部分です。 その他の Azure SQL Database ビジネス継続性ソリューションの概要については、[ビジネス継続性の概要](sql-database-business-continuity.md)に関する記事を参照してください。
- Azure Portal を使用して特定の時点に復元する方法については、[Azure Portal を使用したデータベースのポイントインタイム リストア](sql-database-recovery-using-backups.md)に関するページをご覧ください。
- PowerShell を使用して特定の時点に復元する方法については、[PowerShell を使用したデータベースのポイントインタイム リストア](scripts/sql-database-restore-database-powershell.md)に関するページをご覧ください。
- Azure Portal を使用して、Azure Recovery Services コンテナー内で長期保存されている自動バックアップを構成、管理、および復旧する場合、[Azure Portal を使用した長期バックアップ保存の管理](sql-database-long-term-backup-retention-configure.md)に関する記事を参照してください。
- PowerShell を使用して、Azure Recovery Services コンテナー内で長期保存されている自動バックアップを構成、管理、および復旧する場合、[PowerShell を使用した長期バックアップ保存の管理](sql-database-long-term-backup-retention-configure.md)に関する記事を参照してください。
