# R-Cookbook Tips

## Chapter1 ヘルプ  
### Rに関するドキュメント  
1. 付属ドキュメント  
`help.start()`  
`help.search("pattern")`: パッケージを含めたローカルのドキュメント全般を検索  
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
作業スペース: メインメモリ、使用中のデータや変数、関数が置かれる  
作業ディレクトリ: 入出力ファイルが置かれるデフォルト、作業スペースを記録した.RDataが置かれる  
プロジェクト: 作業スペースと作業ディレクトリのセット、.Rprojに設定が保存される  
ホームディレクトリ: R本体がインストールされているフォルダ、`Sys.getenv("R_HOME")`で表示    

* 環境設定  
環境変数設定を記載した.Profileファイルを作成し、ホームディレクトリに保存しておけば、R起動時に実行される  
`Sys.setenv(DB_USERID = "my_id")`: 環境変数の設定  
`Sys.getenv()`: 環境変数一覧  

* データ  
datasets内のデータはロード済みのため、すぐに使える  
`data()`: datasetsパッケージのデータ一覧  
`data(package="pkgname")`: 特定のパッケージのデータ一覧  
`data(dname, package="pkgname")`: 特定のパッケージのデータをロード  
`help(dname)`: データの内容説明  

* コマンドプロンプトからの実行  
`R CMD BATCH scriptfile outputfile`: スクリプトファイルからの実行  
`Rscript scriptfile arg1 arg2 ...`: 引数が必要な場合  

* オプション設定  
`options(prompt="R> ")`: 設定
`options("prompt")`: 値の取得  
`options()`: 一覧  
`help(options)`: 説明  

### R Tips  
`save.image()`: 作業スペースの保存  
`search()`: ロード済みのパッケージ  
`library()`: インストール済みのパッケージ  
`installed.packages()`: インストール済みのパッケージ詳細  
`detach(package:ggplot2)`: パッケージをメモリから削除  
`remove.packages("ggplot2")`: パッケージを完全に削除  
`source("myScript.R")`: スクリプトの実行  
`install_github("thomasp85/tidygraph")`: githubからのパッケージインストール  

## Chapter4 入出力  
readrパッケージがお勧め（固定幅、タブ・スペース区切り、カンマ区切り等に対応する関数がそれぞれある）  
URLを指定して、HTTPサーバーやFTPサーバーから直接データを取得することも可能  
Excelとのやりとりにはopenxlsxを用いる  
HTMLの読み込みとデータテーブルの抽出にはrvestが便利  
各種RDBとの連携には、ベースとなるDBIと各種バックエンドパッケージが必要になる  
tidyverseパッケージのdbplyrで各種RDBのデータをtydyverseライクに操作できる（対応したSQLを使い分ける必要がない）  
|パッケージ|RDB|
|:--:|:--:|
|odbc|Microsoft SQL Server|
|RPostgreSQL|Postgres, Redshift|
|RMySQL|MySQL, MariaDB|
|RSQLite|SQLite|
|bigquery|Google BigQuery|

* リダイレクト  
処理結果がfilenameに出力される  
~~~
sink("filename")
# 処理
sink()
~~~
* コネクション  
filenameへの追加型の書き込み
~~~
con <- file("filename", "w")
cat(data1, file=con)
cat(data2, file=con)
close(con)
~~~

### R Tips  
`list.files(path='data/')`: 作業ディレクトリのファイル一覧  
`readLines('filename')`: 各行を文字列として取得  
`scan("filename" or "URL", what=numeric(0))`: 各行のトークンを左から右にスキャン  
`order(vector)`: 昇順ランクのリストを返す  
`save(object1, object2, file="myData.RData")`: オブジェクトの保存  
`load('myData.RData')`: オブジェクトの再現  


## Chapter5 データ構造  
### 分類
* オブジェクト  
スカラー、文字列、ベクトル、ファクタ、リスト、データフレーム、関数等  

* モード  
メモリへの格納方法による分類    
数値(numeric)、文字(character)、リスト（オブジェクトへのポインタ）(List)  

* 代表的なクラス
スカラー、ベクトル: 同じクラスのオブジェクトの集合、スカラーは要素が一つのベクトル  
マトリックス: 次元を持ったベクトル  
ファクタ: カテゴリカル変数の集合  
リスト: オブジェクトの集合、最も柔軟性が高い  
データフレーム: 特殊な形式のリスト  
tibble: tidyverseで使われる定義が厳格なデータフレーム  

