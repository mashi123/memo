# いろいろ
- CLI (Common Language Infrastructure)
  - .NET Frameworkの基幹を構成する実行コードや実行環境などについてマイクロソフトが策定した仕様
- CLR (Common Language Runtime)
  - CLIのマイクロソフトの実装
- C++/CLI
  - .NET Frameworkの共通言語基盤 (CLI) 上で実行するプログラムを作るためにC++を拡張したプログラミング言語
- IL (Common Intermediate Language) (CILと書かれていることもある)
  - CLIの仕様に従った中間言語
  - ネイティブコードではなく、実行時にILがJITによりネイティブコードにコンパイルされて実行される
  - 中間言語の段階ではOSやCPUに非依存
- MSIL
  - .NETのコードをコンパイルしたときに生成されるIL
- その他
  - C++/CLI, C#のコードをコンパイルすると、MSILの中間コードが生成される
  - 通常のCやC++は、ネイティブコードにコンパイルされる
  - このため、後述のツールによる扱われ方も形式により異なってくる
- PE (Portable Executable)
  - Microsoftの環境の実行ファイル形式
  - exe, dllなど
  - たぶんネイティブコード、ILいずれもPE形式で作成され、中身で区別されている。

# ツール
### dumpbin
- コマンドラインで操作するツール。Visual Studioに含まれる
- 逆アセンブル、依存関係、インポートしているモジュールの一覧出力等ができる
- IL形式のものは情報がほぼ出ない
- 使えそうなオプション
  - /DEPENDENTS
  - /DISASM[:{BYTES|NOBYTES|NOWIDE|WIDE}]
  - /EXPORTS
  - /IMPORTS[:ファイル名]
- ネイティブコードのモジュールは、これで参照しているDLLがわかると思われる
- /IMPORT オプションで使っている関数もわかるかも(全部でるかもしれないが)

(実行例)
```
>dumpbin.exe xxx.dll /dependents
Microsoft (R) COFF/PE Dumper Version 14.34.31933.0
Copyright (C) Microsoft Corporation.  All rights reserved.


Dump of file xxx1.dll

File Type: DLL

  Image has the following dependencies:

    VCRUNTIME140D.dll
    ucrtbased.dll
    KERNEL32.dll

  Summary

        1000 .00cfg
        1000 .data
        1000 .idata
                        (省略)
```                          

```
>dumpbin.exe xxx.dll /imports
Microsoft (R) COFF/PE Dumper Version 14.34.31933.0
Copyright (C) Microsoft Corporation.  All rights reserved.


Dump of file xxx.dll

File Type: DLL

  Section contains the following imports:

    VCRUNTIME140D.dll
             180020150 Import Address Table
             1800204B0 Import Name Table
                     0 time date stamp
                     0 Index of first forwarder reference

                          31 __vcrt_LoadLibraryExW
                          2F __vcrt_GetModuleHandleW
                          3C memcpy
                        (省略)
```                          

### dependencies
- コマンドライン/GUIあり。OSS(GitHubにある)
- dependency walkerの後継として作成され、exe,dllの依存関係を出力できる(依存のchainも)
- 関数は出力できない？
- 制限あり
  - Only direct, forwarded and delay load dependencies are supported. Dynamic loading via LoadLibrary are not supported

### ildasm
- https://learn.microsoft.com/ja-jp/dotnet/framework/tools/ildasm-exe-il-disassembler
- コマンドライン/GUIで操作するツール。Visual Studioに含まれる(`C:\Program Files (x86)\Microsoft SDKs\Windows`あたりにある)
- ILの逆アセンブラ
- 共通仕様はここにスペックがある：https://www.ecma-international.org/publications-and-standards/standards/ecma-335/
  - これを見ると「.module extern」を探せば参照している外部DLLがわかりそう。そこから使っている関数もわかるか。

(使用例)
```
>ildasm.exe  /html WindowsFormsApp1.exe  /all /output=ildasm-test.html
```

### ILSpy
- ildasm1とできることは似ているが、GUIで表示される情報は多そう
- ILの逆アセンブラ
- コマンドラインでは使えないっぽい

# コールグラフのツール
- Ndepend
  - 有償
- Understand
  - 有償
- doxygen
  - freeだが、他パッケージの依存は出なそう
- visualstudio
  - コードマップ：https://learn.microsoft.com/ja-jp/visualstudio/modeling/map-dependencies-across-your-solutions?view=vs-2022
  - 依存関係図：https://learn.microsoft.com/ja-jp/visualstudio/modeling/create-layer-diagrams-from-your-code?view=vs-2022
  - これらは、`Professional edition`でも読み取りはサポート
- ReSharper
- 参考
  - https://en.wikipedia.org/wiki/List_of_tools_for_static_code_analysis




