---
title: Azure Container Instances のコンテナーの手動での停止または開始
description: Azure Container Instances のコンテナー グループを手動で停止または開始する方法について説明します。
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/15/2019
ms.author: danlep
ms.openlocfilehash: 8e62d106a42dfbec897e5e14cf68fd3d7fd823c4
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65070810"
---
# <a name="manually-stop-or-start-containers-in-azure-container-instances"></a>Azure Container Instances のコンテナーの手動での停止または開始

コンテナー グループの[再起動ポリシー](container-instances-restart-policy.md)設定により、コンテナー インスタンスの既定の開始方法または停止方法が決まります。 コンテナー グループを手動で停止または開始することにより、この既定の設定をオーバーライドできます。

## <a name="stop"></a>Stop

たとえば、[az container stop][az-container-stop] コマンドまたは Azure portal を使用して、実行中のコンテナー グループを手動で停止します。 特定のコンテナー ワークロードでは、一定期間の経過後に実行時間の長いコンテナー グループを停止してコストを節約できます。 

*停止状態になったコンテナー グループは終了し、グループ内のすべてのコンテナーをリサイクルします。コンテナーの状態は保持されません。*

コンテナーがリサイクルされると、[リソース](container-instances-container-groups.md#resource-allocation)の割り当てが解除され、コンテナー グループの課金が停止します。

コンテナー グループがすでに終了している (成功または失敗のいずれかの状態にある) 場合、停止アクションでは何も起こりません。 たとえば、一度だけ実行されるコンテナー タスクを含むコンテナー グループが正常に実行され、成功状態で終了するとします。 この状態のグループを停止しようとしても、状態は変わりません。 

## <a name="start"></a>start

コンテナー グループが独自に終了したか、手動でグループを停止させたためにコンテナー グループが停止しているときに、コンテナーを開始できます。 たとえば、 [az container start][az-container-start] コマンドまたは Azure portal を使用して、グループ内のコンテナーを手動で開始します。 いずれかのコンテナーのコンテナー イメージが更新されると、新しいイメージがプルされます。 

コンテナー グループを開始すると、同じコンテナー構成で新しいデプロイが開始されます。 このアクションにより、期待どおりに動作する既知のコンテナー グループ構成を簡単に再利用できます。 同じワークロードを実行するために新しいコンテナー グループを作成する必要はありません。

このアクションにより、コンテナー グループ内のすべてのコンテナーが開始されます。 グループ内の特定のコンテナーを開始することはできません。

コンテナー グループを手動で起動または再起動した後、コンテナー グループは、構成された再起動ポリシーに従って実行します。
  
## <a name="restart"></a>Restart

コンテナー グループの実行中に、[az container restart][az-container-restart] コマンドなどを使用してコンテナー グループを再起動できます。 このアクションでは、コンテナー グループ内のすべてのコンテナーが起動します。 いずれかのコンテナーのコンテナー イメージが更新されると、新しいイメージがプルされます。 

コンテナー グループの再起動は、デプロイの問題のトラブルシューティングを行うときに役立ちます。 たとえば、一時的なリソース制限により、コンテナーが正しく実行できない場合、グループの再起動で問題が解決することがあります。

このアクションにより、コンテナー グループ内のすべてのコンテナーが再起動されます。 グループ内の特定のコンテナーを再起動することはできません。

コンテナー グループを手動で再起動した後、コンテナー グループは、構成された再起動ポリシーに従って実行されます。

## <a name="next-steps"></a>次の手順

詳細については、Azure Container Instances の[再起動ポリシー設定](container-instances-restart-policy.md)に関する記事を参照してください。

既存の構成を使用したコンテナー グループの停止および開始に加えて、実行中のコンテナー グループの[設定を更新](container-instances-update.md)できます。

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-restart]: /cli/azure/container?view=azure-cli-latest#az-container-restart
[az-container-start]: /cli/azure/container?view=azure-cli-latest#az-container-start
[az-container-stop]: /cli/azure/container?view=azure-cli-latest#az-container-stop
