
# 必要なリンク集
## SPARQL 1.1 Query Language (W3C Recommendation)
https://www.w3.org/TR/sparql11-query/   
→日本語訳　http://www.asahi-net.or.jp/~ax2s-kmtn/internet/rdf/REC-sparql11-query-20130321.html

## WikidataのSPARQLエンドポイント（検索用API）
https://query.wikidata.org/      
※プログラムからアクセスする場合は，こちら https://query.wikidata.org/sparql  　

## Wikidataのプロパティ一覧（抜粋）
https://www.wikidata.org/wiki/Wikidata:List_of_properties

---

# 関心のあるリソースのIRIを探す

例）大阪電気通信大学のWikidata上でのIRI　　

[http://www.wikidata.org/entity/Q7105556](http://www.wikidata.org/entity/Q7105556)


---------------
# 検索例１：主語と述語を指定して「目的語」を取得する
「大阪電気通信大学」（主語）の「位置する行政区」（述語）となる目的語（?o）を取得する　

```
select ?o
where {
   wd:Q7105556 wdt:P131 ?o .
}
```
クエリを試す https://w.wiki/4oY

---------------
## 検索例１-1：主語と述語を指定して「目的語」を取得する【目的語がリテラルの場合】
「大阪電気通信大学」（主語）の「設立」（述語）となる目的語（?o）を取得する　

```
select ?o
where {
   wd:Q7105556 wdt:P571 ?o .
}
```
クエリを試す　https://w.wiki/4od

---------------
## 検索例１-2：主語と述語を指定して「目的語」を取得する【ラベルも取得】
「大阪電気通信大学」（主語）の「位置する行政区」（述語）となる目的語（?o）を取得する　

```
select ?o ?oLabel
where {
   wd:Q7105556 wdt:P131 ?o .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
```
クエリを試す　https://w.wiki/4oZ


---------------
## 検索例１-3：複数の目的語をまとめて取得する
※複数行，慣らべると，まとめて目的語を取得できる．(**変数名は変える** )  
「大阪電気通信大学」（主語）の「位置する行政区」（述語）となる目的語（?o），および「設立」（述語）となる目的語（?o2）を取得する　


```
select ?o ?oLabel ?o2
where {
   wd:Q7105556 wdt:P131 ?o .
   wd:Q7105556 wdt:P571 ?o2 .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
```
クエリを試す　https://w.wiki/4of

---------------
# 【演習１】主語と述語を指定して「目的語」を取得する
## 演習1-a：「述語」を変えてみる
**「検索例１-1，2, 3」**  の **「述語」** を変えて，「大阪電気通信大学」（主語）のいろんな情報（目的語?o）を取得してみる  
述語のIDは「大阪電気通信大学」の[Wikidataのページ](https://www.wikidata.org/wiki/Q7105556)を見て探す．  
→プロパティにマウスを持っていくと表示される「P○○○」の番号を使えばよい．

## 演習1-b：「主語」を変えてみる
**「検索例１-1，2, 3」**  の **「主語」** を変えて，いろんな主語の情報（目的語?o）を取得してみる  
主語のIDは，探したいデータの「Wikidataのページ」（Wikipediaのページ→「ウィキデータ項目」のリンク）を見て探す．  
→ページ上部の「Q○○○○○○」の番号を使えばよい．

---------------




