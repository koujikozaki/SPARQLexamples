# SampleKG SPARQLクエリ例

人物相関図から作成した[RDFデータ `SampleKG.ttl`](KG/SampleKG.ttl) を使って、SPARQLの基本的な使い方を確認するためのサンプルクエリです。

サンプルのRDFデータは、[TSV2KG Editor](https://github.com/oecu-kozaki-lab/TSV2KG-Editor)を用いて作成しました。  
→[サンプルをTSV2KG Editorで開く](https://oecu-kozaki-lab.github.io/TSV2KG-Editor/?tsv=https://raw.githubusercontent.com/koujikozaki/SPARQLexamples/refs/heads/master/KG/SampleKG.txt&view=1)

## 共通PREFIX

各クエリの先頭に、以下のPREFIXを付けて実行します。

```sparql
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ex:   <http://example.com/>
PREFIX prop: <http://example.com/prop/>
```

## 例1: すべてのトリプルを取得する

まず、RDFデータにどのようなトリプルが入っているかを確認します。

```sparql
SELECT ?s ?p ?o
WHERE {
  ?s ?p ?o .
}
LIMIT 50
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3Fs%20%3Fp%20%3Fo%0AWHERE%20%7B%0A%20%20%3Fs%20%3Fp%20%3Fo%20.%0A%7D%0ALIMIT%2050%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B1%20%E3%81%99%E3%81%B9%E3%81%A6%E3%81%AE%E3%83%88%E3%83%AA%E3%83%97%E3%83%AB%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例2: リソースとラベルの一覧を取得する

`rdfs:label` を使って、IRIと人間が読みやすい名前の対応を確認します。

```sparql
SELECT ?x ?label
WHERE {
  ?x rdfs:label ?label .
}
ORDER BY ?x
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3Fx%20%3Flabel%0AWHERE%20%7B%0A%20%20%3Fx%20rdfs%3Alabel%20%3Flabel%20.%0A%7D%0AORDER%20BY%20%3Fx%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B2%20%E3%83%AA%E3%82%BD%E3%83%BC%E3%82%B9%E3%81%A8%E3%83%A9%E3%83%99%E3%83%AB%E3%81%AE%E4%B8%80%E8%A6%A7%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例3: 主語と述語を指定して目的語を取得する

例: `ex:1` の指導教員を取得します。

```sparql
SELECT ?teacher ?teacherLabel
WHERE {
  ex:1 prop:supervisor ?teacher .
  ?teacher rdfs:label ?teacherLabel .
}
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3Fteacher%20%3FteacherLabel%0AWHERE%20%7B%0A%20%20ex%3A1%20prop%3Asupervisor%20%3Fteacher%20.%0A%20%20%3Fteacher%20rdfs%3Alabel%20%3FteacherLabel%20.%0A%7D%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B3%20%E4%B8%BB%E8%AA%9E%E3%81%A8%E8%BF%B0%E8%AA%9E%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%97%E3%81%A6%E7%9B%AE%E7%9A%84%E8%AA%9E%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

この例では、`ex:1` というIRIと、`prop:supervisor` という述語をあらかじめ知っているものとして検索しています。

## 例4: プロパティの一覧を取得する

例3では `prop:supervisor` を直接指定しました。では、このRDFデータで使えるプロパティには何があるでしょうか。  
`rdf:type rdf:Property` として定義されているリソースを取得します。

```sparql
SELECT ?property ?propertyLabel
WHERE {
  ?property rdf:type rdf:Property .
  OPTIONAL {
    ?property rdfs:label ?propertyLabel .
  }
}
ORDER BY ?property
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3Fproperty%20%3FpropertyLabel%0AWHERE%20%7B%0A%20%20%3Fproperty%20rdf%3Atype%20rdf%3AProperty%20.%0A%20%20OPTIONAL%20%7B%0A%20%20%20%20%3Fproperty%20rdfs%3Alabel%20%3FpropertyLabel%20.%0A%20%20%7D%0A%7D%0AORDER%20BY%20%3Fproperty%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B4%20%E3%83%97%E3%83%AD%E3%83%91%E3%83%86%E3%82%A3%E3%81%AE%E4%B8%80%E8%A6%A7%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例5: ラベルで主語を探してから目的語を取得する

実際には `ex:1` のようなIRIを覚えているとは限りません。  
そこで、まずラベル `"太郎"` を持つリソースを探し、そのリソースの指導教員を取得します。

```sparql
SELECT ?student ?teacher ?teacherLabel
WHERE {
  ?student rdfs:label "太郎"@ja .
  ?student prop:supervisor ?teacher .
  ?teacher rdfs:label ?teacherLabel .
}
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3Fstudent%20%3Fteacher%20%3FteacherLabel%0AWHERE%20%7B%0A%20%20%3Fstudent%20rdfs%3Alabel%20%22%E5%A4%AA%E9%83%8E%22%40ja%20.%0A%20%20%3Fstudent%20prop%3Asupervisor%20%3Fteacher%20.%0A%20%20%3Fteacher%20rdfs%3Alabel%20%3FteacherLabel%20.%0A%7D%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B5%20%E3%83%A9%E3%83%99%E3%83%AB%E3%81%A7%E4%B8%BB%E8%AA%9E%E3%82%92%E6%8E%A2%E3%81%97%E3%81%A6%E3%81%8B%E3%82%89%E7%9B%AE%E7%9A%84%E8%AA%9E%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

このように、ラベルから対象リソースを探して、そのリソースを主語として次の条件につなげることができます。

## 例6: 述語と目的語を指定して主語を取得する

例: `ex:7` を指導教員にもつ学生を取得します。

```sparql
SELECT ?student ?studentLabel
WHERE {
  ?student prop:supervisor ex:7 .
  ?student rdfs:label ?studentLabel .
}
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3Fstudent%20%3FstudentLabel%0AWHERE%20%7B%0A%20%20%3Fstudent%20prop%3Asupervisor%20ex%3A7%20.%0A%20%20%3Fstudent%20rdfs%3Alabel%20%3FstudentLabel%20.%0A%7D%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B6%20%E8%BF%B0%E8%AA%9E%E3%81%A8%E7%9B%AE%E7%9A%84%E8%AA%9E%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%97%E3%81%A6%E4%B8%BB%E8%AA%9E%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例7: ある人物の複数の情報を取得する

例: `"太郎"` の所属、研究テーマ、興味をまとめて取得します。

```sparql
SELECT ?labLabel ?themeLabel ?interestLabel
WHERE {
  ?person rdfs:label "太郎"@ja .

  ?person prop:affiliation ?lab .
  ?person prop:researchTheme ?theme .
  ?person prop:interest ?interest .

  ?lab rdfs:label ?labLabel .
  ?theme rdfs:label ?themeLabel .
  ?interest rdfs:label ?interestLabel .
}
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3FlabLabel%20%3FthemeLabel%20%3FinterestLabel%0AWHERE%20%7B%0A%20%20%3Fperson%20rdfs%3Alabel%20%22%E5%A4%AA%E9%83%8E%22%40ja%20.%0A%0A%20%20%3Fperson%20prop%3Aaffiliation%20%3Flab%20.%0A%20%20%3Fperson%20prop%3AresearchTheme%20%3Ftheme%20.%0A%20%20%3Fperson%20prop%3Ainterest%20%3Finterest%20.%0A%0A%20%20%3Flab%20rdfs%3Alabel%20%3FlabLabel%20.%0A%20%20%3Ftheme%20rdfs%3Alabel%20%3FthemeLabel%20.%0A%20%20%3Finterest%20rdfs%3Alabel%20%3FinterestLabel%20.%0A%7D%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B7%20%E3%81%82%E3%82%8B%E4%BA%BA%E7%89%A9%E3%81%AE%E8%A4%87%E6%95%B0%E3%81%AE%E6%83%85%E5%A0%B1%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例8: 所属研究室ごとに学生を取得する

「誰がどの研究室に所属しているか」を取得します。

```sparql
SELECT ?studentLabel ?labLabel
WHERE {
  ?student prop:affiliation ?lab .

  ?student rdfs:label ?studentLabel .
  ?lab rdfs:label ?labLabel .
}
ORDER BY ?labLabel ?studentLabel
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3FstudentLabel%20%3FlabLabel%0AWHERE%20%7B%0A%20%20%3Fstudent%20prop%3Aaffiliation%20%3Flab%20.%0A%0A%20%20%3Fstudent%20rdfs%3Alabel%20%3FstudentLabel%20.%0A%20%20%3Flab%20rdfs%3Alabel%20%3FlabLabel%20.%0A%7D%0AORDER%20BY%20%3FlabLabel%20%3FstudentLabel%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B8%20%E6%89%80%E5%B1%9E%E7%A0%94%E7%A9%B6%E5%AE%A4%E3%81%94%E3%81%A8%E3%81%AB%E5%AD%A6%E7%94%9F%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例9: 同じ研究室に所属する人物の組を取得する

`研究室メンバー` という関係を直接持たせず、`所属` から同じ研究室の人物を検索する例です。

```sparql
SELECT ?person1Label ?person2Label ?labLabel
WHERE {
  ?person1 prop:affiliation ?lab .
  ?person2 prop:affiliation ?lab .

  FILTER(?person1 != ?person2)

  ?person1 rdfs:label ?person1Label .
  ?person2 rdfs:label ?person2Label .
  ?lab rdfs:label ?labLabel .
}
ORDER BY ?labLabel ?person1Label ?person2Label
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3Fperson1Label%20%3Fperson2Label%20%3FlabLabel%0AWHERE%20%7B%0A%20%20%3Fperson1%20prop%3Aaffiliation%20%3Flab%20.%0A%20%20%3Fperson2%20prop%3Aaffiliation%20%3Flab%20.%0A%0A%20%20FILTER(%3Fperson1%20!%3D%20%3Fperson2)%0A%0A%20%20%3Fperson1%20rdfs%3Alabel%20%3Fperson1Label%20.%0A%20%20%3Fperson2%20rdfs%3Alabel%20%3Fperson2Label%20.%0A%20%20%3Flab%20rdfs%3Alabel%20%3FlabLabel%20.%0A%7D%0AORDER%20BY%20%3FlabLabel%20%3Fperson1Label%20%3Fperson2Label%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B9%20%E5%90%8C%E3%81%98%E7%A0%94%E7%A9%B6%E5%AE%A4%E3%81%AB%E6%89%80%E5%B1%9E%E3%81%99%E3%82%8B%E4%BA%BA%E7%89%A9%E3%81%AE%E7%B5%84%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例10: 研究室と担当教授を取得する

研究室と、その担当教授を取得します。

```sparql
SELECT ?labLabel ?professorLabel
WHERE {
  ?lab prop:professorInCharge ?professor .

  ?lab rdfs:label ?labLabel .
  ?professor rdfs:label ?professorLabel .
}
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3FlabLabel%20%3FprofessorLabel%0AWHERE%20%7B%0A%20%20%3Flab%20prop%3AprofessorInCharge%20%3Fprofessor%20.%0A%0A%20%20%3Flab%20rdfs%3Alabel%20%3FlabLabel%20.%0A%20%20%3Fprofessor%20rdfs%3Alabel%20%3FprofessorLabel%20.%0A%7D%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B10%20%E7%A0%94%E7%A9%B6%E5%AE%A4%E3%81%A8%E6%8B%85%E5%BD%93%E6%95%99%E6%8E%88%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例11: 講義、担当教員、履修学生を取得する

講義を担当している教員と、その講義を履修している学生を取得します。

```sparql
SELECT ?courseLabel ?teacherLabel ?studentLabel
WHERE {
  ?teacher prop:inCharge ?course .
  ?student prop:courseCompletion ?course .

  ?course rdfs:label ?courseLabel .
  ?teacher rdfs:label ?teacherLabel .
  ?student rdfs:label ?studentLabel .
}
ORDER BY ?courseLabel ?studentLabel
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3FcourseLabel%20%3FteacherLabel%20%3FstudentLabel%0AWHERE%20%7B%0A%20%20%3Fteacher%20prop%3AinCharge%20%3Fcourse%20.%0A%20%20%3Fstudent%20prop%3AcourseCompletion%20%3Fcourse%20.%0A%0A%20%20%3Fcourse%20rdfs%3Alabel%20%3FcourseLabel%20.%0A%20%20%3Fteacher%20rdfs%3Alabel%20%3FteacherLabel%20.%0A%20%20%3Fstudent%20rdfs%3Alabel%20%3FstudentLabel%20.%0A%7D%0AORDER%20BY%20%3FcourseLabel%20%3FstudentLabel%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B11%20%E8%AC%9B%E7%BE%A9%E3%80%81%E6%8B%85%E5%BD%93%E6%95%99%E5%93%A1%E3%80%81%E5%B1%A5%E4%BF%AE%E5%AD%A6%E7%94%9F%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例12: OPTIONALを使って、あれば興味も取得する

`OPTIONAL` を使うと、指定した情報が存在する場合だけ追加で取得できます。

```sparql
SELECT ?personLabel ?themeLabel ?interestLabel
WHERE {
  ?person prop:researchTheme ?theme .
  ?person rdfs:label ?personLabel .
  ?theme rdfs:label ?themeLabel .

  OPTIONAL {
    ?person prop:interest ?interest .
    ?interest rdfs:label ?interestLabel .
  }
}
ORDER BY ?personLabel
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3FpersonLabel%20%3FthemeLabel%20%3FinterestLabel%0AWHERE%20%7B%0A%20%20%3Fperson%20prop%3AresearchTheme%20%3Ftheme%20.%0A%20%20%3Fperson%20rdfs%3Alabel%20%3FpersonLabel%20.%0A%20%20%3Ftheme%20rdfs%3Alabel%20%3FthemeLabel%20.%0A%0A%20%20OPTIONAL%20%7B%0A%20%20%20%20%3Fperson%20prop%3Ainterest%20%3Finterest%20.%0A%20%20%20%20%3Finterest%20rdfs%3Alabel%20%3FinterestLabel%20.%0A%20%20%7D%0A%7D%0AORDER%20BY%20%3FpersonLabel%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B12%20OPTIONAL%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%80%81%E3%81%82%E3%82%8C%E3%81%B0%E8%88%88%E5%91%B3%E3%82%82%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 例13: 集計する

研究室ごとに、所属している人物数を数えます。

```sparql
SELECT ?labLabel (COUNT(?person) AS ?count)
WHERE {
  ?person prop:affiliation ?lab .
  ?lab rdfs:label ?labLabel .
}
GROUP BY ?labLabel
ORDER BY DESC(?count)
```

[YASGUIで実行](https://yasgui.triply.cc/#query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20ex%3A%20%20%20%3Chttp%3A%2F%2Fexample.com%2F%3E%0APREFIX%20prop%3A%20%3Chttp%3A%2F%2Fexample.com%2Fprop%2F%3E%0A%0ASELECT%20%3FlabLabel%20(COUNT(%3Fperson)%20AS%20%3Fcount)%0AWHERE%20%7B%0A%20%20%3Fperson%20prop%3Aaffiliation%20%3Flab%20.%0A%20%20%3Flab%20rdfs%3Alabel%20%3FlabLabel%20.%0A%7D%0AGROUP%20BY%20%3FlabLabel%0AORDER%20BY%20DESC(%3Fcount)%0A&endpoint=https%3A%2F%2Flod.hozo.jp%2Fkz-fuseki%2FMIclass2026%2Fsparql&requestMethod=POST&tabTitle=%E4%BE%8B13%20%E9%9B%86%E8%A8%88%E3%81%99%E3%82%8B&headers=%7B%7D&contentTypeConstruct=application%2Fn-triples%2C*%2F*%3Bq%3D0.9&contentTypeSelect=application%2Fsparql-results%2Bjson%2C*%2F*%3Bq%3D0.9&outputFormat=table)

## 演習案

以下のクエリを自分で作ってみます。

1. ある学生の指導教員と研究テーマを取得する。
2. ある教員が指導している学生を取得する。
3. ある講義を履修している学生を取得する。
4. ある研究室に所属している学生を取得する。
5. 同じ研究室に所属している学生の組を取得する。
6. 各自が作成した人物相関図RDFで、同じ形式のクエリを実行する。

