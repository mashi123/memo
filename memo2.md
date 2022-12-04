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
  - C++/CLI, C#のコードを困憊すると、MSILの中間コードが生成される
  - 通常のCやC++は、ネイティブコードにコンパイルされる
  - このため、後述のツールによる扱われ方も形式により異なってくる
- PE (Portable Executable)
  - Microsoftの環境の実行ファイル形式
  - exe, dllなど
  - たぶんネイティブコード、ILいずれもPE形式で作成され、中身で区別されている。

# ツール

