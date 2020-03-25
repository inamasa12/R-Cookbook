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


### R Tips  
`cat(a, b, c)`: 複数のスカラー、ベクトルを一列に表示  
`ls()`: 変数のリストを表示  
`str(a)`: オブジェクトの構造を表示  
`mode(a)`: データ型を表示  
`map_dbl(a, f)`: データフレームaの各列に関数fを適用  
