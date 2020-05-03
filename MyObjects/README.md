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
分布関数について、rは乱数の生成、dは確率密度、pは累積密度、qはパーセンタイル値（任意の累積確率になる値）  
ベルヌーイ試行は、一定確率で成功（失敗）を繰り返すプロセス  
n回ベルヌーイ試行を行った時の成功回数の確率分布が二項分布（パラメータは試行回数と成功確率）  
n=1の二項分布はベルヌーイ分布と同じ  

### R Tips  
`choose(n, k)`: 組合せの数  
`combn(vec, k)`: 全ての組合せを行列で出力  
`set.seed(n)`: 乱数シードの固定  
`sample(vec, n, replace=TRUE, prob=c(0.8, 0.2)`:  
vecからn個の無作為抽出、replaceがTRUEなら復元抽出、FALSE（デフォルト）なら非復元抽出   

## Chapter9 一般統計学  
* 一変量のt検定（平均値の検定）    
平均値の水準を検定、帰無仮説は「平均はmである」  
サンプル数が多い場合（30以上が目安）は対象の変量の分布は問わないが、少ない場合は正規分布である必要がある  

* 二変量のt検定（平均値の差の検定）    
母集団の平均値の差異を検定、帰無仮説は「平均値は同じである」  
サンプル数が多い場合（20以上が目安）は対象の変量の分布は問わないが、少ない場合は正規分布である必要がある  
二変量の分散が同じか、二変量のサンプルが同一個体であるか等で計算が異なる  

* 一変量のウィルコクソン符号付順位検定（中央値の検定）  
中央値の水準を検定、帰無仮説は「中央値はmuである」  
ノンパラメトリック  

* 二変量のウィルコクソン＝マン・ホイットニー検定（分布のずれの検定）  
分布の中央位置にずれがあるかどうかを検定  
分布の形状が同じである必要がある  
ノンパラメトリックのため、変量に対する制約がない  

* 標本比率の検定  
標本比率の水準を検定  
仮定する比率からの乖離をカイ二乗検定する  

* 相関の検定  
正規母集団の変数についてはピアソン、非正規母集団についてはスピアマンを用いるべき  
corのデフォルトはピアソン  

### R Tips  
`summary(vec)`: 統計量（四分位値、最大値、最小値、平均値）  
`mean(conditions)`: 割合の計算、パーセンタイル値の算出  
`table(vec1, vec2)`: 集計表を作成  
`summary(cross table)`: 独立性のカイ二乗検定  
`scale(vec)`: 正規化（zスコア）  
`t.test(vec, m=100, conf.level=0.99)`: 一変量のt検定  
`t.test(vec1, vec2, paired=TRUE)`: 二変量のt検定、paierdはサンプルの個体が同一の場合  
`pairwise.t.test(values, factors)`: 多変量のt検定  
`wilcox.text(vec, mu=100, conf.int=TRUE)`: 一変量のウィルコクソン符号付順位検定（ノンパラメトリック）  
`wilcox.text(vec1, vec2, paired=TRUE)`: 二変量のウィルコクソン＝マン・ホイットニー検定（ノンパラメトリック）  
`prop.test(出現回数, サンプル数, 想定確率)`: 標本比率の検定、信頼区間、一変量の場合  
`prop.test(c(出現回数), c(サンプル数))`: 標本比率の検定、複数変量の場合  
`shaprio.test(vec)`: 正規性の検定  
`runs.test(factor)`: 連検定、帰無仮説は「ランダムな配列である」  
`load(data.rdata)`: rdataファイルの読み込み  
`cor.test(vec1, vec2, method="spearman")`: 相関の検定  
`ks.test(vec1, vec2)`: コルモゴロフ＝スミルノフ検定、分布の形状の差異をノンパラメトリック検定  


## Chapter10 グラフィックス  
* ggplot2  
ggplotが基本のプロット用関数  
リスト型（縦長）に形成したデータフレームがデータの基本形  
aes関数（エステティック属性）でデータ項目を対応するグラフ項目に紐付ける ⇒ aes(x軸に割り当てる項目, y軸に割り当てる項目)  
　ファクタに応じてプロットの形や色を変える場合は、shape、colorにファクタを指定する  
　order（軸に割り当てる項目、ソートする項目）で並びを変えることが可能   
グラフの形状を決める幾何オブジェクト関数(geom)  
　散布図: geom_point()、折れ線グラフ: geom_line()、箱ひげ図: geom_boxplot()、棒グラフ: geom_bar()  
ラベル関係はlabsで指定  
　title、x、yでそれぞれの名前を指定  
背景はthemeで指定、目盛りpenel.grid.major、補助目盛りpanel.grid.minor、背景penel.backgroundがある  
　element_line(rect)について、colorやlinetype、fillなどを指定  
　既成の背景、theme_bw()、theme_minimal()等がある  
凡例が不要な場合はtheme(legend.position = "none")で指定する（left、right、bottom、topや座表軸で位置指定が可能）  
ファクタ別にサブプロットを作成する場合はfacet_wrap(~fact)を使う
geom_smooth、geom_abline、geom_hline、geom_vlineで補助線を追加できる  
棒グラフはgeom_bar、集計の有無は引数statで指定する  
折れ線グラフはgeom_line、linetype、size、colで線の形状を指定する  
箱ひげ図はgeom_boxplot、coord_flip()で軸を回転させることができる  
ヒストグラムはgeom_histogram、binsで区分の数を設定、geom_density(aes(y=..density..))で密度分布を追加  

例）  
~~~
ggplot(mtcars, aes(hp, mpg, color=type, shape=type)) +
  geom_point() +
  labs(title="The Title",
       x="X-axis Label",
       y="Y-axis Label") +
  theme(panel.background=element_rect(fill="white", color="grey")) +
  theme(panel.grid.major=element_line(color="red", linetype=3)) +
  theme(panel.grid.minor=element_line(color="blue", linetype=4)) +
  facet_wrap(~type) +
  theme(legend.position="none")
~~~

* Q-Qプロット  
stat_qqがプロット、stat_qq_lineが理論線を描画する  
~~~
ggplot(df, aes(sample=x)) +
  stat_qq(想定する分布とパラメータ、デフォルトは正規分布) +
  stat_qq_line(想定する分布とパラメータ、デフォルトは正規分布)
~~~

* それ以外  
二変数の関係をプロットするには、GGally::ggpairs()、plot()が便利  



### R Tips  
gather(df, 分類名, 値名, -対象外の列): データフレームをリスト型の列に変換する  


