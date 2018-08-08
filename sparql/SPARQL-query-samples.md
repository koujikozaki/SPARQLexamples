
# 必要なリンク集
## SPARQL 1.1 Query Language (W3C Recommendation)
https://www.w3.org/TR/sparql11-query/   
→日本語訳　http://www.asahi-net.or.jp/~ax2s-kmtn/internet/rdf/REC-sparql11-query-20130321.html

## WikidataのSPARQLエンドポイント（検索用API）
https://query.wikidata.org/      
※プログラムからアクセスする場合は，こちら https://query.wikidata.org/sparql  　

## PREFIXの検索サービス
https://prefix.cc/

## Wikidataのプロパティ一覧（抜粋）
https://www.wikidata.org/wiki/Wikidata:List_of_properties

## Wikidataのクラス一覧
※独自にWikidataを解析して抽出したものであるため，実際のデータと一致しな部分が含まれる可能性があります．
https://docs.google.com/spreadsheets/d/1e2r9w-PnAroO4MTAMJGfw-bZV7r68Xul-UtK6QbrK_w/edit#gid=0

---

# 関心のあるリソースのIRIを探す

例）大阪大学のWikidata上でのIRI　　

[http://www.wikidata.org/entity/Q651233](http://www.wikidata.org/entity/Q651233)

---

# 検索例１：主語のみ指定
「大阪大学」を主語（Subject）に含むトリプルの述語（?p）と目的語（?o）を取得する　

```
select ?p ?o
where {
   <http://www.wikidata.org/entity/Q651233> ?p ?o .
}
LIMIT 100
```
クエリを試す　http://tinyurl.com/ya9krs2v

---------------

## 検索例1-2：主語のみ指定．その主語が持つプロパティの一覧を取得．

「大阪大学」を主語（Subject）とするトリプルの述語（?p）を取得する（重複除く）

```
select distinct ?p
where {
   <http://www.wikidata.org/entity/Q651233> ?p ?o .

}
LIMIT 100
```
クエリを試す　http://tinyurl.com/y9ekx5fw

---------------
## 検索例1-3：PREFIXを用いた省略表現

```
PREFIX wd: <http://www.wikidata.org/entity/>

select distinct ?p
where {
   wd:Q651233 ?p ?o .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/ycqcfxux

---------------
# 検索例２：主語と述語を指定

## 例2-1）「大阪大学」の「本部所在地」となる目的語（?o）を取得

```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

select distinct ?o
where {
   wd:Q651233 wdt:P159 ?o .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/ydxxo72h

---------------
## 例2-2） 「大阪大学」のラベルとなる目的語（?o）を取得

```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select distinct ?o
where {
   wd:Q651233 rdfs:label ?o .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/y9fr7pgs

---------------
## 例2-3)「大阪大学」のラベルとなる目的語（?o）と，その言語種別を取得

```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select distinct ?o (lang(?o) AS ?ln)
where {
   wd:Q651233 rdfs:label ?o .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/yckbo7mv

---------------

# 検索例3：FILTERによる絞り込み

## 例3-1)「大阪大学」のラベルとなる目的語（?o）から，“日本語のラベルのみ”を取得

```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select distinct ?o
where {
   wd:Q651233 rdfs:label ?o .
   FILTER (lang(?o) = "ja") .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/ycpcvo2l

---------------

## 例3-2)「大阪大学」のラベルとなる目的語（?o）のうち，“Osaka”という文字列を含むもの取得

```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select distinct ?o
where {
   wd:Q651233 rdfs:label ?o .
   FILTER (regex(?o,"Osaka")) .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/yb3pwk7t

---------------

# 検索例4：述語と目的語を指定

## 例）4-1：「国が“日本”と一致する」トリプルの主語（?s）を取得する
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

select ?s
where {
 ?s wdt:P17 wd:Q17 .
}
LIMIT 100
```
クエリを試す　http://tinyurl.com/yd9jk3s3

---------------

## 例）4-2： 「ラベルが“大阪大学”と一致する」トリプルの主語（?s）を取得する
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select ?s
where {
 ?s rdfs:label "大阪大学"@ja .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/y967mwjx

---------------

## 例）4-3)「大阪大学」のクラス（何のインスタンスか？）を取得する
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>

select ?o
where {
 wd:Q651233  wdt:P31 ?o.
}
```
クエリを試す　http://tinyurl.com/y8fehpd3

---------------

## 例4-4)「大学」のインスタンスの一覧を取得する
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
select ?s
where {
  ?s wdt:P31 wd:Q3918.
}LIMIT 100
```
クエリを試す　http://tinyurl.com/ydh426yk

---------------

# 検索例5：複数パターンの組み合わせ

## 例5-1)複数の述語を指定して，目的語を取得する

「大阪大学」の“本部所在地”と“創立日”取得する

```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>

select ?o1 ?o2 where {
  wd:Q651233 wdt:P159 ?o1.
  wd:Q651233 wdt:P571 ?o2.
}
```
クエリを試す　http://tinyurl.com/ycgfhd4z

---------------

## 例5-2：述語と目的語を指定し，主語の一覧を取得（ラベルを併記）

「大学」のインスタンスの一覧と，その“日本語ラベル付き”を取得する
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select ?s ?o where {
  ?s wdt:P31 wd:Q3918.
  ?s rdfs:label ?o .
  FILTER (lang(?o) = "ja") .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/y9xmm3fo

---------------
## 例5-3：述語と目的語を指定し，主語の一覧を取得（あれば，ラベルを併記）

「大学」のインスタンスの一覧を取得し，その“日本語ラベル付き”があれば，取得する
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select ?s ?o where {
  ?s wdt:P31 wd:Q3918.
  OPTIONAL{
    ?s rdfs:label ?o .
    FILTER (lang(?o) = "ja") .
  }
}LIMIT 100
```
クエリを試す　http://tinyurl.com/y9mb3297

---------------
## 例5-4：主語の述語と，その目的語の述語を指定

「大阪大学」の“本部所在地”と，その“日本語ラベル”取得する
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>

select ?o1 ?o2 where {
  wd:Q651233 wdt:P159 ?o1 .
  ?o1 rdfs:label ?o2 .
  FILTER (lang(?o2) = "ja") .
}
```
クエリを試す　http://tinyurl.com/ydfps923

---------------
## 例5-5：主語の述語と，その目的語の述語を指定

「大阪大学」の“クラス”と，「大阪大学」 の“卒業生”＝「大阪大学を“educated-at”の目的語とする主語（?s）」を取得得する
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>

select ?o1 ?o2 where {
  wd:Q651233 wdt:P31 ?o1 .
  ?o2 wdt:P69 wd:Q651233 .
}
```
クエリを試す　http://tinyurl.com/y9acojun

---------------
## 例5-6：主語と目的語の組み合わせ

「大学」と「卒業生」の組み合わせを取得する
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select ?s ?univ where {
 ?univ wdt:P31  wd:Q3918.
 ?s       wdt:P69  ?univ .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/ydc84ov3

---------------
## 例5-7：主語と目的語の組み合わせ（日本語ラベル併記）

「大学」と「卒業生」の組み合わせを取得する．日本語のラベルを併記
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select ?univ ?univl ?s ?l where {
  ?univ wdt:P31 wd:Q3918.
  ?s wdt:P69 ?univ.
  OPTIONAL{
    ?univ rdfs:label ?univl .
    FILTER (lang(?univl) = "ja") .
    ?s rdfs:label ?l .
    FILTER (lang(?l) = "ja") .
  }
}LIMIT 100
```
クエリを試す　http://tinyurl.com/ycq5jtjh

---------------

# 検索例６：カウントを利用したランキング

## 例６-1) カウントの利用
「大学」のインスタンスの数を取得する
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select (count (?s) AS ?c) where {
  ?s wdt:P31 wd:Q3918.
}
```
クエリを試す　http://tinyurl.com/y9gwnxpm

---------------
## 例6-2) 組み合わせのカウント
「大学」と「卒業生の数」の組み合わせをランキング
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select ?univ ?univl (count(?s) As ?c) where {
  ?univ wdt:P31 wd:Q3918.
  ?s wdt:P69 ?univ.
  OPTIONAL{
    ?univ rdfs:label ?univl .
    FILTER (lang(?univl) = "ja") .
  }
}GROUP BY ?univ ?univl
ORDER BY DESC(?c)
LIMIT 100
```
クエリを試す　http://tinyurl.com/y8gksgw7　

---------------
