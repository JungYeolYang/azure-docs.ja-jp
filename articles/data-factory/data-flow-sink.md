---
title: Azure Data Factory の Mapping Data Flow のシンク変換
description: Azure Data Factory の Mapping Data Flow のシンク変換
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: a39fa0949276b7e86c7fdd0d0861492a9a0b723e
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438634"
---
# <a name="mapping-data-flow-sink-transformation"></a>Mapping Data Flow のシンク変換

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![シンクのオプション](media/data-flow/sink1.png "シンク 1")

データ フローの変換が完了したら、変換されたデータを宛先データセットにシンクできます。 シンク変換では、宛先の出力データに使用するデータセット定義を選択できます。 データ フローに必要なだけの数のシンク変換を設定できます。

受信データの変更に対処したり、スキーマの誤差に対処したりするための一般的な方法として、出力データセットに定義済みのスキーマがない状態で出力データをフォルダーにシンクします。 さらに、ソースで [Allow Schema Drift] (スキーマの誤差を許可) を選択してから、シンク内のすべてのフィールドを自動マップすることによって、ソース内のすべての列変更に対処できます。

データセットにシンクする場合は、データ フローを上書きするか、追加するか、または失敗させるかを選択できます。

[automap] (自動マップ) を選択して、すべての受信フィールドをシンクすることもできます。 宛先にシンクするフィールドを選択する場合や、宛先にあるフィールドの名前を変更する場合は、[automap] (自動マップ) で [オフ] を選択してから、[マッピング] タブをクリックして出力フィールドをマップします。

![シンクのオプション](media/data-flow/sink2.png "シンク 2")

## <a name="output-to-one-file"></a>1 つのファイルに出力する
Azure Storage BLOB または Data Lake のシンクの種類の場合は、変換されたデータをフォルダーに出力します。 Spark は、シンク変換で使用されるパーティション分割スキームに基づいて、パーティション分割された出力データ ファイルを生成します。 このパーティション分割スキームは、[最適化] タブをクリックして設定できます。ADF で出力を単一ファイルにマージする場合は、[単一パーティション] ラジオ ボタンをクリックします。

![シンクのオプション](media/data-flow/opt001.png "シンクのオプション")

## <a name="field-mapping"></a>フィールドのマッピング

シンク変換の [マッピング] タブでは、受信 (左側) の列を宛先 (右側) にマップできます。 データ フローをファイルにシンクした場合、ADF は常に、新しいファイルをフォルダーに書き込みます。 データベース データセットにマップする場合は、このスキーマで新しいテーブルを生成する ([Save Policy] (ポリシーの保存) を [上書き] に設定する) か、または既存のテーブルに新しい行を挿入し、それらのフィールドを既存のスキーマにマップするかのいずれかを選択できます。

マッピング テーブルで複数選択を使用すると、1 回のクリックで複数の列をリンクしたり、複数の列のリンクを削除したり、複数の行を同じ列名にマップしたりできます。

常に受信フィールド セットを取得して、ターゲットにそのままマップする場合は、[Allow Schema Drift]\(スキーマの誤差を許可\) を設定します。

![フィールドのマッピング](media/data-flow/multi1.png "複数のオプション")

列のマッピングをリセットする場合は、[Remap] (再マップ) ボタンを押してマッピングをリセットします。

![シンクのオプション](media/data-flow/sink1.png "シンク 1")

![シンクのオプション](media/data-flow/sink2.png "シンク")

* [Allow Schema Drift] (スキーマの誤差を許可) および [スキーマの検証] オプションがシンクで使用できるようになりました。 これにより、柔軟なスキーマ定義を完全に許可すか ([スキーマの誤差])、またはスキーマが変更された場合はシンクを失敗させるか ([スキーマの検証]) のどちらかを ADF に指示できるようになります。

* フォルダーをクリアします。 ADF は、宛先ファイルをターゲット フォルダーに書き込む前に、そのシンク フォルダーの内容を切り捨てます。

## <a name="file-name-options"></a>ファイル名のオプション

   * 既定値はSpark は PART の既定値に基づいて、ファイルに名前を付けることができます。
   * パターン:出力ファイルのパターンを入力します。 たとえば、"loans[n]" と入力すると、loans1.csv、loans2.csv の順に作成されます。
   * [Per partition] (パーティションごと): パーティションごとのファイル名を入力します。
   * [As data in column] (列内のデータとして): 出力ファイルを列の値に設定します。

> [!NOTE]
> ファイル操作は、データ フローの実行アクティビティを実行している場合にのみ実行され、データ フロー デバッグ モードにある間は実行されません。

## <a name="database-options"></a>データベース オプション

* 挿入、更新、削除、Upsert を許可します。 既定では、挿入を許可します。 行の更新、アップサート、または削除を行う場合は、最初に、それらの特定のアクションに対して行をタグ付けするための行の変更変換を追加する必要があります。 [挿入の許可]\(Allow insert\) をオフにすると、ADF はソースから新しい行を挿入するのを停止します。
* テーブルを切り捨てます (データ フローを完了する前に、対象のテーブルからすべての行を削除します)
* テーブルを再作成します (データ フローを完了する前に、対象のテーブルの削除と作成を行います)
* 大量データ読み込みのバッチ サイズ。 書き込みをチャンクにまとめる数値を入力します
* ステージングの有効化:これは、シンク データセットとして Azure Data Warehouse を読み込むときに Polybase を使用するように ADF に指示します

> [!NOTE]
> データ フローで、新しいテーブル名を持つデータセットをシンク変換に設定することで、ターゲット データベースに新しいテーブル定義を作成するように ADF に指示します。 SQL データセットで、テーブル名の下の [編集] をクリックして新しいテーブル名を入力します。 次に、シンク変換で、[Allow Schema Drift]\(スキーマの誤差を許可\) をオンにします。 [スキーマのインポート] を [なし] に設定します。

![ソース変換スキーマ](media/data-flow/dataset2.png "SQL スキーマ")

![SQL シンク オプション](media/data-flow/alter-row2.png "SQL オプション")

> [!NOTE]
> データベース シンク内の行を更新または削除するときに、キー列を設定する必要があります。 このようにして、行の変更で、DML 内の一意の行を特定できます。

## <a name="next-steps"></a>次の手順

これでデータ フローが作成されたので、[データ フローの実行アクティビティをパイプラインに](concepts-data-flow-overview.md)追加します。
