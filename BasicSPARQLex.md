# 基本的なクエリ例
## すべてのトリプルを取得（１）
```
select *
where{
  ?S ?p ?o
}
LIMIT 100
```
## ある主語がもつ述語＋目的語を取得 
```
select *
where{
  <（１）で得た主語のURI> ?p ?o
}
LIMIT 100
```
## ある目的語をもつ主語＋述語を取得
```
select *
where{
  ?s ?p  <（１）で得た目的語のURI>
}
LIMIT 100
```
## 述語の一覧を取得
```
select DISTINCT ?p
where{
  ?s ?p ?o
}
LIMIT 100
```
## ラベルの一覧を取得
```
select ?o
where{
  ?s ?p ?o .
  FILTER(isLiteral(?o))
}
LIMIT 100
```
### 日本語ラベルに限定（重い場合あり？）
```
select ?o
where{
  ?s ?p ?o .
  FILTER(isLiteral(?o))
  FILTER(lang(?o)="ja")
}
LIMIT 100
```
## クラスの一覧を取得（２）
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

select DISTINCT ?o
where{
  ?s rdf:type ?o . 
}
LIMIT 100
```
または
```
select DISTINCT ?o
where{
  ?s a ?o . 
}
LIMIT 100
```
※「a」は「rdf:type」の省略表現．
## あるクラスのインスタンス一覧を取得
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

select DISTINCT ?s
where{
  ?s rdf:type <（２）で得たクラスのURI> . 
}
LIMIT 100
```
または
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

select DISTINCT ?s
where{
  ?s a <（２）で得たクラスのURI> . 
}
LIMIT 100
```
