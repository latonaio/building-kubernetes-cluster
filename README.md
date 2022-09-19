# building-kubernetes-cluster-by-kubespray
building-kubernetes-cluster-by-kubespray は Kubespray[https://kubespray.io/#/] を用いて Kubernetes Cluster を構築するためのマイクロサービスです。


# 対象環境
本マイクロサービスの対象環境は、AWS、GCE、Azure、OpenStack、vSphere、ベアメタル、などの環境です。  

## 構築手順
### Kuberspray を clone
Kuberspray を git clone してください。
```
make clone-kubespray
```

### env の設定
env.json5.sample から env.json5 を複製して用意してください。

- user
    - ansible をあてるための ssh username
- executionPath
    - kubespray の git clone した場所

### inventory の設定
#### 作成
inventory の情報ファイルを生成してください。
```
make copy-inventory
```

#### ip address のセットアップ
inventory に 対象の ip address を追加してください。
```
declare -a IPS=(192.168.100.101 192.168.100.102 192.168.100.103)
CONFIG_FILE=kubespray/inventory/mycluster/hosts.yml python3 kubespray/contrib/inventory_builder/inventory.py ${IPS[@]}
```

#### hosts を設定
「inventory/mycluster/hosts.yml」の設定ファイルの control plane と worker の設定を行ってください。

### cri-dockerd 対応
#### group_vars
kuberspray 公式の「docs/docker.md」のドキュメントに沿って cri-dockerd に対応させるようにします。
生成された「inventory/mycluster/group_vars/all/docker.yml」配下の対象設定項目を差し替えます。

#### container_manager
各ファイルで設定している container_manager の設定項目を docker に差し替えます。
- inventory/mycluster/group_vars/all/etcd.yml
- inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml

### ansible-playbook の実行
ansible-playbook を実行します。
```
make execute
```
