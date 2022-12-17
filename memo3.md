# k3s
- Requirements
  - https://docs.k3s.io/installation/requirements#operating-systems
- Joining K3s agent node to K8s Master node #1284  
  - https://github.com/k3s-io/k3s/issues/1284
- windows support #114
  - https://github.com/k3s-io/k3s/issues/114
- v2.6.6-Matrix
  - https://www.suse.com/assets/Rancher-Manager-v266-Matrix.pdf
- Rancher Support Matrices
  - https://www.suse.com/suse-rancher/support-matrix/all-supported-versions/rancher-v2-7-0/
- Launching Kubernetes on Windows Clusters
  - https://docs.ranchermanager.rancher.io/pages-for-subheaders/use-windows-clusters
- Installation Requirements
  - https://docs.ranchermanager.rancher.io/v2.6/pages-for-subheaders/installation-requirements#operating-systems-and-container-runtime-requirements

## k3sとk8sの違い
- etcdをSQLite(デフォルト)に置き換え
- ローカルストレージプロバイダー、ロードバランサーサービスなど一部機能が組み込まれている
- 証明書配布などのプロセスが組み込まれている(コントロールプレーン相当を１つのバイナリに組み込むことで実現)
- 外部環境への依存が減っている(contanerd, flannel, coreDNSが組み込まれている)
- master - worker間の通信が異なる(k3sでは server - agent と表現)
  - agentは自動生成されたパスワードを/etc/rancher/node/passwordに保存している
  - serverへのagent登録時にパスワードを通知する
  - serverはsecretとしてagentのパスワードを保存して以降このパスワードを使用する
- ref:
  - https://docs.k3s.io/
  - https://docs.k3s.io/architecture

## k3s vs k8s spec
- k8s
  - cpu: 2以上
  - mem: 2GB以上
  - disk: 記載なし(１０GBでも動いた実績あり)
- k3s
  - cpu: 1以上
  - mem: 512MB(推奨は1GB以上, １０ノードまでなら2GB)
  - disk: サイズについては特になし。SSDを推奨。

## k3sのwindows対応について
- Reqiurement (https://docs.k3s.io/installation/requirements#operating-systems)
  > K3s is expected to work on most modern Linux systems.
- FAQ (https://docs.k3s.io/faq#does-k3s-support-windows)
  > Does K3s support Windows?
At this time K3s does not natively support Windows, however we are open to the idea in the future.
- サポートする予定はないとのこと。
  - [https://github.com/k3s-io/k3s/issues/114](https://github.com/k3s-io/k3s/issues/114#issuecomment-924212454)

## k3sとk8sの混在について
- 以下を見るとサポートしてない。今後も予定はないとのこと。やはり証明書周りは直さないといけないみたい。
  - https://github.com/k3s-io/k3s/issues/1284
  - https://github.com/k3s-io/k3s/issues/1110
  

