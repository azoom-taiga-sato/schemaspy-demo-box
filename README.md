# Schemaspy使用方法

## 事前セッティング(インストール/ ダウンロード)

* (Java 8のインストール)[https://www.java.com/ja/download/]

* (Graphvizのインストール)[https://www.graphviz.org/download/]
```bash
brew install graphviz 
```

* (JDBCドライバのダウンロード)[https://www.mysql.com/products/connector/]
    * `JDBC Driver for MySQL (Connector/J)`>`Platform Independent`を選択
    * ダウンロードしたjarファイルを`mysql-connector-j-9.0.0`を対象ディレクトリに格納

* (Schemaspy Jarfileのダウンロード)[https://github.com/schemaspy/schemaspy/releases]
    * 最新versionのjarファイルを対象ディレクトリに格納

## 使用方法
### A: コマンドにconfig情報直接入力=>outputファイル作成 (◯)
```bash
java -jar ./schemaspy-6.2.4.jar \
    -t mysql \
    -dp ./mysql-connector-j-9.0.0.jar \
    -db parking_app \
    -host localhost \
    -port 3306 \
    -s parking_app \
    -u root \
    -o ./output \
    -hq /opt/homebrew/bin/dot \
    -imageformat svg \
    -imagefontadjust true
```
* Graphviz rendererについて以下のようなエラーが多く発生するが、無視して良さそう。((根拠 from Github issues)[https://github.com/schemaspy/schemaspy/issues/833])
```bash
.ERROR - dot ~~ Warning: cell size too small for content
```

### B: Configファイルを作成 => コマンドでconfigファイルを参照=>outputファイル作成(×: エラー解決できず、index.htmlが生成されない)
* `schemaspy.properties`で設定情報を記載=>`java -jar ./schemaspy-6.2.4.jar \ -configFile schemaspy.properties -vizjs`を実行
    ```bash
    //schemaspy.properties
    schemaspy.t=mysql
    schemaspy.dp=./mysql-connector-j-9.0.0.jar
    schemaspy.db=parking_app
    schemaspy.host=localhost
    schemaspy.port=3306
    schemaspy.s=parking_app
    schemaspy.u=root
    # schemaspy.p=
    schemaspy.o=./output
    schemaspy.hq=/opt/homebrew/bin/dot
    schemaspy.imageformat=svg
    schemaspy.imagefontadjust=true
    ```

* 以下のようなエラーが発生する。Javaランタイムが認識するクラスファイルバージョンが古いのが原因。Versionを11に変えれば解消するようだが、まだ解決できていない。
```bash
Caused by: java.lang.UnsupportedClassVersionError: org/openjdk/nashorn/api/scripting/NashornScriptEngineFactory has been compiled by a more recent version of the Java Runtime (class file version 55.0), this version of the Java Runtime only recognizes class file versions up to 52.0
```

### Dockerで実施(未着手)


## 特徴

### 🙆
* 各テーブル、及びそのER図をサイトとして一覧できる。
* Anomaliesで不整合なテーブルを検出できる。
* Orphan Tablesで他テーブルとのRelationがないものを検出できる。
* Searchでテーブルレベル、テーブル内のカラムレベルの検索が可能。

### 🙅
* 各テーブル・カラムに対するコメントの記載や、テーブルをカテゴライズなどのカスタマイズがしにくい？
* index.htmlを参照するためには、github pagesなどを使用する必要がある。ただし、github pagesはpublicでないとサイトを公開できない。Private公開&内部でのみサイトを公開するためには、Github Enterpriseを使用する必要がある？


## 参考文献`
* [公式ドキュメント](https://schemaspy.org/)
* [Github](https://github.com/schemaspy)
* [SchemaSpyを使ってER図を自動で作る](https://zenn.dev/politive/articles/3efb08d5cafc63#%E6%A6%82%E8%A6%81)
* [Specifying the renderer from schemaspy's issue](https://github.com/schemaspy/schemaspy/issues/33)
* (MacでスタイリッシュなER図を作成する(Oracle))[https://qiita.com/uhooi/items/83e70e32d5bd7f12af8a#jdbc%E3%83%89%E3%83%A9%E3%82%A4%E3%83%90%E3%81%AE%E3%83%80%E3%82%A6%E3%83%B3%E3%83%AD%E3%83%BC%E3%83%89]
*[Create MS SQL Database documentations using SchemaSpy](https://www.youtube.com/watch?v=Cev4jEvppsk)
* (SchemaSpyでデータベースのドキュメントを生成してみた)[https://dev.classmethod.jp/articles/schemaspy-doc/]
* (docker compose 上で SchemaSpy が動かないのを直す (MySQL 8, Apple M1 Mac))[https://qiita.com/mikankari/items/785cb00d8b7b1f563f13]
* (SchemaSpyを使ってER図を自動で作る)[https://zenn.dev/politive/articles/3efb08d5cafc63#%E6%A6%82%E8%A6%81]
* (SchemaSpyを使ってデータベースの中身をまとめたかった)[https://qiita.com/kazuyaseo/items/4dc76d4d2847f009c3bb]