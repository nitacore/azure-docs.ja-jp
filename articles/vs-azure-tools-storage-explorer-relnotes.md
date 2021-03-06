---
title: "Microsoft Azure ストレージ エクスプローラー (プレビュー) のリリース ノート"
description: "Microsoft Azure ストレージ エクスプローラー (プレビュー) のリリース ノート"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: d23ddfb881695b2310d379a9112e6ab8305c0cce
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2018
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a>Microsoft Azure ストレージ エクスプローラー (プレビュー) のリリース ノート

この記事には、Azure Storage Explorer 0.9.5 (プレビュー) リリースのリリース ノート、および以前のバージョンのリリース ノートが含まれています。

[Microsoft Azure ストレージ エクスプローラー (プレビュー)](./vs-azure-tools-storage-manage-with-storage-explorer.md) は、Windows、macOS、Linux で Azure Storage データを容易に操作できるスタンドアロン アプリです。

## <a name="version-095"></a>バージョン 0.9.5
2018/02/06

### <a name="download-azure-storage-explorer-095-preview"></a>Azure Storage Explorer 0.9.5 (プレビュー) をダウンロードする
- [Windows 用 Azure Storage Explorer 0.9.5 (プレビュー)](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac 用 Azure Storage Explorer 0.9.5 (プレビュー)](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux 用 Azure Storage Explorer 0.9.5 (プレビュー)](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>新規

* ファイル共有のスナップショットのサポート:
    * ファイル共有のスナップショットを作成、管理できます。
    * ファイル共有のスナップショット間でビューを簡単に切り替えながら、内容を参照できます。
    * 以前のバージョンのファイルを復元できます。
* Azure Data Lake Store のプレビュー サポート:
    * 複数のアカウントで ADLS リソースに接続できます。
    * ADL URI を使用して ADLS リソースに接続し、リソースを共有できます。
    * 基本的なファイル/フォルダー操作を再帰的を実行できます。
    * 個々のフォルダーをクイック アクセスにピン留めできます。
    * フォルダーの統計を表示できます。

### <a name="fixes"></a>修正
* 起動パフォーマンスの改善。
* さまざまなバグの修正。

### <a name="known-issues"></a>既知の問題
* Storage Explorer では ADFS アカウントがサポートされません。
* Azure Stack を対象にしている場合、一部のファイルについては、追加 BLOB としてアップロードできない可能性があります。
* タスクの [キャンセル] をクリックすると、そのタスクのキャンセルに少し時間がかかる場合があります。 これは、ここで説明したフィルターのキャンセル回避策を使用しているためです。
* 誤った PIN/スマートカードの証明書を選択した場合、その記録をストレージ エクスプローラーから消すためには、再起動する必要があります
* サブスクリプションをフィルターするために資格情報の再入力が必要であると、アカウント設定パネルに表示されることがあります。
* BLOB の名前の変更で (個別または名前を変更する BLOB コンテナーの内部)、スナップショットが保持されません。 BLOB、ファイル、エンティティの他のすべてのプロパティとメタデータは、名前変更の間に保持されます。
* 現在、Azure Stack ではファイル共有をサポートしていませんが、ファイル共有ノードがアタッチされた Azure Stack ストレージ アカウントの下に表示され続けます。
* Storage Explorer で使用されている Electron シェルには、一部の GPU (グラフィックス処理装置) ハードウェア アクセラレータで問題が発生します。 Storage Explorer に空白 (空) のメイン ウィンドウが表示される場合は、コマンド ラインから Storage Explorer を起動し、`--disable-gpu` スイッチを追加して、GPU アクセラレータを無効にしてみてください: 

```
./StorageExplorer.exe --disable-gpu
```

* Ubuntu 14.04 のユーザーの場合、GCC が最新版であることを確認する必要があります。これは、次のコマンドを実行し、コンピューターを再起動して行います。

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 のユーザーの場合、GConf をインストールする必要があります。次のコマンドを実行し、マシンを再起動して、この操作を行うことができます。

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-094--093"></a>バージョン 0.9.4/0.9.3
2018/01/21

### <a name="download-azure-storage-explorer-094-preview"></a>Azure Storage Explorer 0.9.4 (プレビュー) をダウンロードする
* [Windows 用 Azure Storage Explorer 0.9.4 (プレビュー) のダウンロード](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Mac 用 Azure Storage Explorer 0.9.4 (プレビュー) のダウンロード](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Linux 用 Azure Storage Explorer 0.9.4 (プレビュー) のダウンロード](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a>新規
* 既存の Storage Explorer ウィンドウは以下の場合に再利用されます。
    * Storage Explorer で生成された直接リンクを開く。
    * Storage Explorer をポータルから開く。
    * Storage Explorer を Azure Storage VS Code 拡張機能 (近日公開予定) から開く。
* Storage Explorer 内から新しい Storage Explorer ウィンドウを開く機能が追加されました。
    * Windows の場合は、[ファイル] メニューとタスク バーのコンテキスト メニューに [新しいウィンドウ] オプションがあります。
    * Mac の場合は、アプリケーション メニューに [新規ウィンドウ] オプションがあります。

### <a name="fixes"></a>修正
* セキュリティの問題を修正しました。 できるだけ早く 0.9.4 にアップグレードしてください。
* 古いアクティビティが適切にクリーンアップされませんでした。 これは長時間実行ジョブのパフォーマンスに影響していました。 現在は適切にクリーンアップされるようになっています。
* 大量のファイルとディレクトリに関連するアクションによって、Storage Explorer がフリーズすることがありました。 Azure へのファイル共有の要求は、システム リソースの使用を制限するように調節されるようになりました。

### <a name="known-issues"></a>既知の問題
* Storage Explorer では ADFS アカウントがサポートされません。
* "Explorer の表示" と "アカウント管理の表示" のショートカット キーはそれぞれ、Ctrl/Cmd + Shift + E キーと Ctrl/Cmd + Shift + A キーです。
* Azure Stack を対象にしている場合、一部のファイルについては、追加 BLOB としてアップロードできない可能性があります。
* タスクの [キャンセル] をクリックすると、そのタスクのキャンセルに少し時間がかかる場合があります。 これは、ここで説明したフィルターのキャンセル回避策を使用しているためです。
* 誤った PIN/スマートカードの証明書を選択した場合、その記録をストレージ エクスプローラーから消すためには、再起動する必要があります
* サブスクリプションをフィルターするために資格情報の再入力が必要であると、アカウント設定パネルに表示されることがあります。
* BLOB の名前の変更で (個別または名前を変更する BLOB コンテナーの内部)、スナップショットが保持されません。 BLOB、ファイル、エンティティの他のすべてのプロパティとメタデータは、名前変更の間に保持されます。
* 現在、Azure Stack ではファイル共有をサポートしていませんが、ファイル共有ノードがアタッチされた Azure Stack ストレージ アカウントの下に表示され続けます。
* Storage Explorer で使用されている Electron シェルには、一部の GPU (グラフィックス処理装置) ハードウェア アクセラレータで問題が発生します。 Storage Explorer に空白 (空) のメイン ウィンドウが表示される場合は、コマンド ラインから Storage Explorer を起動し、`--disable-gpu` スイッチを追加して、GPU アクセラレータを無効にしてみてください: 
```
./StorageExplorer --disable-gpu
```
* Ubuntu 14.04 のユーザーの場合、GCC が最新版であることを確認する必要があります。これは、次のコマンドを実行し、コンピューターを再起動して行います。

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 のユーザーの場合、GConf をインストールする必要があります。次のコマンドを実行し、マシンを再起動して、この操作を行うことができます。

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="previous-releases"></a>以前のリリース

* [バージョン 0.9.2](#version-092)
* [バージョン 0.9.1 / 0.9.0](#version-091)
* [バージョン 0.8.16](#version-0816)
* [Version 0.8.14](#version-0814)
* [バージョン 0.8.13](#version-0813)
* [バージョン 0.8.12 / 0.8.11 / 0.8.10](#version-0812--0811--0810)
* [バージョン 0.8.9 / 0.8.8](#version-089--088)
* [バージョン 0.8.7](#version-087)
* [バージョン 0.8.6](#version-086)
* [バージョン 0.8.5](#version-085)
* [バージョン 0.8.4](#version-084)
* [バージョン 0.8.3](#version-083)
* [バージョン 0.8.2](#version-082)
* [バージョン 0.8.0](#version-080)
* [バージョン 0.7.20160509.0](#version-07201605090)
* [バージョン 0.7.20160325.0](#version-07201603250)
* [バージョン 0.7.20160129.1](#version-07201601291)
* [バージョン 0.7.20160105.0](#version-07201601050)
* [バージョン 0.7.20151116.0](#version-07201511160)

## <a name="version-092"></a>バージョン 0.9.2
11/01/2017

### <a name="hotfixes"></a>修正プログラム
* ローカル タイム ゾーンによっては、テーブル エンティティの Edm.DateTime 値を編集する時に、予期しないデータの変更が発生する可能性がありました。 エディターは現在プレーンテキスト ボックスを使用しており、Edm.DateTime 値の制御は正確で一貫性があります。
* 名前とキーを付けると BLOB のグループのアップロード/ダウンロードが起動しませんでした。 この問題は修正されています。
* これまで Storage Explorer では、1 つまたは複数のアカウントのサブスクリプションが選択された場合に、古いアカウントの再認証のダイアログを表示するだけでした。 現在では Storage Explorer は、アカウントが完全に除外された場合でもダイアログを表示します。
* Azure US Government のエンドポイントのドメインに誤りがありました。 この問題は修正されています。
* アカウントの管理パネルの適用ボタンをクリックしにくい場合がありました。 この問題はもう発生しません。

### <a name="new"></a>新規
* Azure Cosmos DB のプレビューのサポート:
    * [オンライン ドキュメント](./cosmos-db/storage-explorer.md)
    * データベースとコレクションの作成
    * データの操作
    * ドキュメントの照会、作成、または削除
    * ストアド プロシージャ、ユーザー定義関数、またはトリガーの更新
    * 接続文字列を使用したデータベース接続と管理
* 多数の小さな BLOB をアップロード/ダウンロードするときのパフォーマンスが向上しました。
* "すべて再試行" アクションが追加されました (BLOB アップロード グループまたは BLOB ダウンロード グループでのエラー発生時)。
* Storage Explorer で BLOB をアップロード/ダウンロード中、ネットワーク接続が失われたことが検出されると、イテレーションが一時停止します。 ネットワーク接続が再確立された時点で、イテレーションを再開できます。
* コンテキスト メニューから、タブに対して "すべて閉じる"、"その他を閉じる"、"閉じる" 操作を行うことができるようになりました。
* Storage Explorer で、ネイティブ ダイアログとネイティブ コンテキスト メニューを利用できます。
* Storage Explorer が、さらにアクセスしやすくなりました。 機能強化は次のとおりです。
    * スクリーン リーダー (Windows の NVDA、Mac の VoiceOver) のサポートの強化
    * ハイ コントラスト テーマの向上
    * キーボードの Tab キーを使った移動とキーボード フォーカスの修正

### <a name="fixes"></a>修正
* 無効な Windows ファイル名の BLOB を開こうとしたとき、またはダウンロードしようとしたときに、操作が失敗しました。 Storage Explorer によって BLOB 名が無効かどうかが検出され、エンコードするか、BLOB をスキップするかが尋ねられます。 また、Storage Explorer によって、ファイル名がエンコードされている可能性があるかどうかも検出され、アップロードの前にデコードするかどうかを尋ねられます。
* BLOB アップロード中、ターゲット BLOB コンテナーのエディターが正しく更新されないことがありました。 この問題は修正されています。
* 接続文字列と SAS URI の形式のいくつかについて、サポートが後退していました。 既知の問題はすべて解決されていますが、さらに問題が発生した場合は、フィードバックをお送りください。
* 更新の通知が、0.9.0 の一部のユーザーに送信されませんでした。 この問題は修正されました。バグの影響を受けた方は、Storage Explorer の最新バージョンを[こちら](https://azure.microsoft.com/en-us/features/storage-explorer/)からダウンロードしてください。

### <a name="known-issues"></a>既知の問題
* Storage Explorer では ADFS アカウントがサポートされません。
* "Explorer の表示" と "アカウント管理の表示" のショートカット キーはそれぞれ、Ctrl/Cmd + Shift + E キーと Ctrl/Cmd + Shift + A キーです。
* Azure Stack を対象にしている場合、一部のファイルについては、追加 BLOB としてアップロードできない可能性があります。
* タスクの [キャンセル] をクリックすると、そのタスクのキャンセルに少し時間がかかる場合があります。 これは、ここで説明したフィルターのキャンセル回避策を使用しているためです。
* 誤った PIN/スマートカードの証明書を選択した場合、その記録をストレージ エクスプローラーから消すためには、再起動する必要があります
* サブスクリプションをフィルターするために資格情報の再入力が必要であると、アカウント設定パネルに表示されることがあります。
* BLOB の名前の変更で (個別または名前を変更する BLOB コンテナーの内部)、スナップショットが保持されません。 BLOB、ファイル、エンティティの他のすべてのプロパティとメタデータは、名前変更の間に保持されます。
* 現在、Azure Stack ではファイル共有をサポートしていませんが、ファイル共有ノードがアタッチされた Azure Stack ストレージ アカウントの下に表示され続けます。
* Storage Explorer で使用されている Electron シェルには、一部の GPU (グラフィックス処理装置) ハードウェア アクセラレータで問題が発生します。 Storage Explorer に空白 (空) のメイン ウィンドウが表示される場合は、コマンド ラインから Storage Explorer を起動し、`--disable-gpu` スイッチを追加して、GPU アクセラレータを無効にしてみてください: 
```
./StorageExplorer --disable-gpu
```
* Ubuntu 14.04 のユーザーの場合、GCC が最新版であることを確認する必要があります。これは、次のコマンドを実行し、コンピューターを再起動して行います。

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 のユーザーの場合、GConf をインストールする必要があります。次のコマンドを実行し、マシンを再起動して、この操作を行うことができます。

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-091--090-preview"></a>バージョン 0.9.1/0.9.0 (プレビュー)
10/20/2017
### <a name="new"></a>新規
* Azure Cosmos DB のプレビューのサポート:
    * [オンライン ドキュメント](./cosmos-db/storage-explorer.md)
    * データベースとコレクションの作成
    * データの操作
    * ドキュメントの照会、作成、または削除
    * ストアド プロシージャ、ユーザー定義関数、またはトリガーの更新
    * 接続文字列を使用したデータベース接続と管理
* 多数の小さな BLOB をアップロード/ダウンロードするときのパフォーマンスが向上しました。
* "すべて再試行" アクションが追加されました (BLOB アップロード グループまたは BLOB ダウンロード グループでのエラー発生時)。
* Storage Explorer で BLOB をアップロード/ダウンロード中、ネットワーク接続が失われたことが検出されると、イテレーションが一時停止します。 ネットワーク接続が再確立された時点で、イテレーションを再開できます。
* コンテキスト メニューから、タブに対して "すべて閉じる"、"その他を閉じる"、"閉じる" 操作を行うことができるようになりました。
* Storage Explorer で、ネイティブ ダイアログとネイティブ コンテキスト メニューを利用できます。
* Storage Explorer が、さらにアクセスしやすくなりました。 機能強化は次のとおりです。
    * スクリーン リーダー (Windows の NVDA、Mac の VoiceOver) のサポートの強化
    * ハイ コントラスト テーマの向上
    * キーボードの Tab キーを使った移動とキーボード フォーカスの修正

### <a name="fixes"></a>修正
* 無効な Windows ファイル名の BLOB を開こうとしたとき、またはダウンロードしようとしたときに、操作が失敗しました。 Storage Explorer によって BLOB 名が無効かどうかが検出され、エンコードするか、BLOB をスキップするかが尋ねられます。 また、Storage Explorer によって、ファイル名がエンコードされている可能性があるかどうかも検出され、アップロードの前にデコードするかどうかを尋ねられます。
* BLOB アップロード中、ターゲット BLOB コンテナーのエディターが正しく更新されないことがありました。 この問題は修正されています。
* 接続文字列と SAS URI の形式のいくつかについて、サポートが後退していました。 既知の問題はすべて解決されていますが、さらに問題が発生した場合は、フィードバックをお送りください。
* 更新の通知が、0.9.0 の一部のユーザーに送信されませんでした。 この問題は修正されました。バグの影響を受けた方は、Storage Explorer の最新バージョンを[こちら](https://azure.microsoft.com/en-us/features/storage-explorer/)からダウンロードしてください

### <a name="known-issues"></a>既知の問題
* Storage Explorer では ADFS アカウントがサポートされません。
* "Explorer の表示" と "アカウント管理の表示" のショートカット キーはそれぞれ、Ctrl/Cmd + Shift + E キーと Ctrl/Cmd + Shift + A キーです。
* Azure Stack を対象にしている場合、一部のファイルについては、追加 BLOB としてアップロードできない可能性があります。
* タスクの [キャンセル] をクリックすると、そのタスクのキャンセルに少し時間がかかる場合があります。 これは、ここで説明したフィルターのキャンセル回避策を使用しているためです。
* 誤った PIN/スマートカードの証明書を選択した場合、その記録をストレージ エクスプローラーから消すためには、再起動する必要があります
* サブスクリプションをフィルターするために資格情報の再入力が必要であると、アカウント設定パネルに表示されることがあります。
* BLOB の名前の変更で (個別または名前を変更する BLOB コンテナーの内部)、スナップショットが保持されません。 BLOB、ファイル、エンティティの他のすべてのプロパティとメタデータは、名前変更の間に保持されます。
* 現在、Azure Stack ではファイル共有をサポートしていませんが、ファイル共有ノードがアタッチされた Azure Stack ストレージ アカウントの下に表示され続けます。
* Storage Explorer で使用されている Electron シェルには、一部の GPU (グラフィックス処理装置) ハードウェア アクセラレータで問題が発生します。 Storage Explorer に空白 (空) のメイン ウィンドウが表示される場合は、コマンド ラインから Storage Explorer を起動し、`--disable-gpu` スイッチを追加して、GPU アクセラレータを無効にしてみてください: 
```
./StorageExplorer --disable-gpu
```
* Ubuntu 14.04 のユーザーの場合、GCC が最新版であることを確認する必要があります。これは、次のコマンドを実行し、コンピューターを再起動して行います。

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 のユーザーの場合、GConf をインストールする必要があります。次のコマンドを実行し、マシンを再起動して、この操作を行うことができます。

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0816"></a>バージョン 0.8.16
8/21/2017

### <a name="new"></a>新規
* 変更が検出された場合は、BLOB を開いたときに、ダウンロードしたファイルをアップロードするようストレージ エクスプ ローラーから求められます。
* 改善された Azure Stack サインイン エクスペリエンス
* 同時に多数の小さなファイルをアップロード/ダウンロードしたときのパフォーマンスの向上


### <a name="fixes"></a>修正
* BLOB の種類によっては、アップロード競合中に "置換" を選択すると、アップロードが再開されることがあります。
* バージョン 0.8.15 では、アップロードが 99% 完了した時点で停止する場合があります。
* ファイル共有にファイルをアップロードする場合、まだ存在しないディレクトリへのアップロードを選択すると、アップロードは失敗します。
* ストレージ エクスプ ローラーで共有アクセス署名とテーブル クエリに対するタイムスタンプが正しく生成されませんでした。


### <a name="known-issues"></a>既知の問題
* 名前とキー接続文字列は現在使用できません。 この問題は次のリリースで修正される予定です。 それまでは、名前とキーの添付を使用できます。
* 無効な Windows ファイル名のファイルを開こうとすると、ダウンロード中にファイルが見つからないというエラーが発生します。
* タスクの [キャンセル] をクリックすると、そのタスクのキャンセルに少し時間がかかる場合があります。 これは、Azure Storage ノード ライブラリの制限事項です。
* BLOB のアップロードが完了したら、アップロードを開始するタブが更新されます。 これは以前の動作から変更されており、選択しているコンテナーのルートに戻ることになります。
* 誤った PIN/スマートカードの証明書を選択した場合、その記録をストレージ エクスプローラーから消すためには、再起動する必要があります
* サブスクリプションをフィルターするために資格情報の再入力が必要であると、アカウント設定パネルに表示されることがあります。
* BLOB の名前の変更で (個別または名前を変更する BLOB コンテナーの内部)、スナップショットが保持されません。 BLOB、ファイル、エンティティの他のすべてのプロパティとメタデータは、名前変更の間に保持されます。
* 現在、Azure Stack ではファイル共有をサポートしていませんが、ファイル共有ノードがアタッチされた Azure Stack ストレージ アカウントの下に表示され続けます。
* Ubuntu 14.04 のユーザーの場合、GCC が最新版であることを確認する必要があります。これは、次のコマンドを実行し、コンピューターを再起動して行います。

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 のユーザーの場合、GConf をインストールする必要があります。次のコマンドを実行し、マシンを再起動して、この操作を行うことができます。

    ```
    sudo apt-get install libgconf-2-4
    ```

### <a name="version-0814"></a>Version 0.8.14
06/22/2017

### <a name="new"></a>新規

* いくつかの重要なセキュリティ アップデートを利用するために、Electron バージョンを 1.7.2 に更新しました
* ヘルプ メニューからオンラインのトラブルシューティング ガイドにすばやくアクセスできるようになりました
* ストレージ エクスプローラーのトラブルシューティング [ガイド][2]
* Azure Stack サブスクリプションへの接続に関する[指示][3]

### <a name="known-issues"></a>既知の問題

* フォルダーの削除の確認ダイアログ上のボタンは、Linux のマウス クリックに登録されません。 Enter キーを使用して回避することができます
* 誤った PIN/スマートカードの証明書を選択した場合、ストレージ エクスプローラーがその決定を忘れるように、再起動する必要があります
* BLOB またはファイルのグループを 3 つ以上同時にアップロードすると、エラーが発生する場合があります
* サブスクリプションをフィルターするために、資格情報の再入力が必要であることがアカウント設定パネルに表示されることがあります
* BLOB の名前の変更で (個別または名前を変更する BLOB コンテナーの内部)、スナップショットが保持されません。 BLOB、ファイル、エンティティの他のすべてのプロパティとメタデータは、名前変更の間に保持されます。
* 現在、Azure Stack ではファイル共有をサポートしていませんが、ファイル共有ノードがアタッチされた Azure Stack ストレージ アカウントの下に表示され続けます。
* Ubuntu 14.04 のインストールには、更新またはアップグレードされた gcc バージョンが必要です。アップグレードの手順は次のとおりです。

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

### <a name="version-0813"></a>バージョン 0.8.13
05/12/2017

#### <a name="new"></a>新規

* ストレージ エクスプローラーのトラブルシューティング [ガイド][2]
* Azure Stack サブスクリプションへの接続に関する[指示][3]

#### <a name="fixes"></a>修正

* 修正: ファイルのアップロードは、メモリ不足エラーを発生させる高い可能性がありました
* 修正: PIN/スマートカードでサインインできるようになりました
* 修正: [ポータルで開く] が、Azure 中国、Azure ドイツ、Azure 米国政府機関、Azure Stack で機能するようになりました
* 修正: フォルダーを BLOB コンテナーにアップロードしているときに、"無効な操作" エラーが発生する場合があります
* 修正: スナップショットを管理しているときに、[すべて選択] が無効になっていました
* 修正: ベース BLOB のメタデータは、そのスナップショットのプロパティを表示した後に上書きされる可能性があります

#### <a name="known-issues"></a>既知の問題

* 誤った PIN/スマートカードの証明書を選択した場合、ストレージ エクスプローラーがその決定を忘れるように、再起動する必要があります
* 拡大縮小を行っているときに、ズーム レベルが既定のレベルにすぐにリセットされる可能性があります
* BLOB またはファイルのグループを 3 つ以上同時にアップロードすると、エラーが発生する場合があります
* サブスクリプションをフィルターするために、資格情報の再入力が必要であることがアカウント設定パネルに表示されることがあります
* BLOB の名前の変更で (個別または名前を変更する BLOB コンテナーの内部)、スナップショットが保持されません。 BLOB、ファイル、エンティティの他のすべてのプロパティとメタデータは、名前変更の間に保持されます。
* 現在、Azure Stack ではファイル共有をサポートしていませんが、ファイル共有ノードがアタッチされた Azure Stack ストレージ アカウントの下に表示され続けます。
* Ubuntu 14.04 のインストールには、更新またはアップグレードされた gcc バージョンが必要です。アップグレードの手順は次のとおりです。

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812--0811--0810"></a>バージョン 0.8.12 / 0.8.11 / 0.8.10
04/07/2017

#### <a name="new"></a>新規

* 更新通知から更新プログラムをインストールすると、ストレージ エクスプローラーは自動的に閉じます
* インプレース クイック アクセスでは、頻繁にアクセスされるリソースを操作するために、拡張されたエクスペリエンスを提供します
* BLOB コンテナー エディターで、リース BLOB が属している仮想マシンを確認できるようになりました
* 左側のパネルを折りたためるようになりました
* 検出がダウンロードと同時に実行されるようになりました
* BLOB コンテナー、ファイル共有、およびテーブル エディターの統計情報を使用して、リソースまたは選択範囲のサイズを確認します
* Azure Active Directory (AAD) を基にした Azure Stack アカウントにサインインできるようになりました。
* 32MB を超えるアーカイブ ファイルを Premium ストレージ アカウントにアップロードできるようになりました
* アクセシビリティ サポートの向上
* [編集] &gt; [SSL 証明書] &gt; [証明書のインポート] に移動して、信頼できる Base 64 encoded X.509 SSL 証明書を追加できるようになりました

#### <a name="fixes"></a>修正

* 修正: アカウントの資格情報を更新した後に、ツリー ビューが自動的に更新されないことがありました
* 修正: エミュレーター キューとテーブルに SAS を生成すると、無効な URL になる場合がありました
* 修正: プロキシが有効なときに、Premium Storage アカウントを展開できるようになりました
* 修正: 選択したアカウントが 1 または 0 個の場合、アカウント管理ページの適用ボタンが機能しませんでした
* 修正: 競合の解決を必要とする BLOB のアップロードに失敗する場合があります - 0.8.11 で修正済み
* 修正: 0.8.11 でフィードバックの送信が中断されました - 0.8.12 で修正済み

#### <a name="known-issues"></a>既知の問題

* 0.8.10 にアップグレードした後に、すべての資格情報を更新する必要があります。
* 拡大縮小を行っているときに、ズーム レベルが既定のレベルにすぐにリセットされる可能性があります。
* BLOB またはファイルのグループを 3 つ以上同時にアップロードすると、エラーが発生する場合があります。
* サブスクリプションをフィルターするために、資格情報の再入力が必要であることがアカウント設定パネルに表示されることがあります。
* BLOB の名前の変更で (個別または名前を変更する BLOB コンテナーの内部)、スナップショットが保持されません。 BLOB、ファイル、エンティティの他のすべてのプロパティとメタデータは、名前変更の間に保持されます。
* 現在、Azure Stack ではファイル共有をサポートしていませんが、ファイル共有ノードがアタッチされた Azure Stack ストレージ アカウントの下に表示され続けます。
* Ubuntu 14.04 のインストールには、更新またはアップグレードされた gcc バージョンが必要です。アップグレードの手順は次のとおりです。

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089--088"></a>バージョン 0.8.9 / 0.8.8
02/23/2017

>[!VIDEO https://www.youtube.com/embed/R6gonK3cYAc?ecver=1]

>[!VIDEO https://www.youtube.com/embed/SrRPCm94mfE?ecver=1]


#### <a name="new"></a>新規

* ストレージ エクスプローラー 0.8.9 では、更新プログラムの最新バージョンを自動的にダウンロードします。
* 修正プログラム: ポータルで生成された SAS URL を使用して、ストレージ アカウントをアタッチすると、エラーになります。
* BLOB スナップショットの作成、管理、および昇格を行えるようになりました。
* Azure 中国、Azure ドイツ、Azure 米国政府機関のアカウントにサインインできるようになりました。
* ズーム レベルを変更できるようになりました。 [表示] メニューのオプションを使用して、拡大、縮小、およびズームのリセットを行います。
* Unicode 文字が、BLOB とファイルのユーザー メタデータでサポートされるようになりました。
* アクセシビリティが向上しました。
* 次のバージョンのリリース ノートは、更新通知から表示できます。 [ヘルプ] メニューから最新のリリース ノートを表示することもできます。

#### <a name="fixes"></a>修正

* 修正: バージョン番号が、Windows のコントロール パネルで正しく表示されるようになりました
* 修正: 検索の 50,000 ノードまでの制限がなくなりました
* 修正: 対象になるディレクトリが存在しない場合に、ファイル共有へのアップロードが回転し続けます
* 修正: 長時間のアップロードとダウンロードの安定性が向上しました

#### <a name="known-issues"></a>既知の問題

* 拡大縮小を行っているときに、ズーム レベルが既定のレベルにすぐにリセットされる可能性があります。
* クイック アクセスは、サブスクリプション ベースのアイテムでのみ動作します。 このリリースでは、ローカル リソースと、キーまたは SAS トークンによってアタッチされたリソースは、サポートされていません。
* リソースの数によっては、クイック アクセスで対象リソースに移動するまでに数秒かかる場合があります。
* BLOB またはファイルのグループを 3 つ以上同時にアップロードすると、エラーが発生する場合があります。

12/16/2016
### <a name="version-087"></a>バージョン 0.8.7

>[!VIDEO https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1]

#### <a name="new"></a>新規

* [アクティビティ] ウィンドウで更新、ダウンロード、またはコピー セッションの開始時に競合を解決する方法を選ぶことができます
* タブをポイントすると、ストレージ リソースの完全なパスが表示されます
* タブをクリックすると、左側のナビゲーション ウィンドウ内の場所と同期されます

#### <a name="fixes"></a>修正

* 修正: ストレージ エクスプローラーは、Mac の信頼できるアプリになりました
* 修正: Ubuntu 14.04 が再びサポートされるようになりました
* 修正: サブスクリプションを読み込むときに [アカウントの追加] UI が点滅することがありました
* 修正: 左側のナビゲーション ウィンドウにすべてのストレージ リソースが一覧表示されないことがありました
* 修正: 操作ウィンドウに空の操作が表示されることがありました
* 修正: 前回終了したセッションからウィンドウ サイズが維持されるようになりました
* 修正: コンテキスト メニューを使って同じリソースに対して複数のタブを開くことができるようになりました

#### <a name="known-issues"></a>既知の問題

* クイック アクセスは、サブスクリプション ベースのアイテムでのみ動作します。 このリリースでは、ローカル リソースと、キーまたは SAS トークンによってアタッチされたリソースは、サポートされていません
* リソースの数によっては、クイック アクセスで対象リソースに移動するまでに数秒かかる場合があります
* BLOB またはファイルのグループを 3 つ以上同時にアップロードすると、エラーが発生する場合があります
* 検索によって処理されたノードが約 50,000 を超えると、パフォーマンスが低下するか、またはハンドルされない例外が発生する可能性があります
* macOS でストレージ エクスプローラーを初めて使うとき、キーチェーンにアクセスするためのユーザーのアクセス許可を確認する複数のプロンプトが表示される可能性があります。 プロンプトが繰り返し表示されないように、[常に許可] を選ぶことをお勧めします

11/18/2016
### <a name="version-086"></a>バージョン 0.8.6

#### <a name="new"></a>新規

* 最もよく使うサービスを簡単にアクセスできるようにクイック アクセスにピン留めできます
* 複数のエディターを別のタブで開けるようになりました。 シングルクリックで一時的なタブが開き、ダブルクリックで永続的なタブが開きます。一時的なタブをクリックして永続的なタブにすることもできます
* アップロードとダウンロードでのパフォーマンスと安定性を大幅に向上させました (特に高速コンピューターでの大きいファイルの場合)
* 空の "仮想" フォルダーを BLOB コンテナーに作成できます
* 新しく拡張されたサブ文字列の検索で範囲指定の検索を再導入したため、検索には次の 2 つのオプションがあります。
    * グローバル検索 - 検索用語を検索テキストボックスに入力するだけです
    * 範囲指定の検索 - ノードの横にある虫眼鏡アイコンをクリックして、パスの末尾に検索用語を追加するか、右クリックして [ここから検索] を選択します
* さまざまなテーマを追加しました。明るい (既定値)、暗い、ハイコントラスト黒、ハイ コントラスト白。 [編集] &gt; [テーマ] を選んで、テーマの設定を変更します
* BLOB とファイルのプロパティを変更できます
* エンコードされた (Base64) キュー メッセージとエンコードされていないキュー メッセージをサポートするようになりました
* Linux では、64 ビット OS が必要になりました。 このリリースでは、 64 ビット Ubuntu 16.04.1 LTS のみをサポートします
* ロゴを更新しました。

#### <a name="fixes"></a>修正

* 修正: 画面がフリーズする問題
* 修正: セキュリティの強化
* 修正: アタッチされたアカウントが重複して表示されることがあります
* 修正: 定義されていないコンテンツ タイプを持つ BLOB は、例外を生成する場合があります
* 修正: 空のテーブルでクエリ パネルを開くことができませんでした
* 修正: 検索でのさまざまなバグ
* 修正: [Load More]\(さらに読み込む\) をクリックするときに読み込むリソース数が、50 から 100 に増えました
* 修正: 最初の実行で、アカウントがサインインされた場合、既定でアカウントにすべてのサブスクリプションを選択するようになりました

#### <a name="known-issues"></a>既知の問題

* ストレージ エクスプローラーのこのリリースは、Ubuntu 14.04 で実行されません
* 同じリソースに対して複数のタブを開くために、同じリソースを連続してクリックしないでください。 別のリソースをクリックし、元のリソースに戻ってクリックし、別のタブでもう一度開きます
* クイック アクセスは、サブスクリプション ベースのアイテムでのみ動作します。 このリリースでは、ローカル リソースと、キーまたは SAS トークンによってアタッチされたリソースは、サポートされていません
* リソースの数によっては、クイック アクセスで対象リソースに移動するまでに数秒かかる場合があります
* BLOB またはファイルのグループを 3 つ以上同時にアップロードすると、エラーが発生する場合があります
* 検索によって処理されたノードが約 50,000 を超えると、パフォーマンスが低下するか、またはハンドルされない例外が発生する可能性があります

10/03/2016
### <a name="version-085"></a>バージョン 0.8.5

#### <a name="new"></a>新規

* ポータルで生成された SAS キーを使用して、ストレージ アカウントやリソースにアタッチできるようになりました

#### <a name="fixes"></a>修正

* 修正: 検索中の競合状態は、ノードが展開不可能になることがあります
* 修正: アカウント名とキーでストレージ アカウントに接続している場合、"HTTP の使用" は動作しません
* 修正: SAS キー (特にポータルで生成されたキー) は、"末尾のスラッシュ" エラーを返します
* 修正: テーブルのインポートの問題
    * パーティション キーと行キーが逆順になる場合がありました
    * "null" のパーティション キーを読み取ることができません

#### <a name="known-issues"></a>既知の問題

* 検索によって処理されたノードが約 50,000 を超えると、パフォーマンスが低下する可能性があります
* Azure Stack は現在ファイルをサポートしないので、[ファイル] を展開しようとするとエラーが表示されます

09/12/2016
### <a name="version-084"></a>バージョン 0.8.4

>[!VIDEO https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1]

#### <a name="new"></a>新規

* リソースの共有と簡単なアクセスのため、ストレージ アカウントまたはコンテナー、キュー、テーブル、ファイル共有に直接リンクします (Windows と Mac OS でサポート)
* 検索ボックスから BLOB コンテナー、テーブル、キュー、ファイル共有、またはストレージ アカウントを検索します
* テーブル キュー ビルダーで句をグループ化できまるようになりました
* BLOB コンテナー、ファイル共有、テーブル、BLOB、BLOB フォルダー、ファイルおよびディレクトリの名前を変更したり、SAS がアタッチされたアカウントとコンテナー内からコピーまたは貼り付けを行います
* BLOB コンテナーとファイル共有の名前を変更してコピーすると、プロパティとメタデータを保持するようになりました

#### <a name="fixes"></a>修正

* 修正: ブール値またはバイナリ プロパティが含まれる場合は、テーブル エンティティを編集できません

#### <a name="known-issues"></a>既知の問題

* 検索によって処理されたノードが約 50,000 を超えると、パフォーマンスが低下する可能性があります

08/03/2016
### <a name="version-083"></a>バージョン 0.8.3

>[!VIDEO https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1]

#### <a name="new"></a>新規

* コンテナー、テーブル、ファイル共有の名前を変更します
* クエリ ビルダーのエクスペリエンスが向上しました
* クエリの保存および読み込み機能
* リソースの共有と簡単なアクセスのため、ストレージ アカウントまたはコンテナー、キュー、テーブル、ファイル共有に直接リンクします (Windows のみ、macOS のサポートは近日公開予定です)
* CORS ルールを管理および構成する機能

#### <a name="fixes"></a>修正

* 修正: Microsoft アカウントは、8 ～ 12 時間ごとに再認証が必要です

#### <a name="known-issues"></a>既知の問題

* UI がフリーズして表示されることがあります。ウィンドウを最大化すると、この問題を解決することができます
* macOS のインストールには管理者アクセス許可が必要な場合があります
* サブスクリプションをフィルターするために、資格情報の再入力が必要であることがアカウント設定パネルに表示されることがあります
* ファイル共有、BLOB コンテナー、およびテーブルの名前を変更しても、ファイル共有のクォータ、パブリック アクセス レベル、またはアクセス ポリシーなど、コンテナーのメタデータやその他のプロパティを保持することはありません
* BLOB の名前の変更で (個別または名前を変更する BLOB コンテナーの内部)、スナップショットが保持されません。 BLOB、ファイル、エンティティの他のすべてのプロパティとメタデータは、名前変更の間に保持されます
* リソースのコピーまたは名前の変更は、SAS がアタッチされたアカウントで機能しません

07/07/2016
### <a name="version-082"></a>バージョン 0.8.2

>[!VIDEO https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1]

#### <a name="new"></a>新規

* ストレージ アカウントはサブスクリプションごとにグループ化されます。キーまたは SAS によってアタッチされた開発ストレージとリソースは、(ローカルと接続されている) ノードに表示されます
* [Azure アカウントの設定] パネルのアカウントからサインオフします
* サインインを有効にして管理するように、プロキシ設定を構成します
* BLOB リースの作成と中断
* シングルクリックで、BLOB コンテナー、キュー、テーブル、およびファイルを開きます

#### <a name="fixes"></a>修正

* 修正: .NET または Java ライブラリで挿入されたキュー メッセージは、Base64 から適切にデコードされません
* 修正: $metrics テーブルは、Blob Storage アカウントに表示されません
* 修正: テーブル ノードはローカル (開発) ストレージで機能しません

#### <a name="known-issues"></a>既知の問題

* macOS のインストールには管理者アクセス許可が必要な場合があります

06/15/2016
### <a name="version-080"></a>バージョン 0.8.0

>[!VIDEO https://www.youtube.com/embed/ycfQhKztSIY?ecver=1]

>[!VIDEO https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1]

>[!VIDEO https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1]

#### <a name="new"></a>新規

* ファイル共有のサポート: ファイルとディレクトリ、SAS URI (作成および接続) の表示、アップロード、ダウンロード、コピー
* SAS URI またはアカウント キーでストレージに接続するためのユーザー エクスペリエンスが向上しました
* テーブル クエリの結果をエクスポートします
* テーブルの列の並べ替えとカスタマイズ
* メトリックスを有効にしたストレージ アカウントの $logs BLOB コンテナーと $metrics テーブルの表示
* 改善されたエクスポートとインポートの動作にプロパティの値の種類が含まれるようになりました

#### <a name="fixes"></a>修正

* 修正: 大きい BLOB のアップロードまたはダウンロードは、不完全なアップロードまたはダウンロードになります
* 修正: 数値文字列 ("1") を含むエンティティの編集、追加、またはインポートは、double に変換されます
* 修正: ローカル開発環境のテーブル ノードを展開できません

#### <a name="known-issues"></a>既知の問題

* $metrics テーブルが、Blob Storage アカウントに表示されません
* Base64 エンコーディングを使用してメッセージがエンコードされている場合、プログラムを使用して追加されたキュー メッセージが正しく表示されない可能性があります

05/17/2016
### <a name="version-07201605090"></a>バージョン 0.7.20160509.0

#### <a name="new"></a>新規

* アプリ クラッシュのエラー処理が改善されました

#### <a name="fixes"></a>修正

* サインインの資格情報が必要なときに、情報バーのメッセージが表示されないバグが修正されました

#### <a name="known-issues"></a>既知の問題

* テーブル: あいまいな数値 ("1"、"1.0" など) を含むプロパティがあるエンティティを追加、編集、またはインポートしている場合、ユーザーは `Edm.String` としてエンティティを送信しようとし、値は Edm.Double としてクライアント API によって戻されます

03/31/2016

### <a name="version-07201603250"></a>バージョン 0.7.20160325.0

>[!VIDEO https://www.youtube.com/embed/imbgBRHX65A?ecver=1]

>[!VIDEO https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1]

#### <a name="new"></a>新規

* テーブルのサポート: エンティティの表示、クエリ、エクスポート、インポート、および CRUD 操作
* キューのサポート: メッセージの表示、追加、デキュー
* ストレージ アカウントの SAS URI の生成
* SAS URI でストレージ アカウントに接続
* ストレージ エクスプローラーへの今後の更新プログラムの通知を更新します
* 外観を更新しました

#### <a name="fixes"></a>修正

* パフォーマンスと信頼性の向上

### <a name="known-issues-amp-mitigations"></a>既知の問題 &amp; 軽減策

* 大きい BLOB ファイルのダウンロードは、現在機能しません - Microsoft がこの問題に対処している間は、AzCopy を使用することをお勧めします
* ホーム フォルダーが見つからない、または書き込めない場合に、アカウントの資格情報が取得またはキャッシュされることはありません
* あいまいな数値 ("1"、"1.0" など) を含むプロパティがあるエンティティを追加、編集、またはインポートしている場合は、ユーザーは `Edm.String` としてエンティティを送信しようとし、値は Edm.Double としてクライアント API によって戻されます
* 複数行のレコードを含む CSV ファイルをインポートしていると、データはチョップされたり、スクランブルをかけられる場合があります

02/03/2016

### <a name="version-07201601291"></a>バージョン 0.7.20160129.1

#### <a name="fixes"></a>修正

* BLOB をアップロード、ダウンロード、コピーするときの全体的なパフォーマンスが改善されました

01/14/2016

### <a name="version-07201601050"></a>バージョン 0.7.20160105.0

#### <a name="new"></a>新規

* Linux Support (OSX へのパリティ機能)
* Shared Access Signatures (SAS) キーを持つ BLOB コンテナーを追加します
* Azure 中国用にストレージ アカウントを追加します
* カスタム エンドポイントを持つストレージ アカウントを追加します
* コンテンツ テキストと画像 BLOB を開いて表示します
* BLOB のプロパティとメタデータを表示および編集します

#### <a name="fixes"></a>修正

* 修正: 大量の BLOB (500 以上) をアップロードまたはダウンロードすると、アプリの画面が白くなることがあります
* 修正: BLOB コンテナーをパブリック アクセス レベルに設定すると、コンテナーのフォーカスをリセットするまで新しい値は更新されません。 また、ダイアログは常に既定の "パブリック アクセスはありません" に設定され、実際の現在の値にはなりません。
* キーボードまたはアクセシビリティと UI のサポートが全体的に改善されました
* 階層リンクの履歴テキストが空白で長い場合は、折り返されます
* SAS ダイアログでは、入力の検証をサポートします
* ユーザーの資格情報の期限が切れた場合でも、ローカル ストレージがそのまま有効になります
* 開かれている BLOB コンテナーを削除すると、右側の BLOB エクスプローラーが閉じられます

#### <a name="known-issues"></a>既知の問題

* Linux のインストールには、更新またはアップグレードされた gcc バージョンが必要です。アップグレードの手順は次のとおりです。
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>バージョン 0.7.20151116.0

#### <a name="new"></a>新規

* macOS、Windows バージョン
* サインインしてストレージ アカウントを表示します。組織のアカウント、Microsoft アカウント、2FA などを使用します。
* ローカル開発ストレージ (ストレージ エミュレーターを使用、Windows のみ)
* Azure Resource Manager およびクラシック リソースのサポート
* BLOB、キュー、またはテーブルを作成および削除します
* 特定の BLOB、キュー、またはテーブルを検索します
* BLOB コンテナーの内容を探索します
* ディレクトリ内を表示して移動します
* BLOB とフォルダーをアップロード、ダウンロード、削除します
* BLOB のプロパティとメタデータを表示および編集します
* SAS キーを生成します
* 保存されているアクセス ポリシー (SAP) を管理および作成します
* プレフィックスで BLOB を検索します
* ドラッグ アンド ドロップでファイルをアップロードまたはダウンロードします

#### <a name="known-issues"></a>既知の問題

* BLOB コンテナーをパブリック アクセス レベルに設定すると、コンテナーのフォーカスをリセットするまで新しい値は更新されません
* ダイアログを開いてパブリック アクセス レベルを設定した場合、常に既定の "パブリック アクセスはありません" と表示され、実際の現在の値ではありません
* ダウンロードされた BLOB の名前を変更できません
* エラーが発生したときに、アクティビティ ログ エントリで進行中の状態に "スタック" を受け取ることがあり、そのエラーは表示されません
* 大量の BLOB をアップロードまたはダウンロードしようとすると、クラッシュしたり、完全に白くなったりする場合があります
* コピー操作のキャンセルが機能しない場合があります
* コンテナー (BLOB、キュー、テーブル) の作成中に、無効な名前を入力して、別のコンテナーの種類でもう 1 つ作成を続行した場合は、新しい種類にフォーカスを設定することはできません
* 新しいフォルダーの作成またはフォルダーの名前の変更を行うことができません




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md
