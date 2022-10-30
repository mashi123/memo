## コンテナのネットワークインターフェースの変え方（Ubuntu）
ubuntuコンテナで動作確認。

### ベースコンテナ起動
```
docker run -it --name base ubuntu /bin/bash
```

### 必要なパッケージインストール
```
apt update
apt install vim
apt install netplan.io
apt install systemd udev init
```

(参考)
- https://genchan.net/it/virtualization/docker/13093/

### コンテナ再作成
コンテナからコンテナイメージを作成。
```
docker commit base test:v1
```

特権付きでコンテナを起動しコンテナに接続。
```
docker run --privileged -itd --name  test test:v1 /sbin/init
docker exec -it test /bin/bash
```

## netplanの設定ファイル作成
まず以下コマンドで変更するNICのMACアドレスを確認。
```
ip a
```

以下内容で50-cloud-init.yamlを作成（たぶんファイル名は何でもいい）。  
set-nameに変更後の名前を入れる。
```bash
cat /etc/netplan/50-cloud-init.yaml .
network:
    version: 2
    ethernets:
        eth0:
            dhcp4: true
            match:
                macaddress: 02:42:ac:11:00:02
            set-name: XXX
```

設定を適用する。以下は例。
```
# netplan try
Do you want to keep these settings?


Press ENTER before the timeout to accept the new configuration


Changes will revert in 117 seconds
Configuration accepted.
root@001b19ddaa76:/# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
3: sit0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/sit 0.0.0.0 brd 0.0.0.0
12: XXX@if13: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::42:acff:fe11:2/64 scope link
       valid_lft forever preferred_lft forever
```

