## コンテナのネットワークインターフェースの変え方（Ubuntu）
ubuntuコンテナで動作確認。  
参考URL: https://askubuntu.com/questions/1317036/how-to-rename-a-network-interface-in-20-04

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
systemdを動かすために以下を参考にした。
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

## DLLのwrap
- wrap_dll実行

```
wrap_dll-master>python wrap_dll.py --hook hook_project1.h D:\visualstudio\dll\Project1.dll --dumpbin=D:\visualstudio\2022\Community\VC\Tools\MSVC\14.34.31933\bin\Hostx64\x86\dumpbin.exe --undname=D:\visualstudio\2022\Community\VC\Tools\MSVC\14.34.31933\bin\Hostx64\x86\undname.exe
D:\visualstudio\dll\Project1.dll is a x64 DLL
```
- 実行が成功すると、元のDLLは`real_xxx.dll`にリネームされる。
- フォルダ移動＆生成物確認

```
wrap_dll-master>cd Project1

wrap_dll-master\Project1>dir
 ドライブ C のボリューム ラベルがありません。
 ボリューム シリアル番号は A442-747B です

 wrap_dll-master\Project1 のディレクトリ

2022/11/21  23:05    <DIR>          .
2022/11/21  23:05    <DIR>          ..
2022/11/21  23:05               211 CMakeLists.txt
2022/11/21  23:05               356 hook_macro.h
2022/11/21  23:05               781 hook_project1.h
2022/11/21  23:05             1,367 Project1.cpp
2022/11/21  23:05                89 Project1.def
2022/11/21  23:05               171 Project1_asm.asm
2022/11/21  23:05            59,392 real_Project1.dll
               7 個のファイル              62,367 バイト
               2 個のディレクトリ  17,322,323,968 バイトの空き領域
```

- cmake実行

```
wrap_dll-master\Project1>D:\visualstudio\2022\Community\Common7\IDE\CommonExtensions\Microsoft\CMake\CMake\bin\cmake.exe CMakeLists.txt
-- Building for: Visual Studio 17 2022
-- Selecting Windows SDK version 10.0.22000.0 to target Windows 10.0.19044.
-- The C compiler identification is MSVC 19.34.31933.0
-- The CXX compiler identification is MSVC 19.34.31933.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: D:/visualstudio/2022/Community/VC/Tools/MSVC/14.34.31933/bin/Hostx64/x64/cl.exe - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: D:/visualstudio/2022/Community/VC/Tools/MSVC/14.34.31933/bin/Hostx64/x64/cl.exe - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- The ASM_MASM compiler identification is MSVC
-- Found assembler: D:/visualstudio/2022/Community/VC/Tools/MSVC/14.34.31933/bin/Hostx64/x64/ml64.exe
CMake Warning (dev) in CMakeLists.txt:
  No cmake_minimum_required command is present.  A line of code such as

    cmake_minimum_required(VERSION 3.24)

  should be added at the top of the file.  The version specified may be lower
  if you wish to support older CMake versions for this project.  For more
  information run "cmake --help-policy CMP0000".
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring done
-- Generating done
-- Build files have been written to: wrap_dll-master/Project1
```

- 生成物確認

```
wrap_dll-master\Project1>dir
 ドライブ C のボリューム ラベルがありません。
 ボリューム シリアル番号は A442-747B です

 wrap_dll-master\Project1 のディレクトリ

2022/11/21  23:05    <DIR>          .
2022/11/21  23:05    <DIR>          ..
2022/11/21  23:05            71,411 ALL_BUILD.vcxproj
2022/11/21  23:05               315 ALL_BUILD.vcxproj.filters
2022/11/21  23:05            16,105 CMakeCache.txt
2022/11/21  23:05    <DIR>          CMakeFiles
2022/11/21  23:05               211 CMakeLists.txt
2022/11/21  23:05             1,493 cmake_install.cmake
2022/11/21  23:05               356 hook_macro.h
2022/11/21  23:05               781 hook_project1.h
2022/11/21  23:05             1,367 Project1.cpp
2022/11/21  23:05                89 Project1.def
2022/11/21  23:05             3,116 Project1.sln
2022/11/21  23:05            82,789 Project1.vcxproj
2022/11/21  23:05             1,428 Project1.vcxproj.filters
2022/11/21  23:05               171 Project1_asm.asm
2022/11/21  23:05            59,392 real_Project1.dll
2022/11/21  23:05            71,023 ZERO_CHECK.vcxproj
2022/11/21  23:05               552 ZERO_CHECK.vcxproj.filters
              16 個のファイル             310,599 バイト
               3 個のディレクトリ  17,324,503,040 バイトの空き領域

wrap_dll-master\Project1>
```
    
- この語、Project1.slnを開いてビルドするとProject1.dllができた。
