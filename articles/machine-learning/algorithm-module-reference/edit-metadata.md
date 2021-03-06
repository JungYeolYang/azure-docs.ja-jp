---
title: Edit Metadata (メタデータの編集):モジュール リファレンス
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning service で Edit Metadata (メタデータの編集) モジュールを使用して、データセット内の列に関連付けられているメタデータを変更する方法を学習します。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: cfee607aca155b6cf68e5bddc40eb9c752df5e34
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027727"
---
# <a name="edit-metadata-module"></a>Edit Metadata (メタデータの編集) モジュール

この記事では、Azure Machine Learning service に使用されるビジュアル インターフェイス (プレビュー) のモジュールについて説明します。

このモジュールを使用して、データセット内の列に関連付けられているメタデータを変更します。 **Edit Metadata (メタデータの編集)** モジュールの使用後、データセットの値とデータ型は変更されます。 
 
一般的なメタデータの変更には以下が含まれます。
  
+ ブール値または数値の列をカテゴリ値として扱う  
  
+ *クラス* ラベル、または分類や予測する値がどの列に含まれているかを示す  
  
+ 列をフィーチャーとしてマークする
  
+ 日付/時刻の値を数値に、またはその逆に変更する  
  
+ 列名を変更する
  
 列の定義を変更する必要がある場合はいつでも (通常はダウンストリーム モジュールの要件を満たすために)、Edit Metadata (メタデータの編集) を使用します。 たとえば、一部のモジュールでは、特定のデータ型でのみ動作したり、列でフラグ (`IsFeature` または `IsCategorical` など) を要求したりできます。  
  
 必要な操作を実行すると、メタデータを元の状態にリセットできます。 
  
## <a name="configure-edit-metadata"></a>Edit Metadata (メタデータの編集) を構成する
  
1.  Azure Machine Learning で、[Edit Metadata (メタデータの編集)](./edit-metadata.md) を実験に追加して、更新するデータセットを接続します。 これは **[Manipulate]\(操作\)** カテゴリの **[Data Transformation]\(データ変換\)** の下にあります。
  
2.  **[Launch the column selector]\(列セレクターの起動\)** をクリックして、使用する列または列のセットを選択します。 名前またはインデックスで列を個別に選択することも、型で列のグループを選択することもできます。  
  
3.  選択した列に別のデータ型を割り当てる必要がある場合は、**[Data type]\(データ型\)** オプションを選択します。 特定の操作には、データ型の変更が必要な場合があります。たとえば、ソース データセットにテキストとして処理される数値がある場合、算術演算を使用する前にそれらを数値データ型に変更する必要があります。 

    + サポートされるデータ型は `String`、`Integer`、`Double`、`Boolean`、`DateTime` です。 

    + 複数の列が選択されている場合は、メタデータの変更を**すべて**の選択された列に適用する必要があります。 たとえば、2 - 3 の数値列を選択するとします。 1 つの操作でそれらをすべて文字列データ型に変更して、名前を変更することができます。 ただし、ある列を文字列データ型に変更し、別の列を float から integer に変更することはできません。
  
    + 新しいデータ型を指定しない場合は、列のメタデータは変更されません。 
    
    + [Edit Metadata (メタデータの編集)](./edit-metadata.md) の操作後、列の型と値が変更されます。 [Edit Metadata (メタデータの編集)](./edit-metadata.md) を使用して列のデータ型をリセットすることで、いつでも元のデータ型を復元できます。  

    > [!NOTE]
    > 任意の数値の型を **DateTime** 型に変更する場合は、**[DateTime Format]\(DateTime 形式\)** フィールドを空白のままにします。 現時点では、ターゲットのデータ形式を指定することはできません。  

      
4.  **[Categorical]\(カテゴリ\)** オプションを選択して、選択した列内の値をカテゴリとして扱うことを指定します。 

    たとえば、0、1、2 の数値を含む列があり、数値が実際には "Smoker (喫煙者)"、"Non-smoker (非喫煙者)"、"Unknown (不明)" を意味していることがわかっているとします。 この場合、列をカテゴリとしてフラグ設定することで、値が数値の計算に使われず、データのグループ化にのみ使用されることを保証できます。 
  
5.  Azure Machine Learning がモデル内のデータを使用する方法を変更する場合は、**[Fields]\(フィールド\)** オプションを使用します。

    + **機能**:フィーチャー列でのみ動作するモジュールで使用するため、このオプションを使用して列をフィーチャーとしてフラグ設定します。 既定では、すべての列が最初はフィーチャーとして扱われます。  
  
    + **ラベル**:ラベル (予測可能な属性、またはターゲット変数とも呼ばれます) をマーク付けするには、このオプションを使用します。 多くのモジュールでは、ラベル列が少なくとも 1 つ (および 1 つだけ) データセット内に存在している必要があります。 
    
        多くの場合、Azure Machine Learning では、クラス ラベルが含まれている列を推測できますが、このメタデータを設定することで、列が正しく識別されることを保証できます。 このオプションを設定してもデータの値は変更されず、一部の機械学習アルゴリズムがデータを処理する方法だけが変更されます。
  

  
    > [!TIP]
    >  これらのカテゴリに適合しないデータがありますか。  たとえば、データセットに変数として有用ではない一意の識別子などの値が含まれている場合があります。 ID をモデル内で使用すると問題が発生する場合があります。 
    >   
    >  さいわい、Azure Machine Learning の "内部" にはすべてのデータが保持されているため、データセットからこのような列を削除する必要はありません。 いくつか特殊な列のセットに対して操作を実行する必要がある場合には、[Select Columns in Dataset (データセット内の列の選択)](./select-columns-in-dataset.md) モジュールを使用して、他のすべての列を一時的に削除するだけです。 後で [Add Columns (列の追加)](./add-columns.md) モジュールを使用して、列をマージしてデータセットに戻すことができます。  
  
6. 前の選択を消去してメタデータを既定値に復元するには、次のオプションを使用します。  
  
    + **[Clear feature]\(フィーチャーのクリア\)**:フィーチャー フラグを削除するには、このオプションを使用します。  
  
         数値演算を実行するモジュールでは、すべての列が最初はフィーチャーとして扱われるため、数値列が変数として扱われることを防ぐためにこのオプションを使用する必要があります。
  
    + **[Clear label]\(ラベルのクリア\)**:指定した列から**ラベル** メタデータを削除するには、このオプションを使用します。  
  
    + **[Clear score]\(スコアのクリア\)**:指定した列から**スコア** メタデータを削除するには、このオプションを使用します。  
  
         現時点では、列をスコアとして明示的にマークする機能は、Azure Machine Learning では使用できません。 ただし、一部の操作の結果として、列が内部的にスコアとしてフラグ設定されます。 また、カスタム R モジュールでスコアの値が出力される場合があります。
  
  
7.  **[New column names]\(新しい列名\)** には、選択した列 (複数可) の新しい名前を入力します。  
  
    + 列名には、UTF-8 エンコードでサポートされている文字のみを使用できます。 空の文字列、null、またはスペースのみで構成されている名前は許可されていません。  
  
    + 複数の列の名前を変更するには、コンマ区切りリストとして列のインデックスの順序で名前を入力します。  
  
    + 選択したすべての列の名前を変更する必要があります。 列を省略したりスキップすることはできません。  
  
  
8.  実験を実行します。  

## <a name="next-steps"></a>次の手順

Azure Machine Learning service で[使用できる一連のモジュール](module-reference.md)を参照してください。 