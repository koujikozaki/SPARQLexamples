# SPARQLを用いたWikidataの検索演習
WikidataのSPARQLエンドポイント（検索用API）  
https://query.wikidata.org/  　    
を使った，SPARQLクエリの演習問題．  

検索例を試した後，検索例のクエリの「一部を変更」して，いろんなクエリを作成してみる．

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
※複数行ならべると，まとめて目的語を取得できる．(**変数名は変える** )  
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
## 【演習１】主語と述語を指定して「目的語」を取得する
### 演習1-a：「述語」を変えてみる
**「検索例１-1，2, 3」**  の **「述語」** を変えて，「大阪電気通信大学」（主語）のいろんな情報（目的語?o）を取得してみる  
述語のIDは「大阪電気通信大学」の[Wikidataのページ](https://www.wikidata.org/wiki/Q7105556)を見て探す．  
→プロパティにマウスを持っていくと表示される「P○○○」の番号を使えばよい．

### 演習1-b：「主語」を変えてみる
**「検索例１-1，2, 3」**  の **「主語」** を変えて，いろんな主語の情報（目的語?o）を取得してみる  
主語のIDは，探したいデータの「Wikidataのページ」（Wikipediaのページ→「ウィキデータ項目」のリンク）を見て探す．  
→ページ上部の「Q○○○○○○」の番号を使えばよい．

---------------

# 検索例２：述語と目的語を指定して「主語」の一覧を取得
*“<述語>が<目的語>となる<?主語>は？”*  
## 検索例2-1 「位置する行政区」（述語）が「寝屋川市」（目的語）となる「主語（?s）」の一覧を取得する　

```
select ?s ?sLabel
where {
   ?s wdt:P131 wd:Q389633 .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す https://w.wiki/4oh

---------------
## 検索例2-2 「分類」（述語）が「大学」（目的語）となる「主語（?s）」の一覧を取得する　
→「大学」の（インスタンスの）一覧を取得する  
**instance of (P31)** という述語はデータ（インスタンス）， **「分類」** を表し，「同じ種類（クラス）のデータの一覧」を取得するのに利用できる．

```
select ?s ?sLabel
where {
   ?s wdt:P31 wd:Q3918 .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す https://w.wiki/4oj

---------------
## 【演習２】述語と目的語を指定して「主語」の一覧を取得する
### 演習2-a：「目的語」を変えて，いろんな種類（クラス）のデータ一覧を取得してみる
**「検索例2-2」**  の **「目的語」** となるクラスを変える  
クラスのIDは適当なデータの「Wikidataのページ」で，**instance of (P31)の目的語** を調べると良い．  

### 演習2-b：「述語」と「目的語」の組み合わせを変えて，いろんなデータ一覧を取得してみる
**「検索例2-1」**  の **「述語」** や **「目的語」** を変える  

---------------
# 検索例３：「主語」の一覧の「絞り込み」
## 検索例3-1 「大学の一覧（主語）」を「国（述語）」の「目的語(?contry）」と共に取得する　

```
select ?s ?sLabel ?country ?countryLabel
where {
   ?s wdt:P31 wd:Q3918 .
   ?s wdt:P17 ?country .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す https://w.wiki/5kU

---------------
## 検索例3-2 「大学の一覧」を「国（述語）が日本（目的語）」のものに絞り込む　

```
select ?s ?sLabel ?country ?countryLabel
where {
   ?s wdt:P31 wd:Q3918 .
   ?s wdt:P17 ?country .
   ?s wdt:P17 wd:Q17 .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す https://w.wiki/4oo  
  
※**「国が日本」** であることを確認したい場合．
```
select ?s ?sLabel ?country ?countryLabel
where {
   ?s wdt:P31 wd:Q3918 .
   ?s wdt:P17 ?country .
   ?s wdt:P17 wd:Q17 .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す https://w.wiki/5kY

---------------

## 検索例3-3 「日本にある大学の一覧」を「設立日」と共に取得する　

```
select ?s ?sLabel ?o
where {
   ?s wdt:P31 wd:Q3918 . # ?Sの「分類」が「大学」
   ?s wdt:P17 wd:Q17 .   # ?sの「国」が「日本」
   ?s wdt:P571 ?o .      # ?sの「設立」を?oとする
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}
LIMIT 100
```
クエリを試す https://w.wiki/4os

### 検索例3-3-a 「設立日」で並び替え
```
select ?s ?sLabel ?o
where {
   ?s wdt:P31 wd:Q3918 . # ?Sの「分類」が「大学」
   ?s wdt:P17 wd:Q17 .   # ?sの「国」が「日本」
   ?s wdt:P571 ?o .      # ?sの「設立」を?oとする
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
}ORDER BY ?o    # ?o(設立日)で並び替え →DESC(?o)とする「降順」に
LIMIT 100
```
クエリを試す　https://w.wiki/4ou

---------------
## 【演習３】いろんなデータの一覧を取得してみる
### 「目的語」を変えて，さまざまな「絞り込み」を試す
**「検索例3-2」**  の **「述語」と「目的語」の組み合わせ** を変え，様々な条件で絞り込んだ「大学の一覧」を取得する  

### 演習3-b：いろんなデータ一覧を取得する
**「検索例3-1,2,3」**  を変えて，いろんなデータ一覧を取得してみる  
**演習2-aや演習2-b**に，**「主語の条件」を追加**して，データ一覧を絞り込む


---------------
# 補足例
以下，より詳細なクエリを作成するための補足例となります．

----- 
## 補足１：PREFIXを利用しない表現
### PREFIXの定義
上述の例1-1）「大阪電気通信大学」（主語）の「位置する行政区」（述語）となる目的語（?o）を取得する  
において，PREFIXの定義を明記すると下記のようになる.  
※WikidataのEndpointでクエリを発行する際は，これらのWikidataに関するPREFIXの定義は省略可．
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
select ?o
where { 
 wd:Q7105556 wdt:P131 ?o .
}LIMIT 100
```
### PREFIXを利用しない表現
例1-1）をPREFIX（接頭語）を用いず書いた場合は，下記のようになる．
```
select ?o
where { 
   <http://www.wikidata.org/entity/Q7105556> <http://www.wikidata.org/prop/direct/P131> ?o . 
}LIMIT 100

```
---------------
## 補足２：SPARQLの省略表現
省略前
```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select distinct ?p ?o where {
  ?s rdfs:label "大阪"@ja .  
  ?s ?p ?o.
}LIMIT 100
```
省略表現
```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select distinct ?p ?o where {
  ?s rdfs:label "大阪"@ja ;  
     ?p ?o.
}LIMIT 100
```
---------------
## 補足3：ラベルの取得方法【RDF一般】
「大阪電気通信大学」のラベルとなる目的語（?o）を取得  
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select distinct ?o
where { 
   wd:Q7105556 rdfs:label ?o . 
}LIMIT 100
```
クエリを試す　https://w.wiki/646  

------------
「大阪電気通信大学」のラベルとなる目的語（?o）を取得
「言語の種別」を合わせて取得
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select  ?o (lang(?o) AS ?ln)
where { 
   wd:Q7105556 rdfs:label ?o . 
}
```
クエリを試す 　https://w.wiki/648  　 

------------ 
補足例） 「大阪電気通信大学」のラベルとなる目的語（?o）を取得
「言語の種別=日本語（ja）」をのみ
```
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select  ?o (lang(?o) AS ?ln)
where { 
   wd:Q7105556 rdfs:label ?o . 
   FILTER (lang(?o) = "ja") .
}
```
クエリを試す https://w.wiki/64A  

--------------- 
## 補足4：カウントの利用  
補足例） 「大学」のインスタンスの数を取得する  
```
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX wd: <http://www.wikidata.org/entity/>

select (count (?s) AS ?c) where { 
  ?s wdt:P31 wd:Q3918. 
}
```
クエリを試す　https://w.wiki/64B 

---------------
## 補足5：グループ化の利用  
補足例） 「大学の一覧（主語）」を「国（述語）」の「目的語（?country）とそのラベル」と共に取得し,「国ごとのインスタンス数」を取得する
```
select ?country ?countryLabel (count(?s) As ?c)
where {
   ?s wdt:P31 wd:Q3918 .
   ?s wdt:P17 ?country .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "ja". }
} GROUP BY ?country ?countryLabel
```
クエリを試す　https://w.wiki/64D  

補足例） 「大学の一覧（主語）」を「国（述語）」の「目的語（?country）とそのラベル」と共に取得し，「国ごとのインスタンス数」を取得し，多い順にソート．
```
select ?country ?countryLabel (count(?s) As ?c)
where {
   ?s wdt:P31 wd:Q3918 .
   ?s wdt:P17 ?country .
  SERVICE wikibase:label {
     bd:serviceParam wikibase:language "ja". }
} GROUP BY ?country ?countryLabel
ORDER BY DESC (?c)
```
クエリを試す https://w.wiki/64E  

---------------
## 補足６：クラス階層を考慮したクエリ例
例）「漫画家」を「国籍」毎にランキングする  
`/wdt:P279*`を“入れる/入れない”で結果が変わる

```
select ?o ?oLabel (count(?s) As ?c) 
where { 
    ?s  wdt:P106/wdt:P279*   wd:Q715301 .
    ?s  wdt:P27  ?o .
SERVICE wikibase:label { 
bd:serviceParam wikibase:language "[AUTO_LANGUAGE],ja". }
} GROUP BY ?o ?oLabel
ORDER BY DESC(?c)
```
クエリを試す　https://w.wiki/Dqt  

---------------

# 参考リンク集
## より複雑なクエリの例
- Wikidataを使った日本の政治家の出身大学ランキング  
http://bit.ly/2PBt8fn  
- Wikidataを使って鉄道会社ごとの総路線長をランキングしてみる  
https://qiita.com/RK-miha/items/6d94f425871c4e9f5f73
- Wikidat公式サイトのサンプルクエリ集
https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples

## SPARQL 1.1 Query Language (W3C Recommendation)
https://www.w3.org/TR/sparql11-query/   
→日本語訳　http://www.asahi-net.or.jp/~ax2s-kmtn/internet/rdf/REC-sparql11-query-20130321.html

## Wikidataのプロパティ一覧（抜粋）
https://www.wikidata.org/wiki/Wikidata:List_of_properties

