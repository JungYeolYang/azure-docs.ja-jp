---
title: Azure Red Hat OpenShift に関してよく寄せられる質問 | Microsoft Docs
description: Microsoft Azure Red Hat OpenShift に関してよく寄せられる質問への回答を示します
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: 881734caf855ccfc4f001693fe261b8448b49bc4
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2019
ms.locfileid: "65466198"
---
# <a name="azure-red-hat-openshift-faq"></a>Azure Red Hat OpenShift に関する FAQ

この記事では、Microsoft Azure Red Hat OpenShift に関してよく寄せられる質問 (FAQ) を説明します。

## <a name="which-azure-regions-are-supported"></a>どの Azure リージョンがサポートされていますか?

Azure Red Hat OpenShift がサポートされている世界中のリージョンの一覧については、[サポートされているリソース](supported-resources.md#azure-regions)に関するページを参照してください。

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>既存の仮想ネットワークにクラスターをデプロイできますか?

いいえ。 もっとも、ピアリングを使って Azure Red Hat OpenShift クラスターを既存の VNET に接続することはできます。 詳細については、[クラスターの仮想ネットワークを既存の仮想ネットワークに接続する方法](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network)に関するセクションを参照してください。

## <a name="what-cluster-operations-are-available"></a>どのクラスター操作を使用できますか?

コンピューティング ノード数の増大または減少のみを行えます。 作成後は、`Microsoft.ContainerService/openShiftManagedClusters` リソースに対するその他の変更は許可されません。 コンピューティング ノードの最大数は 20 に制限されています。

## <a name="what-virtual-machine-sizes-can-i-use"></a>どの仮想マシンのサイズを使用できますか?

Azure Red Hat OpenShift クラスターで使用できる仮想マシンのサイズの一覧については、[Azure Red Hat OpenShift 仮想マシンのサイズ](supported-resources.md#virtual-machine-sizes)に関するページを参照してください。

## <a name="is-data-on-my-cluster-encrypted"></a>クラスター上のデータは暗号化されていますか?

既定では、保存時の暗号化があります。 Azure Storage プラットフォームは、永続化する前に自動的にデータを暗号化し、取得前にデータの暗号化を解除します。 詳細については、[保存データの Azure Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) に関するページを参照してください。

## <a name="can-i-use-prometheusgrafana-to-monitor-containers-and-manage-capacity"></a>Prometheus/Grafana を使用して、コンテナーを監視して容量を管理できますか?

いいえ、現時点ではできません。

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>Jenkins などのツールを使用するできるように、Docker レジストリを外部で使用できますか?

Docker レジストリは `https://docker-registry.apps.<clustername>.<region>.azmosa.io/` から利用できますが、強力なストレージ耐久性保証は得られません。 [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) を使用することもできます。

## <a name="is-cross-namespace-networking-supported"></a>名前空間間のネットワークはサポートされていますか?

顧客と個々 のプロジェクト管理者は、`NetworkPolicy` オブジェクトを使用して、プロジェクトごとに名前空間間のネットワークをカスタマイズできます (拒否を含む)。

## <a name="can-an-admin-manage-users-and-quotas"></a>管理者はユーザーおよびクォータを管理できますか?

はい。 Azure Red Hat OpenShift 管理者は、ユーザーおよびクォータを管理するほか、ユーザーが作成したすべてのプロジェクトにアクセスできます。

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>クラスターを特定の Azure AD ユーザーに制限できますか?

はい。 Azure AD アプリケーションを構成することで、クラスターにサインインできる Azure AD ユーザーを制限できます。 詳細については、「[方法: ご利用のアプリを特定のユーザー セットに制限する](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users)」を参照してください。

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>クラスターは複数の Azure リージョンにわたってコンピューティング ノードを持つことができますか?

いいえ。 Azure Red Hat OpenShift クラスターのすべてのノードは、同じ Azure リージョンに基づく必要があります。

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>マスターおよびインフラストラクチャ ノードは、Azure Kubernetes Service (AKS) の場合と同様に無視されますか?

いいえ。 クラスター マスターを含めすべてのリソースは、顧客サブスクリプションで実行します。 これらの種類のリソースは、読み取り専用のリソース グループに置かれます。
