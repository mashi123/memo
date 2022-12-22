
# Advanced Cluster Management
## サポート環境
https://access.redhat.com/articles/6968787
ベア環境もVMwareもサポート

## Pod
インフラノードで動かせる

## ネットワーク要件
inbound/outboundの要件あり

## サイジング
https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.6/html/install/installing#product-environment
- 「1.2.4.1. Product environment」 と 「1.2.4.1.1. OpenShift Container Platform on additional services」 があり、合計になるのか後者だけになるのか読み取れない。
- 1.2.4.1.1. を見ると、以下になる。

- OpenShift Container Platform(VMware)
    - 3ノード　4CPU 16GiB 120GiB
- OpenShift Container Platform(ベアメタル)
    - 3ノード　4CPU 16GiB 120GiB　※コントロールプレーンだけでworkerなしで動ける 

- ここの情報を見ると、かなりスペックが必要と書いてある。
    - https://rheb.hatenablog.com/entry/OpenShift_ACM
    - Master：8CPU 24GB Memory x 3台
    - Worker：8CPU 24GB Memory x 3台
    - 4CPU / 16GB ではPodが起動できず構築が完了しなかったとのこと(OCP4.5)。


# quay
https://access.redhat.com/documentation/en-us/red_hat_quay/3.8/html/deploy_red_hat_quay_on_openshift_with_the_quay_operator/operator-concepts#operator-prereq

- RHEL8, OpenShiftどちらでも使える
- OpenshiftではOperatorHubからいんすとーるできる

## OCPバージョン
OpenShift 4.5以上

## リソース/podあたり
- 8Gi of memory
- 2000 millicores of CPU.

## ストレージ
- インストール前にオブジェクトストレージを設定する必要がある
- https://access.redhat.com/documentation/ja-jp/red_hat_quay/3.8/html/deploy_red_hat_quay_on_openshift_with_the_quay_operator/operator-storage-preconfig

- マネージドストレージとしてODFを使える。
- Ceph を使用する ODFの場合は別途サブスクリプションが必要
- アンマネージドのものはS3などの選択肢もあり
> Red Hat OpenShift Data Foundations (ODF) Operator
- 50 GiB がデフォルトで割り当てられる

## その他
- 特にベアなどの制限の記載なし


# Advanced Cluster Security
## サポート環境
https://access.redhat.com/ja/node/6983614
-　Red Hat OpenShift Container Platform (OCP) 4.x	

## インストール方法
- Operator (recommended)
- Helm
https://access.redhat.com/documentation/ja-jp/red_hat_advanced_cluster_security_for_kubernetes/3.73/html-single/installing/index#install-rhacs-ocp

## スペック
- 2CPU ＋ 3GiB ?
    - 実際にはコンポーネントごとに必要なスペックがある
https://access.redhat.com/documentation/ja-jp/red_hat_advanced_cluster_security_for_kubernetes/3.73/html-single/installing/index#acs-general-requirements_prerequisites-ocp

### Central
- Central	CPU	メモリー	ストレージ
    - Request　1.5 コア　4 GiB　100 GiB
    - Limit　4 コア　8 GiB　100 GiB　※500ノード
    - ※100ノードの場合、2 コア　4 GiB　100 GiB

- PVC (Ceph FS ストレージは使えないらしい？)
- Scanner
    - limit ５コア　8GB
- Sensor
    - Limit 2 コア　4 GiB
- Admission コントローラー
    - Limit 0.5 500MiB
- Collector
    - 0.75コア　1GiB

## その他
ベア環境での制限など記載なし
