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
`install.packages("foreign_0.8-74.tar.gz", repos = NULL, type = "source")`:  
古いバージョンのパッケージをインストールする場合は、作業フォルダに解凍前のファイルを保存し、インストールする  


## Chapter4 入出力  
readrパッケージがお勧め（固定幅、タブ・スペース区切り、カンマ区切り等に対応する関数がそれぞれある）  
Uinstall.packages("foreign_0.8-74.tar.gz", repos =  
NULL, type = "source"  RLを指定して、HTTPサーバーやFTPサーバーから直接データを取得することも可能  
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
ggsaveで保存  

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
  theme(legend.position="none") +
  ggsave("filename.jpg")
~~~

* Q-Qプロット  
想定する分布とサンプルの分布の累積密度のペアをプロットしたもの  
サンプルの分布が想定する分布と同じであれば、プロットは直線になる  
stat_qqがプロット、stat_qq_lineが理論線を描画する  
~~~
ggplot(df, aes(sample=x)) +
  stat_qq(想定する分布とパラメータ、デフォルトは正規分布) +
  stat_qq_line(想定する分布とパラメータ、デフォルトは正規分布)
~~~

* 特定の関数のグラフ化  
stat_functionを用いる  
自作の関数を描画することも可能  
~~~
f <- function(x) exp(-abs(x))*sin(2*pi*x)
ggplot(data.frame(x=c(-3, 3))) +
  aes(x) +
  stat_function(fun=f)
~~~

* patchworkライブラリ  
グラフをまとめることができる  
~~~
g1 + g2 + g3 + g3 + plot_layout(ncol=2)
~~~

* それ以外  
二変数の関係をプロットするには、GGally::ggpairs()、plot()が便利  

### R Tips  
`gather(df, 分類名, 値名, -対象外の列)`: データフレームをリスト型の列に変換する  
`colors()`: 使用可能な色のリストを出力  
`tidyverse::if_else(condition, A, B)`: if演算  
`ggsave("g1.jpg", plot=g1, units="in", width=5, height=4)`: ggsaveは独立した関数としても使用できる  


## Chapter11 線形回帰とANOVA  

R2は予測値の分散を実測値の分散で割ったもの  
F値は予測値の分散を予測誤差の分散で割ったもの、これが有意に大きい時に「全ての回帰係数がゼロ」という帰無仮説は棄却される（これはANOVAでも用いられる） 
予測値の分散は予測誤差の分散の変化と同値であり、F値は、モデルとナイーブ予測、またはモデル間での残差の変化を検定する値となる  
⇒ 両者の比較において、残差が有意に低下するかどうかを検定  

回帰はlm関数を使用する  
交互作用項は他の変数によって、ある変数の説明力が影響を受けることを考慮する  
てこ比（leverage）はハット行列の対角成分であり、ある被説明変数の観測値がその予測値に与える影響を表す  
⇒ 大きいほど予測値に与える影響が大きい  
クックの距離はある観測値の有無が全ての予測値に与える影響を正規化した数値で、てこ比と標準化残差で表される  
⇒ 両者が大きいほど値が大きくなり、0.5を超えると影響が大きい、1.0を超えると影響が非常に大きいとされる  
信頼区間は母平均の区間推定値、予測区間は予測値の区間推定値  

~~~
lm(y ~ x1 + x2, data=df, subset=(col1=="A"))lm(y ~ u + I(u^2), data=df)
~~~
ステップワイズ回帰において、変数を減らすのが後退法、増やすのが前進法  
~~~
step(full.model, direction="backward")
step(min.model, direction="forward", scope=(~x1 + x2 + x3 + x4))
~~~
回帰式の中で演算子を普通に使いたい場合はI()で囲む  
~~~
lm(y ~ u + I(u^2), data=df)
~~~
特に多項式回帰を行う場合はpoly関数を用いる  
~~~
lm(y ~ poly(x, 3, raw=TRUE))
~~~
関数変換はそのまま使用できる  
~~~
lm(log(y) ~ x)
~~~

ANOVAは正規分布を仮定している ⇒ そうでない場合はクラスカル＝ウォリス検定を使用する  
但し、クラスカル＝ウォリス検定も各分布が同じ形状であることを前提としている  

ANOVAを実行し、各カテゴリカル変数による違いを視覚化する（Jhon TukeyのHSD法）  
~~~
m <- aov(y~x, subset=1:2500, data=df)
TukeyHSD(m)
plot(TukeyHSD(m))
~~~

### R Tips  
`broom::augment(lm_summary)`: 変数、予測値、残差等をまとめたデータフレームを作成  
`MASS::boxcox(lm_summary)`: 被説明変数の対数変換を前提に、ベキ乗値、回帰係数を最尤法で推定する（Box-Cox法）  
`confint(lm_summary, level=0.99)`: 回帰係数の信頼区間  
`plot(lm_summary)`: 回帰残差のプロット等の回帰診断  
`car::outlierTest(lm_summary)`: 異常値の検定  
`lmtest::dwtest(lm_summary)`: 残差の時系列相関の検定（帰無仮説は「自己相関がない」）  
`predict(lm_summary, newdata=x_df)`: 推定モデルを使用した予測、interval="prediction"で予測区間も併せて算出  
`oneway.test(var~factor, data, subset=index)`: 一元配置分散分析（帰無仮説は「全ての平均値は同じ」）、デフォルトは「グループの分散は異なる」  
`fareway::interaction.plot(df$cat1, df$cat2, df$y)`: 二次元の効果をプロット  
`kruskal.test(var~factor, data=df)`: クラスカル＝ウォリス検定  


## Chapter12 便利なテクニック  

時間の計測  
~~~
library(tictoc)
tic('proc time')
process
res <- toc()
~~~

### R Tips  
`(x <- 1 / pi)`: 括弧で囲むと値が出力されるため、デバッグに使用できる  
`rowSums(df)`: 行ごとに列を合計  
`colSums(df)`: 列ごとに行を合計
`cut(x, breaks, label)`: 連続変数xの各点を、breaksに従って分類し、labelの名称を与える ⇒ ビニング  
`match(a, x)`: aにマッチするxの要素の位置を返す  
`which.min(x)`: 最小値の位置を返す  
`seq_along(x)`: 1:length(x)  
`pmin(x, y), pmax(x, y)`: ペアワイズの最小値、最大値  
`tidyverse::arrange(df, col1, -col2)`: データフレームを指定の列でソート  
`attributes(x), attr(x, "name")`: オブジェクトが持つ属性  
`class(x)`: オブジェクトクラス  
`mode(x)`: ネイティブのデータ構造  
`str(x)`: 内部構造と内容  
`# Section 1 ----`: セクション区切り  


## Chapter13 高度な数値計算と統計  

* クラスタリング  
~~~
d <- dist(x)              観測値間の距離を計算
hc <- hclust(d)           クラスタの計算
clust <- cutree(hc, k=n)  nクラスに分類
~~~

* ロジスティック回帰  
予測として確率値を返したい時は、typeに"response"を選択する  
~~~
m <- glm(y ~ factor, family=binomial)
predict(m, type="response", newdata=new)
~~~

### R Tips  
`optimize(f, lower=-20, upper=20)`: 単変量関数fの最小値と、最小値となる変数の値の組み合わせを返す  
`optim(ini_vec, f)`: 初期値から始めて、多変量関数の最小値を求める  
`eigen(matrix)`: 固有値、固有ベクトルの算出  
`prcomp(~ x + y)`: 主成分分析  
`tapply(x, factor(y), mean)`: ファクター別に関数を適用  
`factanal(matrix, factors=3)`: 因子分析  


## Chapter14 時系列分析  

zooパッケージ、xtsパッケージが有用  
xtsはzooの全ての処理を実行できる  
標準のtsパッケージの関数を使う際には、as.ts(x)関数でtsクラスに変換して適用する  
tidyverse系のtsibbleはパネル分析時に便利  
計量ファイナンスにはtimeSeriesも有用とのこと  
財務データ用のプロット関数を用意したquantmodパッケージがある  
xts['2010/2012']という書式で2010年から2012年までのデータ取得が可能（xtsはデータ抽出の自由度が高い）  
時系列オブジェクト同士の計算では、日付は自動的に揃えてくれる  
自己相関がxある場合は、MA(x)が検討される  
偏自己相関がxある場合は、AR(x)が検討される  


* 時系列データの作成  
~~~
x <- c(3, 4, 1, 4, 8)
dt <- seq(as.Date("2018-1-1"), as.Date("2018-1-5"), by="days")
ts <- zoo(x, dt)
~~~

* 時系列グラフの作成  
~~~
main <- "IBM: Historical vs. Inflation-Adjusted"
lty <- c("dotted", "solid")
plot(ibm.infl,
     lty = lty, main = main,
     legend.loc="left")
~~~

* ARIMA  
~~~
auto.arima(ts, seasonal=T)
m <- arima(ts, order=c(3, 2, 2))
m <- arima(ts, order=c(3, 2, 2), fixed=c(0, NA, NA, 0, NA)): fixedでは除きたい変数にゼロを指定
confint(m): 信頼区間
checkresiduals(m): 残差の正規性診断  
fc_m <- forecast(m): 将来10期の予測
autoplot(fc_m): 予測を含めたグラフ化
~~~

### R Tips  
`index(zoo)`: 時系列インデックスの取得  
`coredata(zoo)`: データ部分の取得  
`window(ibm, start=as.Date("2010-1-5"), end=as.Date("2010-1-7"))`: 指定期間の抽出  
`merge(xts1, xts2, all=TRUE)`: 異種の時系列データ結合、allはTRUEのとき全外部結合、FALSEのとき内部結合  
`na.locf(xts)`: NULLを直前の値で埋める  
`lag(xts, k=-1, na.pad=T)`: 1日前のデータが当日のデータになる（ラグ1）  
`diff(xts, lag=12)`: 12日前との差  
`rollmean(xts, 7, align="right")`: 移動平均値の計算（rightは過去データのみを使用する）  
`apply.daily[weekly, monthly, quarterly, yearly](xts, func)`: 期間毎に関数を適用  
`na.omit(xts)`: データがNAの点を除く  
`rollapply(xts, width=30, by=30, FUN=sd)`: widthの期間のデータを用いてbyの間隔毎にFUNを適用  
`acf(xts)`: 自己相関を計算  
`Box.test(xts)`: 自己相関の検定（ボックス＝ピアース検定）、帰無仮説は「自己相関がない」、サンプル数が少ない場合はtype="Ljung-Box"を指定する（リュング＝ボックス検定）  
`pacf(xts)`: 偏自己相関を計算  
`forecast::Ccf(v1, v2)`: 各ラグの相関を計算  
`ggplot2::autoplot(xts)`: 簡易に時系列をプロット  
`tseries::adf.test(vex)`: 定常性の検定（トレンド除去あり）、帰無仮説は「定常ではない」  
`locpoly(x=x, y=y, bandwidth=bw, gridsize=gridsize)`: 多項式によるスムージング、バンド幅bwとデータ点の数gridsizeを指定  
`dpill(x, y, gridsize=gridsize)`: 多項式スムージングにおける適切なバンド幅を推定  


## Chapter15 シンプルなプログラミング  

独自の関数をスクリプトファイルにまとめて、`source(**.R)`することで使用できる  
スクリプトを選択し、ctrl+Iで書式を整える  


* 条件分岐  
~~~
if (x>=0) {
  print(sqrt(x))
} else {
  print("negative")
}
~~~
* ループ  
~~~
for (x in 1:5){
  print(x)
}
~~~
* switch文  
~~~
num <- switch{s,
  one = 1,
  two = 2,
  NA
}
~~~
* 関数  
~~~
cv <- function(x=1){
  sd(x) / mean(x)
}
~~~

### R Tips  
`ifelse(vec >= 100, "big", "small")`: ベクトルにif関数を適用  
`stop("message")`: エラーメッセージを出して関数を終了する  
`warning('message')`: ウォーニングを出す  
`func2 = possibly(func1, otherwise=NULL)`: エラー時の返り値を設定した新しい関数を返す  
`function(x) print(x)`: 匿名関数、一時的な関数定義  


## Chapter16 R Markdownと公開  

マークダウンで書いておけば、色々なフォーマットに出力できる（ニッティング）
後に修正が容易なのはHTML  
Knitで目的のフォーマットで出力  
プレゼン形式（横長）で出力することも可能  
パラメータも設定できる  