### データ整形テクニック  
* カテゴリ別データの作成  
~~~
freshman <- c(1, 2, 1, 1, 5)
sophomores <- c(3, 2, 3, 3, 5)
juniors <- c(5, 3, 4, 3, 3)
comb <- stack(list(fresh = freshman, soph = sophomores, jrs = juniors))
~~~
* 名前付きリストの作成  
~~~
values <- 1:3
names <- letters[1:3]
lst <- list()
lst[names] <- values
~~~

### R Tips  
`class(object)`: オブジェクトのクラス  
`typeof(object)`: 要素のデータ型  
`dim(vector)=c(2, 3)`: ベクトルをマトリックスに変換  
`list$name <- NULL`: NULLを割り当てることで要素を削除する  
`unlist(list)`: リストをベクトルに変換  
`l(s)apply(object, func)`: リストの各要素や表の列要素に対して関数を適用する、lはリストでsはマトリックスで結果を返す  
`do.call(func, list)`: リストの各要素を変数として関数に代入する  
`Map(func, list)`: リストの各要素に対して関数を適用する（lapplyと機能的には同じ）  
`na.omit(object)`: NAがある行を削除  
`inner(full, left, right)_join(df1, df2, by=key)`: データフレームの結合処理  

## Chapter6 データ変換  
極力ループ処理を避け、ベクトル計算を行う  
データフレームの処理にtidyverseは便利だが、大規模な行列演算ではapplyのスピードが速い  
* グループ別の集計  
~~~
df %>% 
  group_by(col1, col2) %>%
  summarize(
    result_var = fun(target_var)
    )
~~~

### R Tips  
`map(list, func) or lapply(list, func)`:  
リストの各要素（データフレームの場合は列が相当する）に関数を適用し、結果をリストで返す（ベクトルで返すのがsapply）  
`rowwise()`: データフレームの行毎に各要素を抽出  
`mutate(df, col=func(col_old))`: データフレームの列を処理（追加、上書き）  
`apply(matrix or df, 1 or 2, func)`: 行(1)もしくは列(2)に関数を適用  
`map2(vec1, vec2, func) or pmap(list of vecs, func)`: スカラー対応の関数に変数をベクトルで与え、結果をベクトルもしくはリストで返す  
`case_when(conditions)`: tidyverseにおけるcase処理  

## Chapter7 文字列と日付  
日付クラスは、Date、chron、POSIXの順で選択すると良い  
Date: 日付汎用クラスだが、時刻を扱えない  
chron: 時刻対応しているが、機能が少ない  
POSIX: 時刻対応しているが、やや複雑  
その他、tidyverseの日付時刻操作パッケージとして、lubridateがある  
正規表現を使用する場合はregexp  
seqはDateクラスに対応している  

### R Tips  
`nchar(char)`: 文字列の長さをカウントする  
`paste(c('a', 'b'), 'c', sep=', ')`: 異なるベクトルの要素同士の連結、要素数が少ない方のベクトルはリサイクルされる  
`paste(c('a', 'b'), collapse=', ')`: 同じベクトルの要素を連結  
`substr(char, 2, 5)`: 文字列から指定の範囲を抽出  
`strsplit(char, '/')`: 指定の文字（正規表現も可能）で部分文字列のベクトルに分割  
`(g)sub('a', 'b', char)`: 文字列の'a'を'b'で置き換え（gを付けないと最初の一致部分だけを置き換え）  
`outer(vec1, vec2, func)`: 全ての要素の組み合わせについて関数を適用する（結果は行列で返す）  
`expand.grid(vec1, vec2)`: 全ての組み合わせをデータフレームにして返す  
`lower.tri(matrix)`: 下三角にTRUE  
`Sys.Date()`: 現在の日付  
`as.Date('12-31-2020', format='%m-%d-%Y')`: 文字列の日付を日付クラスに変換（デフォルトはYYYY/MM/DD） 
`format(Sys.Date(), format='%m-%d-%Y')`: 日付クラスを文字列に変換（書式はstrftimeのヘルプを参照）  
`ISOdate(year, month, date)`: 年月日からPOSIXctの日付を生成  

## Chapter8 確率  


