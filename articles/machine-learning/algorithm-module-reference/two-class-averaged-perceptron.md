---
title: デシジョン フォレスト回帰:モジュール リファレンス
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning service の 2 クラス平均化パーセプトロン モジュールを使用し、平均化パーセプトロン アルゴリズムに基づいて機械学習モデルを作成する方法について説明します。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f0fec525ed87f91cf102053383b2934aac4b71c0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027771"
---
# <a name="two-class-averaged-perceptron-module"></a>2 クラス平均化パーセプトロン モジュール

この記事では、Azure Machine Learning service のビジュアル インターフェイス (プレビュー) のモジュールについて説明します。

平均化パーセプトロン アルゴリズムに基づいて機械学習モデルを作成するには、このモジュールを使用します。  
  
この分類アルゴリズムは、教師あり学習手法であるため、ラベル列を含む "*タグ付けされたデータセット*" が必要となります。 モデルのトレーニングは、対象のモデルとタグ付けされたデータセットを "[モデルのトレーニング](./train-model.md)" に入力として渡すことにより行うことができます。 その後は、トレーニング済みのモデルを使用して新しい入力例の値を予測することができます。  

### <a name="about-averaged-perceptron-models"></a>平均化パーセプトロン モデルについて

*平均化パーセプトロンの手法*は、ニューラル ネットワークの初期の単純なバージョンです。 この手法では、線形関数に基づいて入力がいくつかの可能な出力に分類され、特徴ベクターから得られる重みのセットと結合されます。このため、"パーセプトロン" と呼ばれます。

単純なパーセプトロン モデルは線形分離可能なパターンを学習するのに適しています。一方、ニューラル ネットワーク (特にディープ ニューラル ネットワーク) は、より複雑なクラス境界をモデル化できます。 ただし、パーセプトロンはより高速であり、ケースを順次処理するため、継続的なトレーニングで使用できます。

## <a name="how-to-configure-two-class-averaged-perceptron"></a>2 クラス平均化パーセプトロンの構成方法

1.  **2 クラス平均化パーセプトロン** モジュールを実験に追加します。  

2.  **[Create trainer mode]\(トレーナー モードの作成\)** オプションを設定して、モデルのトレーニング方法を指定します。  
  
    -   **[Single Parameter]\(単一パラメーター\)**:モデルの構成方法を決めている場合は、特定の値のセットを引数として渡します。
  
3.  **[Learning rate]\(学習速度\)** に*学習速度*の値を指定します。 学習速度の値は、モデルがテストされて修正される度に確率的勾配降下法で使用されるステップのサイズを制御します。
  
     速度の値を小さくすると、モデルのテストが頻繁に実行されますが、ローカルで停滞する可能性があります。 ステップのサイズを大きくすることで収束速度は速くなりますが、真の極小値から離れていってしまうおそれがあります。
  
4.  **[Maximum number of iterations]\(イテレーションの最大数\)** には、アルゴリズムによってトレーニング データを検証する回数を入力します。  
  
     早く停止することで、より優れた一般化がもたらされる場合が多いです。 イテレーションの回数を増やすと適合が向上する反面、過剰適合のおそれがあります。
  
5.  **[Random number seed]\(乱数シード\)** には、必要に応じて、シードとして使用する整数値を入力します。 繰り返し実行したときの実験の再現性を確保したい場合は、シードを使用することをお勧めします。  
  
1.  トレーニング データセットと、次のいずれかのトレーニング モジュールを接続します。
  
    -   **[Create trainer mode]\(トレーナー モードの作成\)** を **[Single Parameter]\(単一パラメーター\)** に設定した場合は、[モデルのトレーニング](train-model.md) モジュールを使用します。

## <a name="results"></a>結果

トレーニングの完了後、次の作業を行います。

+ モデルのパラメーターとトレーニングから学習された特徴の重みを確認するために、[[Train Model]\(モデルのトレーニング\)](./train-model.md) の出力を右クリックします。


## <a name="next-steps"></a>次の手順

Azure Machine Learning service で[使用できる一連のモジュール](module-reference.md)を参照してください。 