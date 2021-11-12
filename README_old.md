# Wikidataを例としたSPARQLの基本クエリ例
---
[Wikidataのクエリサービス](https://query.wikidata.org/)を例とした，基本的なSPARQLクエリの書き方の例を解説しています．  

## 必要なリンク集
### WikidataのSPARQLエンドポイント（検索用API）
https://query.wikidata.org/      
※プログラムからアクセスする場合は，こちら https://query.wikidata.org/sparql  　

## 参考資料
### Wikidataを使った日本の政治家の出身大学ランキング
https://qiita.com/koujikozaki/items/a049e2ac1051e0e43be6

### Wikidataのプロパティ一覧（抜粋）
https://www.wikidata.org/wiki/Wikidata:List_of_properties

### SPARQL 1.1 Query Language (W3C Recommendation)
https://www.w3.org/TR/sparql11-query/   
→日本語訳　http://www.asahi-net.or.jp/~ax2s-kmtn/internet/rdf/REC-sparql11-query-20130321.html

### PREFIXの検索サービス
https://prefix.cc/

---

# 関心のあるリソースのIRI(ID)を探す

例）大阪大学のWikidata上でのIRI　　
[http://www.wikidata.org/entity/Q651233](http://www.wikidata.org/entity/Q651233)

---
# 検索例１：主語と述語を指定
## 例1-1）「大阪大学」の「設立年」となる目的語（?o）を取得

```
select ?o
where {
   wd:Q651233 wdt:P571 ?o .
}LIMIT 100
```
クエリを試す　http://tinyurl.com/y9exqqgg


## 例1-2）「大阪大学」の「本部所在地」となる目的語（?o）を取得

```
select ?o
where {
   wd:Q651233 wdt:P159 ?o .
}
```
クエリを試す　http://tinyurl.com/yd9qggrx

## 例1-3）「大阪大学」の「本部所在地」となる目的語（?o）を取得  
※検索結果がデータのIDとなる場合，下記の記述を追加することで「ラベル」をあわせて取得可能

```
select ?o ?oLabel
where {
   wd:Q651233 wdt:P159 ?o .
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
```
クエリを試す　http://tinyurl.com/ydbuq5ou

---
# 検索例2：複数の述語を指定して，目的語を取得する
## 例2-1：「大阪大学」の“本部所在地”と“創立日”取得する
```
select ?o1 ?o1Label ?o2
where {
  wd:Q651233 wdt:P159 ?o1.
  wd:Q651233 wdt:P571 ?o2.
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
```
クエリを試す　http://tinyurl.com/ydgpgftp

## 例2-2）「大阪大学」の“本部所在地”の“行政区（何県にあるか？）”を取得する
```
select ?o1 ?o1Label ?o2 ?o2Label
where {
  wd:Q651233 wdt:P159 ?o1.
  ?o1 wdt:P131 ?o2.
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
```
クエリを試す　http://tinyurl.com/yd5d5bfd
---
# 検索例3：述語と目的語を指定して「主語」を取得
## 例）3-1：「国が“日本”」となる主語（?s）を取得
```
select ?s ?sLabel
where {
 ?s wdt:P17 wd:Q17 .
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す　http://tinyurl.com/ycddr5mf

## 例）3-2：「分類が“大学”」となる主語（?s）を取得
※分類（incetance-of）を使うと同じ種類のデータ一覧が取得できる

```
select ?s ?sLabel
where {
 ?s wdt:P31 wd:Q3918 .
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す　http://tinyurl.com/yavwjgt2
---
# 検索例４：「主語」と「目的語」の組み合わせ
## 例）4-1：「本部所在地」の「主語」と「目的語」の組み合わせ取得する
```
select ?s ?sLabel ?o ?oLabel
where {
  ?s wdt:P159 ?o.
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す　http://tinyurl.com/y8j8ocv5

## 例）4-2：「本部所在地」の「主語」と「目的語」の組み合わせのうち，「主語が大学である」ものを取得する
```
select ?s ?sLabel ?o ?oLabel
where {
  ?s  wdt:P31  wd:Q3918 .
  ?s  wdt:P159 ?o .
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す　http://tinyurl.com/yczvh3n3

## 例）4-3：「大学」と「本部所在地」の「行政区」の組み合わせを取得する
```
select ?s ?sLabel ?o2 ?o2Label
where {
  ?s  wdt:P31  wd:Q3918 .
  ?s  wdt:P159 ?o .
  ?o  wdt:P131 ?o2.
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}LIMIT 100
```
クエリを試す　http://tinyurl.com/yajosxus

## 例）4-4：「本部所在地」の「主語」と「目的語」の組み合わせのうち，「目的語の国が日本である」ものを取得
```
select ?s ?sLabel ?o ?oLabel
where {
  ?s  wdt:P159 ?o .
  ?o  wdt:P17  wd:Q17 .
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}LIMIT 100
```
クエリを試す　http://tinyurl.com/y77vgwkq

---
# 検索例５：カウントを利用したランキング
## 例）5-1：「大学」のインスタンスの数を取得する
```
select (count (?s) AS ?c) where {
  ?s wdt:P31 wd:Q3918.  
}
```
クエリを試す　http://tinyurl.com/y7dqndru

## 例）5-2：「本部所在地」別の「大学数」をランキング
```
select ?o ?oLabel (count(?s) As ?c)
where {
 ?s  wdt:P31   wd:Q3918 .
 ?s  wdt:P159  ?o .
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
} GROUP BY ?o ?oLabel
ORDER BY DESC(?c)
```
クエリを試す　http://tinyurl.com/ycqdwhdq

## 例）5-3：「本部所在地」の「行政区」別の「大学数」をランキング
```
select ?o2 ?o2Label (count(?s) As ?c)
where {
  ?s  wdt:P31  wd:Q3918 .
  ?s  wdt:P159 ?o .
  ?o  wdt:P131 ?o2.
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
} GROUP BY ?o2 ?o2Label
ORDER BY DESC(?c)
```
クエリを試す　http://tinyurl.com/ycy2zagg
