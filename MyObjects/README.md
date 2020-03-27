# R-Cookbook Tips

## Chapter1 ヘルプ  
### Rに関するドキュメント  
1. 付属ドキュメント  
`help.start()`  
`help.seqrch("pattern")`: パッケージを含めたローカルのドキュメント全般を検索  
1. パッケージドキュメント  
`help(functionname), help(package="packagename")`: 関数自体のドキュメント  
`args(functionname)`: 関数の変数のドキュメント  
`example(functionname)`: 関数の使用例  
`vignette()`: ビニエット（付属ドキュメント）の一覧  
1. CRAN（The Comprehensive R Archive Network）  
Task View、Mailing List  
1. Q&Aサイト  
Stack Overflow（コード）、Cross Validate（統計）、RStudio Community  
1. WEBサイト  
`RSiteSearch("pattern")`: R Projectの検索エンジンを起動  
RSeek: R専用のWEBサイトのみを検索するGoogle検索エンジン  
crantastic: Rパッケージ検索サイト  
1. メーリングリスト  
Nabble: Rメーリングリストの一覧  
### R Tips  
`q()`: RStudioの終了  
`sessionInfo()`: 実行環境の表示  
*.RData*に作業ディレクトリの内容が保存されている  

## Chapter2 基礎  
* ベクトル  
ベクトルの要素を除外するには負のインデックスを使う  
異なるデータ型を混ぜることはできない  
names属性で各要素に名前を付けることができる  
* 注意事項  
\は使用できない  
通常の演算で使用する論理式には&と|を使い、条件文で使用する論理式には&&と||を使う  

### R Tips  
`cat(o1, o2, o3)`: 複数のスカラー、ベクトルを一列に表示  
`ls()`: 変数のリストを表示  
`str(o)`: オブジェクトの構造を表示  
`mode(o)`: データ型を表示  
`map_dbl(df, f)`: データフレームaの各列に関数fを適用  
`quantile(v, 0.05)`: パーセンタイル値  
`filter(df, col > 20)`: データフレームの行を抽出  
`%>%`: パイプ演算子、出力を次に来る関数の第一変数として渡す

## Chapter3 実行環境  
作業ディレクトリ  
プロジェクト  
作業スペース  

### R Tips  
`save.image()`: 作業スペースの保存  
`search()`: ロード済みのパッケージ  
`library()`: インストール済みのパッケージ  
