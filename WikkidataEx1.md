
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

---

# 検索例１：主語と述語を指定して「目的語」を取得する
「大阪電気通信大学」（主語）の「位置する行政区」（述語）となる目的語（?o）を取得する　

```
select ?o
where {
   wd:Q7105556 wdt:P131 ?o .
}
```
クエリを試す　https://w.wiki/4oY

---------------
## 検索例１+：主語と述語を指定して「目的語」を取得する【ラベルも取得】
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


