---
title: Image Analysis の認知検索スキル - Azure Search
description: Azure Search エンリッチメント パイプラインの ImageAnalysis 認知スキルを使用した画像解析を通じてセマンティック テキストを抽出します｡
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: bb18c858a17e290a8ce2cc88dc1e7d88d21afe0f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/02/2019
ms.locfileid: "65021907"
---
#   <a name="image-analysis-cognitive-skill"></a>Image Analysis の認知スキル

**Image Analysis** スキルは､イメージの内容に基づいて豊富な一群のビジュアル フィーチャーを抽出します｡ たとえば､イメージからキャプションを生成したり､タグを生成したり､セレブリティやランドマークを特定したりできます｡ このスキルでは、[Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) Cognitive Services によって提供される機械学習モデルが使用されます。 

> [!NOTE]
> 処理の頻度を増やす、ドキュメントを追加する、または AI アルゴリズムを追加することによってスコープを拡大する場合は、[課金対象の Cognitive Services リソースをアタッチする](cognitive-search-attach-cognitive-services.md)必要があります。 Cognitive Services の API を呼び出すとき、および Azure Search のドキュメントクラッキング段階の一部として画像抽出するときに、料金が発生します。 ドキュメントからのテキストの抽出には、料金はかかりません。
>
> 組み込みスキルの実行は、既存の [Cognitive Services の従量課金制の価格](https://azure.microsoft.com/pricing/details/cognitive-services/)で課金されます。 画像抽出の価格は、[Azure Search の価格のページ](https://go.microsoft.com/fwlink/?linkid=2042400)で説明されています。


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>スキルのパラメーター

パラメーターの大文字と小文字は区別されます。

| パラメーター名     | 説明 |
|--------------------|-------------|
| defaultLanguageCode   |  結果を返す言語を示す文字列｡ サービスは､指定された言語で認識結果を返します｡ このプロパティが指定されていない場合の既定値は "en" です｡ <br/><br/>サポートされている言語は以下の通りです｡ <br/>*en* - 英語 (既定) <br/> *zh* - 簡体字中国語|
|visualFeatures |   結果として返すビジュアル フィーチャー型を示す文字列の並び｡ 有効なビジュアル フィーチャー型には以下があります｡  <ul><li> *categories* - 認知サービス関係の[ドキュメント](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy) で定義されている分類学に従ったイメージのコンテンツの分類です</li><li> *tags* - イメージのコンテンツに関係する単語の詳細な一覧のタグです｡</li><li>*description* - 英文でのイメージのコンテンツの説明です｡</li><li>*faces* - 顔の有無を検出します｡ 存在する場合は､座標と性別､年齢を生成します｡</li><li> *imageType* - 画像がクリップ アートか線画かを検出します｡</li><li>  *color* - アクセント カラー、主要カラー､白黒画像か否かを判定します｡</li><li>*adult* - イメージが事実上のポルノ (裸または性行為を表す) かどうかを検出します｡ セックスを連想されるコンテンツも検出されます｡</li></ul> ビジュアル フィーチャー名は大文字と小文字が区別されます｡|
| 詳細   | 結果として返すドメイン固有の詳細を示す文字列の並び. 有効なビジュアル フィーチャー型には以下があります｡ <ul><li>*celebrities* - イメージ内でセレブリティが検出された場合に、そのセレブリティを特定します｡</li><li>*landmarks* - イメージ内でランドマークが検出された場合に、そのランドマークを特定します｡</li></ul>
 |

## <a name="skill-inputs"></a>スキルの入力

| 入力名      | 説明                                          |
|---------------|------------------------------------------------------|
| image         | 複合型｡ 現在は "/document/normalized_images" フィールドでのみ機能し､ ```imageAction``` が ```none``` 以外の値に設定されている場合に､Azure BLOB インデクサーによって生成されます。 詳しくは､[サンプル](#sample-output) をご覧ください｡|



##  <a name="sample-definition"></a>定義例
```json
{
    "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
    "context": "/document/normalized_images/*",
    "visualFeatures": [
        "Tags",
        "Faces",
        "Categories",
        "Adult",
        "Description",
        "ImageType",
        "Color"
    ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "image",
            "source": "/document/normalized_images/*"
        }
    ],
    "outputs": [
        {
            "name": "categories",
            "targetName": "myCategories"
        },
        {
            "name": "tags",
            "targetName": "myTags"
        },
        {
            "name": "description",
            "targetName": "myDescription"
        },
        {
            "name": "faces",
            "targetName": "myFaces"
        },
        {
            "name": "imageType",
            "targetName": "myImageType"
        },
        {
            "name": "color",
            "targetName": "myColor"
        },
        {
            "name": "adult",
            "targetName": "myAdultCategory"
        }
    ]
}
```

##  <a name="sample-input"></a>サンプル入力

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "image": {
                    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
                    "width": 500,
                    "height": 300,
                    "originalWidth": 5000,
                    "originalHeight": 3000,
                    "rotationFromOriginal": 90,
                    "contentOffset": 500
                }
            }
        }
    ]
}
```


##  <a name="sample-output"></a>サンプル出力

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "categories": [
                    {
                        "name": "abstract_",
                        "score": 0.00390625
                    },
                    {
                        "name": "people_",
                        "score": 0.83984375,
                        "detail": {
                            "celebrities": [
                                {
                                    "name": "Satya Nadella",
                                    "faceBoundingBox": {
                                        "left": 597,
                                        "top": 162,
                                        "width": 248,
                                        "height": 248
                                    },
                                    "confidence": 0.999028444
                                }
                            ],
                            "landmarks": [
                                {
                                    "name": "Forbidden City",
                                    "confidence": 0.9978346
                                }
                            ]
                        }
                    }
                ],
                "adult": {
                    "isAdultContent": false,
                    "isRacyContent": false,
                    "adultScore": 0.0934349000453949,
                    "racyScore": 0.068613491952419281
                },
                "tags": [
                    {
                        "name": "person",
                        "confidence": 0.98979085683822632
                    },
                    {
                        "name": "man",
                        "confidence": 0.94493889808654785
                    },
                    {
                        "name": "outdoor",
                        "confidence": 0.938492476940155
                    },
                    {
                        "name": "window",
                        "confidence": 0.89513939619064331
                    }
                ],
                "description": {
                    "tags": [
                        "person",
                        "man",
                        "outdoor",
                        "window",
                        "glasses"
                    ],
                    "captions": [
                        {
                            "text": "Satya Nadella sitting on a bench",
                            "confidence": 0.48293603002174407
                        }
                    ]
                },
                "requestId": "0dbec5ad-a3d3-4f7e-96b4-dfd57efe967d",
                "metadata": {
                    "width": 1500,
                    "height": 1000,
                    "format": "Jpeg"
                },
                "faces": [
                    {
                        "age": 44,
                        "gender": "Male",
                        "faceBoundingBox": {
                            "left": 593,
                            "top": 160,
                            "width": 250,
                            "height": 250
                        }
                    }
                ],
                "color": {
                    "dominantColorForeground": "Brown",
                    "dominantColorBackground": "Brown",
                    "dominantColors": [
                        "Brown",
                        "Black"
                    ],
                    "accentColor": "873B59",
                    "isBwImg": false
                    },
                "imageType": {
                    "clipArtType": 0,
                    "lineDrawingType": 0
                }
            }
        }
    ]
}
```


## <a name="error-cases"></a>エラーになる場合
次のエラーが発生した場合､要素は抽出されません｡

| エラー コード | 説明 |
|------------|-------------|
| NotSupportedLanguage | 指定された言語はサポートされていません｡ |
| InvalidImageUrl | イメージの URL の形式が不適切か､アクセスできません｡|
| InvalidImageFormat | 入力データが有効なイメージではありません｡ |
| InvalidImageSize | 入力イメージが大きすぎます。 |
| NotSupportedVisualFeature  | 指定されたフィーチャーの型が有効ではありません｡ |
| NotSupportedImage | サポートされていないイメージ､たとえば､子供のポルノ イメージです｡ |
| InvalidDetails | サポートされていないドメイン固有のモデルです｡ |

## <a name="see-also"></a>関連項目

+ [定義済みのスキル](cognitive-search-predefined-skills.md)
+ [スキルセットの定義方法](cognitive-search-defining-skillset.md)
+ [インデクサーの作成 (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
